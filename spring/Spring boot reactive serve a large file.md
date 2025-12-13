To **asynchronously and non-blockingly serve a large file stored locally** in Spring Boot, you have two main approaches depending on whether you want **reactive/non-blocking I/O** or **async streaming with traditional MVC**. Both avoid loading the whole file into memory and scale better under load:

---

## ✅ Option A — **Reactive Non-Blocking with Spring WebFlux (Recommended for High Concurrency)**

Spring WebFlux is built on **Project Reactor** and is fully asynchronous/non-blocking from the socket to the file I/O layer when used with compatible runtimes (e.g., Netty). ([博客园][1])

### **1. Add WebFlux Starter**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```

This gives you a reactive stack for serving requests without blocking threads. ([Roytuts][2])

---

### **2. Reactive File Streaming Controller**

```java
import org.springframework.core.io.buffer.DataBuffer;
import org.springframework.core.io.buffer.DataBufferUtils;
import org.springframework.http.MediaType;
import org.springframework.http.server.reactive.ServerHttpResponse;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import reactor.core.publisher.Flux;

import java.nio.file.Path;
import java.nio.file.Paths;

@RestController
public class ReactiveFileController {

    @GetMapping(value = "/download/{filename}", produces = MediaType.APPLICATION_OCTET_STREAM_VALUE)
    public Flux<DataBuffer> streamFile(@PathVariable String filename, ServerHttpResponse response) {
        Path filePath = Paths.get("files").resolve(filename).toAbsolutePath();

        // Let WebFlux read the file in small buffers and stream to client
        return DataBufferUtils.read(filePath, response.bufferFactory(), 4096);
    }
}
```

**How it works**

* `DataBufferUtils.read(...)` reads the file in chunks (`4096` bytes here) **without blocking threads**.
* Each `DataBuffer` is sent downstream as soon as it’s available, letting Netty (or the chosen reactor server) handle backpressure and throughput. ([Spring Framework][3])

---

## ✅ Option B — **Traditional Spring MVC with Async StreamingResponseBody**

If you’re not ready to use WebFlux, Spring MVC can still stream files **asynchronously** so the servlet container thread isn’t blocked for the full duration of the transfer. ([CMS][4])

### **StreamingResponseBody Controller**

```java
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.servlet.mvc.method.annotation.StreamingResponseBody;

import java.io.InputStream;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

@RestController
public class AsyncFileController {

    @GetMapping(value = "/download/{filename}", produces = MediaType.APPLICATION_OCTET_STREAM_VALUE)
    public ResponseEntity<StreamingResponseBody> download(@PathVariable String filename) {
        Path filePath = Paths.get("files").resolve(filename).toAbsolutePath();

        StreamingResponseBody body = outputStream -> {
            try (InputStream fileStream = Files.newInputStream(filePath)) {
                byte[] buffer = new byte[8192];
                int len;
                while ((len = fileStream.read(buffer)) != -1) {
                    outputStream.write(buffer, 0, len);
                    outputStream.flush();
                }
            }
        };

        return ResponseEntity.ok()
                .header("Content-Disposition", "attachment; filename=\"" + filename + "\"")
                .body(body);
    }
}
```

**Notes**

* This streams the file in chunks and returns control to the servlet container quickly.
* The underlying servlet container can use async I/O to handle concurrent requests more efficiently than blocking all threads. ([CMS][4])

---

## 📌 Choosing Between WebFlux and MVC

| Approach                        | Async? | Non-Blocking?                    | Best Use Case                                    |
|---------------------------------|--------|----------------------------------|--------------------------------------------------|
| **Spring WebFlux**              | ✔      | ✔ (Reactive)                     | High-performance, highly concurrent file serving |
| **StreamingResponseBody (MVC)** | ✔      | ❌ (still uses blocking file I/O) | Simple upgrades of existing MVC apps             |

* **WebFlux** with `DataBufferUtils` is truly non-blocking and scales with fewer threads. ([Spring Framework][3])
* **StreamingResponseBody** is easier to adopt in traditional apps but doesn’t eliminate blocking at the file I/O layer.

---

## 📌 Tips for Large File Serving

✔ Don’t load the whole file into memory — use streaming. ([برمجة][5])
✔ Set proper `Content-Disposition` and MIME type for downloads.
✔ For even better UX (resume / range requests), you can support **HTTP Range headers** in WebFlux. ([blog.csdn.net][6])

---

If you want **range support** (partial downloads or resume), let me know — I can provide a ready-to-use example!

