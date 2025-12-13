Here’s how you can **use Spring WebFlux to asynchronously and non-blockingly serve a large file stored locally** from a reactive controller. Unlike a traditional Spring MVC controller that would block on file I/O, WebFlux works with **reactive types** (`Mono`, `Flux`) and streams the file content reactively so that threads aren’t blocked waiting on disk I/O. ([Baeldung on Kotlin][1])

---

## ✅ Key Concepts

**Non-blocking I/O with WebFlux**

* WebFlux uses **Reactor** types (`Mono`, `Flux`) to represent asynchronous streams of data.
* A large file can be emitted as a `Flux<DataBuffer>`, allowing the server to stream chunks of the file reactively instead of loading it all into memory.
* WebFlux will write chunks to the HTTP response as they become available. ([Home][2])

**Serving files reactively**

* You can expose a GET endpoint that returns the file contents using `Flux<DataBuffer>`.
* No thread is blocked waiting for the whole file; backpressure is supported so the client can receive at its own pace.

---

## 🧠 Approach 1 — Using `Resource` and `DataBufferUtils`

Spring’s `Resource` abstraction (e.g., `FileSystemResource`) points to your file on disk. You then use `DataBufferUtils` to convert it to a `Flux<DataBuffer>` that the HTTP layer sends out reactively.

### Example (Annotated Controller)

```java
import org.springframework.core.io.FileSystemResource;
import org.springframework.core.io.Resource;
import org.springframework.http.MediaType;
import org.springframework.http.server.reactive.ServerHttpResponse;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import reactor.core.publisher.Mono;
import org.springframework.core.io.buffer.DataBuffer;
import org.springframework.core.io.buffer.DataBufferUtils;
import reactor.core.publisher.Flux;

import java.nio.file.Path;

@Controller
public class FileReactiveController {

    @GetMapping(value = "/files/{filename}", produces = MediaType.APPLICATION_OCTET_STREAM_VALUE)
    public Mono<Void> serveFile(
            @PathVariable String filename,
            ServerHttpResponse response
    ) {
        // Path to the file on local disk
        Path filePath = Path.of("/path/to/your/files", filename);

        // Wrap as a Resource
        Resource fileResource = new FileSystemResource(filePath);

        // Convert the resource into a Flux<DataBuffer>
        Flux<DataBuffer> fileStream = DataBufferUtils.read(
                fileResource,  // the resource to read
                response.bufferFactory(), // buffer factory from the response
                4096  // buffer size
        );

        // Set headers non-blocking and write the body as the stream
        return response.writeWith(fileStream);
    }
}
```

### Notes

* `DataBufferUtils.read(...)` creates a reactive stream of buffer chunks from disk.
* `response.writeWith(...)` **streams** those chunks back to the client.
* It doesn’t load the full file into memory.
* Spring WebFlux handles backpressure and chunked transfer automatically because you return a stream. ([Baeldung on Kotlin][1])

---

## 🧠 Approach 2 — Return `ResponseEntity<Flux<DataBuffer>>`

This lets you control headers and status more explicitly:

```java
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.http.HttpHeaders;
import org.springframework.core.io.buffer.DataBuffer;
import org.springframework.core.io.buffer.DataBufferUtils;
import org.springframework.core.io.FileSystemResource;
import org.springframework.core.io.Resource;

@GetMapping(value = "/download/{filename}")
public Mono<ResponseEntity<Flux<DataBuffer>>> downloadLargeFile(
        @PathVariable String filename
) {
    Path filePath = Path.of("/path/to/your/files", filename);
    Resource resource = new FileSystemResource(filePath);

    Flux<DataBuffer> stream = DataBufferUtils.read(
            resource,
            // Default buffer factory
            DefaultDataBufferFactory.sharedInstance,
            4096
    );

    return Mono.just(
            ResponseEntity.ok()
                    .header(HttpHeaders.CONTENT_DISPOSITION,
                            "attachment; filename=\"" + filename + "\"")
                    .contentType(MediaType.APPLICATION_OCTET_STREAM)
                    .body(stream)
    );
}
```

### What’s happening here

* You *return immediately* with a reactive body (`Flux<DataBuffer>`).
* Spring rewrites the HTTP response reactively as chunks become available, keeping threads free to serve other requests. ([Home][2])

---

## 📌 Things to Consider

### Back-pressure support

WebFlux’s reactive streaming honors backpressure — the client controls how quickly it reads chunks, and the server adapts accordingly. This is essential for scaling and handling large files without overwhelming memory. ([Home][2])

### Do **not** block on file I/O

If you read files using blocking APIs (e.g., traditional `InputStream` read loops), you may block the event-loop thread and defeat the purpose of reactive execution. Using `DataBufferUtils.read()` avoids this. ([Baeldung on Kotlin][1])

### Static files vs dynamic

If files are truly static and/or in your classpath (like images or public assets), Spring Boot can serve them automatically from `/static`, `/public` without a controller — but for large local files outside those default locations, the reactive controller approach above is ideal. ([Baeldung on Kotlin][1])

---

## 🧪 Testing with curl

You can test your endpoint with:

```bash
curl -O http://localhost:8080/files/yourLargeFile.zip
```

This will stream the download without loading the entire file in memory.

---

If you want, I can also show how to **upload large files reactively** the same way — let me know!

