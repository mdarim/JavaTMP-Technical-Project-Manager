Below is a **reactive, non-blocking skeleton** showing how to use **Spring WebFlux + Spring Data Reactive MongoDB** to **retrieve large files from MongoDB (GridFS)** asynchronously with backpressure support.

---

## 🔹 1) Dependencies

Make sure you have these in your `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb-reactive</artifactId>
</dependency>
```

These provide reactive WebFlux and MongoDB support. ([GeeksforGeeks][1])

---

## 🔹 2) Configure Reactive MongoDB

Spring Boot will auto-configure the reactive driver from `spring.data.mongodb.uri`. No blocking drivers allowed.

```yaml
spring:
  data:
    mongodb:
      uri: mongodb://localhost:27017/mydb
```

Reactive MongoDB uses `ReactiveMongoDatabaseFactory` internally, and you can get a `ReactiveGridFsTemplate` bean. ([Home][2])

---

## 🔹 3) ReactiveGridFsTemplate Bean

```java
@Configuration
public class GridFsConfig {
    @Bean
    public ReactiveGridFsTemplate reactiveGridFsTemplate(
            ReactiveMongoDatabaseFactory dbFactory,
            MongoConverter converter) {
        return new ReactiveGridFsTemplate(dbFactory, converter);
    }
}
```

`ReactiveGridFsTemplate` lets you interact with GridFS reactively. ([Home][3])

---

## 🔹 4) Controller to Retrieve a Large File

### ✅ Streaming download (non-blocking):

```java
@RestController
@RequestMapping("/files")
public class FileController {

    private final ReactiveGridFsTemplate gridFsTemplate;

    public FileController(ReactiveGridFsTemplate gridFsTemplate) {
        this.gridFsTemplate = gridFsTemplate;
    }

    @GetMapping(value = "/{id}", produces = MediaType.APPLICATION_OCTET_STREAM_VALUE)
    public Mono<Void> downloadFile(@PathVariable String id, ServerWebExchange exchange) {

        return gridFsTemplate
                .findOne(query(where("_id").is(id)))
                // Get a GridFS resource that holds a reactive download stream
                .flatMap(gridFsTemplate::getResource)
                .flatMapMany(resource -> {
                    // reactive stream of DataBuffer (chunked file content)
                    return resource.getDownloadStream();
                })
                .as(dataBufferFlux ->
                    // write with backpressure support to the HTTP response
                    exchange.getResponse().writeWith(dataBufferFlux)
                );
    }
}
```

**What’s happening here:**

* `findOne(...)` fetches metadata for the file by **_id** (or you could query by filename).
* `getResource(...)` wraps it in a reactive resource that exposes a **Flux<DataBuffer>** for the file data.
* `writeWith(...)` streams the bytes to the client **non-blocking, with backpressure**. ([blank][4])

---

## 🔹 5) Notes & Best Practices

### 📌 Why use Flux<DataBuffer>

* WebFlux sends chunks to the client as they are available; no buffering entire file in memory.
* Handles backpressure from the client automatically.

### 📌 Error / Not Found Handling

Add `.switchIfEmpty(...)` to return `404 Not Found` if a file with given ID doesn’t exist.

### 📌 Use ObjectId

Convert `String id` to `ObjectId` if you store IDs as Mongo’s ObjectId.

### 📌 Client Download

Direct GET to `/files/{id}` streams file. Typical clients (curl, browsers) will treat it as file download.

---

## 🧠 Under the Hood

* **GridFS** stores large files in chunks in *fs.files* and *fs.chunks* collections in MongoDB.
* As GridFS splits a large file into smaller chunks, the reactive API reassembles them as a continuous `Flux<DataBuffer>`.
* This approach avoids blocking threads even for files much larger than 16 MB (MongoDB’s BSON size limit). ([MongoDB][5])

---

## 🛠 Summary of Reactive Flow

```
HTTP GET /files/{id}
   └── Reactive controller
         └── ReactiveGridFsTemplate.findOne(...)
                └── getResource(...)
                     └── reactive Flux<DataBuffer>
                          └── writeWith(...) → HTTP response (Netty)
```

Each step is **asynchronous & non-blocking**, backpressured by default.

---

If you want, I can also share the **upload part** reactively (using `FilePart` + `ReactiveGridFsTemplate.store(...)`). Just let me know!

[1]: https://www.geeksforgeeks.org/non-blocking-io-operation-with-spring-webflux/?utm_source=chatgpt.com "Non-Blocking I/O Operation with Spring WebFlux - GeeksforGeeks"
[2]: https://docs.spring.io/spring-data/mongodb/reference/mongodb/template-gridfs.html?utm_source=chatgpt.com "GridFS Support :: Spring Data MongoDB"
[3]: https://docs.spring.io/spring-data/mongodb/docs/current/api/org/springframework/data/mongodb/gridfs/ReactiveGridFsTemplate.html?utm_source=chatgpt.com "ReactiveGridFsTemplate (Spring Data MongoDB 4.5.5 API)"
[4]: https://hantsy.github.io/spring-reactive-sample/web/multipart.html?utm_source=chatgpt.com "Multipart Support | Spring Reactive Programming"
[5]: https://www.mongodb.com/docs/languages/java/reactive-streams-driver/v5.1/tutorials/gridfs/?utm_source=chatgpt.com "GridFS - Java Reactive Streams Driver v5.1 - MongoDB Docs"