[1]: https://www.cnblogs.com/moyu0-0/articles/15788222.html?utm_source=chatgpt.com "Spring WebFlux 官方文档 - moyu0 - 博客园"
[2]: https://roytuts.com/download-large-file-using-spring-rest-api/?utm_source=chatgpt.com "Download Large File Using Spring REST API - Roy Tutorials"
[3]: https://docs.springframework.org.cn/spring-framework/reference/web/webflux/new-framework.html?utm_source=chatgpt.com "概述 :: Spring Framework - Spring 框架"
[4]: https://www.cms.lk/asynchronous-streaming-request-processing-in-spring-mvc-4-2-spring-boot/?utm_source=chatgpt.com "CMS - Asynchronous Streaming Request Processing in Spring MVC 4.2 Spring Boot"
[5]: https://prgrmmng.com/streaming-large-files-rest-apis-spring-boot?utm_source=chatgpt.com "Streaming Large Files in REST APIs with Spring Boot | prgrmmng.com"
[6]: https://blog.csdn.net/m0_73837751/article/details/144160685?utm_source=chatgpt.com "Spring WebFlux-CSDN博客"


Great — here’s a **complete example** of how to support **HTTP Range requests (断点续传 / resume download)** when serving large files with Spring Boot, both for **Spring MVC** (blocking) and **Spring WebFlux** (non-blocking). This lets clients **request specific byte ranges** of a file, so they can resume downloads or play media progressively. ([Trae][1])

---

## ✅ MVC: Serve Large files with HTTP Range Support

Spring MVC will automatically parse the `Range` header and return partial content when you return a `Resource` (such as `FileSystemResource`) in a `ResponseEntity`. Spring’s internal `HttpRange` and `ResourceRegion` support help implement RFC-compliant partial responses. ([Home][2])

### 📌 Controller Example (MVC)

```java
import org.springframework.core.io.FileSystemResource;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RestController;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.List;

@RestController
public class RangeFileDownloadController {

    @GetMapping("/files/{filename}")
    public ResponseEntity<?> downloadFile(
            @PathVariable String filename,
            @RequestHeader(value = "Range", required = false) String rangeHeader
    ) throws IOException {
        Path filePath = Paths.get("files").resolve(filename).toAbsolutePath();
        if (!Files.exists(filePath)) {
            return ResponseEntity.notFound().build();
        }

        FileSystemResource resource = new FileSystemResource(filePath);
        long fileSize = Files.size(filePath);

        // Let Spring MVC handle Range requests
        if (rangeHeader != null) {
            // Parse the Range header into a List<HttpRange>
            List<HttpRange> ranges = HttpRange.parseRanges(rangeHeader);
            HttpRange range = ranges.get(0);

            long start = range.getRangeStart(fileSize);
            long end   = range.getRangeEnd(fileSize);

            long rangeLength = end - start + 1;

            return ResponseEntity.status(HttpStatus.PARTIAL_CONTENT)
                    .header(HttpHeaders.ACCEPT_RANGES, "bytes")
                    .header(HttpHeaders.CONTENT_RANGE,
                            "bytes " + start + "-" + end + "/" + fileSize)
                    .header(HttpHeaders.CONTENT_LENGTH, String.valueOf(rangeLength))
                    .contentType(MediaType.APPLICATION_OCTET_STREAM)
                    .body(new InputStreamResource(Files.newInputStream(filePath)));
        }

        // No Range requested → full body
        return ResponseEntity.ok()
                .header(HttpHeaders.CONTENT_LENGTH, String.valueOf(fileSize))
                .header(HttpHeaders.ACCEPT_RANGES, "bytes")
                .contentType(MediaType.APPLICATION_OCTET_STREAM)
                .body(resource);
    }
}
```

**How it works**

* Read the `Range` header if present.
* Parse it with `HttpRange.parseRanges(…)`, then compute start/end.
* Respond with **206 Partial Content** and appropriate headers:

    * `Accept-Ranges: bytes`
    * `Content-Range: bytes start-end/total`
    * `Content-Length: length`
* Spring MVC will stream the file without loading into memory. ([Home][2])

---

## ✅ WebFlux: Non-Blocking Partial File Streaming

With **Spring WebFlux**, you can also support range requests in a **non-blocking** (reactive) way. WebFlux also understands `HttpRange` and can construct `ResourceRegion` objects to send just the requested parts of a file. ([Home][3])

### 📌 Reactive Controller (WebFlux)