[1]: https://www.baeldung.com/spring-webflux-static-content?utm_source=chatgpt.com "Static Content in Spring WebFlux | Baeldung"
[2]: https://docs.spring.io/spring/docs/5.1.x/spring-framework-reference/web-reactive.html?utm_source=chatgpt.com "Web on Reactive Stack"

Here’s how to implement **reactive, non-blocking large file uploads in Spring WebFlux**, building on the download example you already have — so your server can receive large files without loading everything into memory and while staying fully asynchronous.

---

## 🧠 Key Concepts (WebFlux Upload)

In WebFlux, file upload handling is built on **reactive streams**:

* Each uploaded file part is represented as a `FilePart`, which exposes the file content as reactive chunks (`Flux<DataBuffer>`).
* You write those chunks to disk reactively — no blocking I/O, no whole-file buffering. ([Baeldung on Kotlin][1])

Spring WebFlux handles multipart data streaming and backpressure automatically when using reactive types like `Mono<FilePart>` or `Flux<FilePart>` in your controller. ([Baeldung on Kotlin][1])

---

## ✅ Simple Reactive Upload Controller

### Upload a **single large file**

```java
import org.springframework.http.MediaType;
import org.springframework.http.codec.multipart.FilePart;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestPart;
import org.springframework.web.bind.annotation.RestController;
import reactor.core.publisher.Mono;

import java.nio.file.Path;
import java.nio.file.Paths;

@RestController
public class ReactiveUploadController {

    private final Path uploadDir = Paths.get("uploads");

    @PostMapping(value = "/upload/single", consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
    public Mono<String> uploadSingleFile(
            @RequestPart("file") Mono<FilePart> filePartMono
    ) {
        return filePartMono.flatMap(filePart -> {
            Path destination = uploadDir.resolve(filePart.filename());
            return filePart.transferTo(destination)
                    .then(Mono.just("Upload successful: " + filePart.filename()));
        });
    }
}
```

💡 **What’s happening**

* Spring extracts the uploaded file as a `Mono<FilePart>`.
* `FilePart.transferTo(Path)` streams file chunks to disk **non-blocking** and returns a `Mono<Void>` when done. ([Baeldung on Kotlin][1])

---

## 📤 Upload **multiple large files**

If you want to support uploading multiple files in one request:

```java
@PostMapping(value = "/upload/multiple", consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
public Mono<String> uploadMultipleFiles(
        @RequestPart("files") Flux<FilePart> fileParts
) {
    return fileParts
            .flatMap(filePart -> {
                Path destination = uploadDir.resolve(filePart.filename());
                return filePart.transferTo(destination);
            })
            .then(Mono.just("All files uploaded successfully!"));
}
```

✔ Because `fileParts` is a `Flux<FilePart>`, each file is streamed one by one (or in parallel depending on buffering/dispatcher) without blocking threads or loading entire files into memory. ([Baeldung on Kotlin][1])

---

## 🛠 Required Configuration (Optional but Useful)

### Increase disk buffer limits for large uploads

By default WebFlux may buffer parts up to a certain size before writing to disk. You can adjust these settings in **`application.yml`**:

```yaml
spring:
  webflux:
    multipart:
      max-disk-usage-per-part: 50MB
      max-in-memory-size: 1MB
```

* `max-disk-usage-per-part`: threshold before switching to disk for each part.
* `max-in-memory-size`: total in-memory buffer across all parts.
  These help prevent memory issues with large files. ([Runebook][2])

---

## 📌 Notes & Best Practices

### ❗Avoid blocking operations

Don’t use traditional `InputStream` / `OutputStream` directly in upload handling — this would block Netty worker threads and defeat reactive benefits. Rely on `FilePart.transferTo()` or streaming with `DataBufferUtils`. ([Baeldung on Kotlin][1])

### 💾 Disk space and directory

Make sure the upload directory exists or create it before calling `transferTo()`, or `transferTo` will fail.

Example:

```java
Files.createDirectories(uploadDir);
```

---

## 🧪 Test with `curl`

**Single file:**

```bash
curl -X POST -F "file=@/path/to/largefile.bin" http://localhost:8080/upload/single
```

**Multiple files:**

```bash
curl -X POST \
  -F "files=@/path/to/file1.bin" \
  -F "files=@/path/to/file2.bin" \
  http://localhost:8080/upload/multiple
```

---

## 🧠 What Happens Internally

* WebFlux parses the multipart request in a streaming fashion — reading `PartEvent`s and emitting file parts without buffering whole files. ([Home][3])
* `FilePart.transferTo()` handles writing each chunk to disk reactively, honoring backpressure from the file system, keeping memory usage low. ([Baeldung on Kotlin][1])

---

If you want, I can also show how to **stream uploads to another service** (e.g., directly to S3) using reactive `WebClient` — just ask!

[1]: https://www.baeldung.com/spring-streaming-multipart-data?utm_source=chatgpt.com "Streaming Multipart Data Sequentially in Spring | Baeldung"
[2]: https://runebook.dev/en/articles/spring_boot/application-properties/application-properties.web.spring.webflux.multipart.max-parts?utm_source=chatgpt.com "spring_boot - tag for code blocks."
[3]: https://docs.spring.io/spring-framework/docs/6.0.0/reference/html/web-reactive.html?utm_source=chatgpt.com "Web on Reactive Stack"