Below is a **reactive, non-blocking upload skeleton** for **Spring WebFlux + Spring Data Reactive MongoDB (GridFS)** that works well with large files. We’ll also tie it into how to **download files**, so you get a complete upload/download reactive API.

### 🧱 What You Need Beforehand

* A reactive WebFlux project with:
  ✔ `spring-boot-starter-webflux`
  ✔ `spring-boot-starter-data-mongodb-reactive`
* A configured `ReactiveGridFsTemplate` bean
  (as shown previously)

Spring’s `ReactiveGridFsTemplate.store(...)` method takes a **reactive stream of DataBuffer** and stores it in MongoDB GridFS, returning a `Mono<ObjectId>` when done. ([Home][1])

---

## 📤 1) Reactive Upload Endpoint

Here’s a **WebFlux controller** method that accepts multipart uploads asynchronously using `FilePart`:

```java
@RestController
@RequestMapping("/files")
public class FileController {

    private final ReactiveGridFsTemplate gridFsTemplate;

    public FileController(ReactiveGridFsTemplate gridFsTemplate) {
        this.gridFsTemplate = gridFsTemplate;
    }

    @PostMapping(value = "/upload", consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
    public Mono<ResponseEntity<Map<String, String>>> uploadFile(
            @RequestPart("file") Mono<FilePart> filePartMono) {

        return filePartMono
                .flatMap(filePart -> {
                    // filePart.content() is a Flux<DataBuffer> (reactive stream)
                    return gridFsTemplate
                            .store(
                                filePart.content(),       // reactive upload stream
                                filePart.filename(),      // original filename
                                filePart.headers().getContentType().toString(), // content type
                                null                      // optional metadata
                            )
                            // return the stored file ID
                            .map(objectId -> {
                                return Map.of("id", objectId.toHexString());
                            });
                })
                .map(map -> ResponseEntity.ok(map))
                .defaultIfEmpty(ResponseEntity.badRequest().build());
    }
}
```

### 🔍 How it works

* `FilePart` is Spring WebFlux’s representation of a part in a multipart form upload. ([blank][2])
* `filePart.content()` returns `Flux<DataBuffer>` — a stream of file bytes **without buffering the entire file**.
* `gridFsTemplate.store(...)` takes that stream and writes it to GridFS reactively. ([Home][1])
* The `Mono<ObjectId>` returned contains the GridFS file ID.
* This setup is **fully non-blocking** and works well for large files.

---

## 📥 2) Reactive Download Endpoint (complete)

Here’s a simple **download** endpoint to return file bytes:

```java
@GetMapping(value = "/download/{id}", produces = MediaType.APPLICATION_OCTET_STREAM_VALUE)
public Mono<Void> downloadFile(@PathVariable String id, ServerWebExchange exchange) {
    return gridFsTemplate
            .findOne(query(where("_id").is(id)))
            .flatMap(gridFsTemplate::getResource)
            .flatMapMany(resource -> resource.getDownloadStream())
            .as(dataBufferFlux -> exchange.getResponse().writeWith(dataBufferFlux));
}
```

This streams file content back to clients with backpressure support. (Already shown before.)

---

## 🧠 Tips & Best Practices

### 📌 Multipart parsing

With WebFlux, multipart support is reactive — Spring will not buffer uploads in memory. Use `FilePart` instead of `MultipartFile`. ([blank][2])

### 📌 Metadata (optional)

You can include metadata (like uploader, tags, etc.) by supplying a document to the `store` call. ([Home][1])

### 📌 Error & Empty Handling

Always handle empty uploads or errors (e.g., missing file part) gracefully by returning appropriate response entities.

### 📌 Backpressure

Everything here is reactive — `Flux<DataBuffer>` streams bytes to/from the client and database efficiently with backpressure control.

---

## 🛠 Example Client Upload (optional)

Here’s how a client (e.g., WebClient) could POST a file:

```java
WebClient client = WebClient.create("http://localhost:8080");

MultipartBodyBuilder builder = new MultipartBodyBuilder();
builder.part("file", new FileSystemResource("/path/to/large.file"));

client.post()
      .uri("/files/upload")
      .contentType(MediaType.MULTIPART_FORM_DATA)
      .body(BodyInserters.fromMultipartData(builder.build()))
      .retrieve()
      .bodyToMono(String.class)
      .subscribe(System.out::println);
```

You can also send big file parts without blocking because WebClient is reactive too.

---

## 🧾 Quick Summary

| Operation | WebFlux Type                   | Reactive Pipeline                                   |
|-----------|--------------------------------|-----------------------------------------------------|
| Upload    | `FilePart -> Flux<DataBuffer>` | `gridFsTemplate.store(...) -> Mono<ObjectId>`       |
| Download  | GridFS                         | `getResource -> Flux<DataBuffer> -> writeWith(...)` |

Both are **asynchronous**, **non-blocking**, and support **large files** without loading whole files into memory.

---

If you want, I can also show you how to **add progress notifications** or **store metadata** along with uploads reactively.

[1]: https://docs.spring.io/spring-data/mongodb/docs/current/api/org/springframework/data/mongodb/gridfs/ReactiveGridFsTemplate.html?utm_source=chatgpt.com "ReactiveGridFsTemplate (Spring Data MongoDB 5.0.0 API)"
[2]: https://hantsy.github.io/spring-reactive-sample/web/multipart.html?utm_source=chatgpt.com "Multipart Support | Spring Reactive Programming"