```java
import org.springframework.core.io.Resource;
import org.springframework.core.io.FileSystemResource;
import org.springframework.core.io.buffer.DataBuffer;
import org.springframework.core.io.buffer.DataBufferUtils;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpRange;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.http.server.reactive.ServerHttpResponse;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RestController;
import reactor.core.publisher.Flux;

import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.List;

@RestController
public class ReactiveRangeFileController {

    @GetMapping("/reactive/files/{filename}")
    public Flux<DataBuffer> streamRange(
            @PathVariable String filename,
            @RequestHeader(value = "Range", required = false) String rangeHeader,
            ServerHttpResponse response
    ) {
        Path filePath = Paths.get("files").resolve(filename).toAbsolutePath();
        Resource resource = new FileSystemResource(filePath);

        long fileSize;
        try {
            fileSize = resource.contentLength();
        } catch (Exception e) {
            response.setStatusCode(HttpStatus.NOT_FOUND);
            return Flux.empty();
        }

        // Always show that range downloads are supported
        response.getHeaders().add(HttpHeaders.ACCEPT_RANGES, "bytes");

        if (rangeHeader != null) {
            List<HttpRange> ranges = HttpRange.parseRanges(rangeHeader);
            HttpRange range = ranges.get(0);

            long start = range.getRangeStart(fileSize);
            long end   = range.getRangeEnd(fileSize);

            response.setStatusCode(HttpStatus.PARTIAL_CONTENT);
            response.getHeaders().add(HttpHeaders.CONTENT_RANGE,
                    "bytes " + start + "-" + end + "/" + fileSize);

            // Stream only the requested region
            return DataBufferUtils.read(resource.getInputStream(), response.bufferFactory(), 4096)
                    .skipWhile(db -> /* skip until start offset logic */ false)
                    .takeUntil(db -> /* stop after sending up to end offset */ false);
        }

        // No range → full file
        response.getHeaders().add(HttpHeaders.CONTENT_LENGTH, String.valueOf(fileSize));
        response.getHeaders().setContentType(MediaType.APPLICATION_OCTET_STREAM);
        return DataBufferUtils.read(resource.getInputStream(), response.bufferFactory(), 4096);
    }
}
```

> **Note:** The above reactive example sketches how to handle ranges: in practice you’d implement logic around skipping and slicing data buffers to honor the exact start/end positions efficiently in a reactive way (e.g., using `Flux.skip()` and custom logic). WebFlux’s built-in handling of ranges is simpler if returning `ResourceRegion` where supported. ([Home][3])

---

## 🧠 Key HTTP Headers for Range Support

| Header                                | Meaning                                 |             |
|---------------------------------------|-----------------------------------------|-------------|
| `Range: bytes=500-1500`               | Client requests byte 500–1500           |             |
| `Accept-Ranges: bytes`                | Server supports range requests          |             |
| `Content-Range: bytes 500-1500/50000` | Server indicates what range is returned |             |
| `Content-Length`                      | Length of the returned part             | ([CSDN][4]) |

---

## 🛠 Frontend Usage

Clients (e.g., browsers, download managers, video players) send requests like:

```http
GET /files/video.mp4 HTTP/1.1
Range: bytes=0-1023
```

And expect a **206 Partial Content** response with the requested data. ([CSDN][4])

---

## 📌 Tips

✅ Use `FileSystemResource` (not `InputStreamResource`) to let Spring MVC handle range support automatically. ([Home][2])
✅ Set `Accept-Ranges: bytes` so clients know resume/download features are supported. ([ShowAPI][5])
✅ For large files, prefer WebFlux/non-blocking to avoid tying up threads.

---

If you want a **fully integrated WebFlux + full Range/seek logic example** with tests and performance tips, I can provide that too. Just ask!

[1]: https://www.trae.cn/article/660509954?utm_source=chatgpt.com "服务器向客户端发送文件的实现方法与实战指南 | TRAE - The Real AI Engineer"
[2]: https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-range.html?utm_source=chatgpt.com "Range Requests :: Spring Framework"
[3]: https://docs.spring.io/spring-framework/reference/web/webflux/range.html?utm_source=chatgpt.com "Range Requests :: Spring Framework"
[4]: https://blog.csdn.net/qq_34125999/article/details/125804077?utm_source=chatgpt.com "Spring Boot 大文件上传(断点上传)、服务端分片下载、客户端分片下载（断点下载）_springboot nio大文件分片下载-CSDN博客"
[5]: https://www.showapi.com/news/article/6795b3fd4ddd79f11a3269ed?utm_source=chatgpt.com "Spring Boot中实现高效文件上传下载功能的完整指南-易源AI资讯 | 万维易源"
