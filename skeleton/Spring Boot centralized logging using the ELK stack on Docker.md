Below is a **skeleton guide (with sample configs)** for setting up **centralized logging for a Spring Boot app using the ELK stack on Docker**. It includes:

1. A **Docker Compose skeleton** for ELK
2. **Spring Boot logging setup**
3. **Logstash pipeline config**
4. How the logs flow and basic tips

This minimal example will help you get started quickly and you can build on it for production.
see [ELK Stack Spring Boot](https://github.com/riggedjoker/ELK-stack-springboot)
---

## ✅ 1) Project Structure (Skeleton)

```
logging-demo/
├── spring-boot-app/
│   ├── src/…
│   ├── Dockerfile
│   └── logback-spring.xml
├── elk/
│   ├── docker-compose.yml
│   └── logstash/
│       ├── logstash.conf
│       └── pipeline/
└── README.md
```

---

## ✅ 2) ELK Stack with Docker Compose

Create **elk/docker-compose.yml**:

```yaml
version: '3.8'

services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:8.7.0
    container_name: elasticsearch
    environment:
      - discovery.type=single-node
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ports:
      - "9200:9200"
    networks:
      - elk-net

  kibana:
    image: docker.elastic.co/kibana/kibana:8.7.0
    container_name: kibana
    ports:
      - "5601:5601"
    environment:
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
    networks:
      - elk-net
    depends_on:
      - elasticsearch

  logstash:
    image: docker.elastic.co/logstash/logstash:8.7.0
    container_name: logstash
    volumes:
      - ./logstash/pipeline:/usr/share/logstash/pipeline
    ports:
      - "5000:5000"
    networks:
      - elk-net
    depends_on:
      - elasticsearch

networks:
  elk-net:
    driver: bridge
```

✔ This brings up **Elasticsearch**, **Kibana**, and **Logstash** all in Docker. ([gcore.com][1])

---

## ✅ 3) Logstash Pipeline (elk/logstash/pipeline/logstash.conf)

```conf
input {
  tcp {
    port => 5000
    codec => json
  }
}

filter {
  # optional grok/processing here
}

output {
  elasticsearch {
    hosts => ["elasticsearch:9200"]
    index => "spring-logs-%{+YYYY.MM.dd}"
    # credentials if enabled
  }
  stdout { codec => rubydebug }
}
```

🧠 Logstash listens for JSON log data on TCP port **5000** and sends it to Elasticsearch.

---

## ✅ 4) Spring Boot Logging Setup

### 4a) `pom.xml` (Maven)

Add Logstash/Logback JSON encoder:

```xml
<dependency>
    <groupId>net.logstash.logback</groupId>
    <artifactId>logstash-logback-encoder</artifactId>
    <version>7.4</version>
</dependency>
```

---

### 4b) `logback-spring.xml` (in Spring Boot app)

```xml
<configuration>
  <appender name="LOGSTASH" class="net.logstash.logback.appender.LogstashTcpSocketAppender">
    <destination>logstash:5000</destination>

    <encoder class="net.logstash.logback.encoder.LoggingEventCompositeJsonEncoder">
      <providers>
        <timestamp/>
        <logLevel/>
        <loggerName/>
        <pattern>
          <pattern>
            {
              "app": "spring-elk-demo"
            }
          </pattern>
        </pattern>
      </providers>
    </encoder>

    <includeCallerData>true</includeCallerData>
  </appender>

  <root level="INFO">
    <appender-ref ref="LOGSTASH"/>
  </root>
</configuration>
```

👉 This config sends Spring Boot logs to Logstash as **JSON** over TCP (port 5000). ([DEV Community][2])

---

## ✅ 5) Spring Boot Dockerfile

Create **spring-boot-app/Dockerfile**:

```dockerfile
FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app
COPY target/*.jar app.jar
ENTRYPOINT ["java","-jar","/app/app.jar"]
```

---

## ✅ 6) Run & Test

1. In `elk/`:

   ```bash
   docker-compose up -d
   ```

2. Build and run Spring Boot app:

   ```bash
   mvn clean package
   docker build -t spring-elk-app ../spring-boot-app
   docker run --network=logging-demo_elk-net spring-elk-app
   ```

3. Open **Kibana UI**:

   ```
   http://localhost:5601
   ```

   Create an index pattern like:

   ```
   spring-logs-*
   ```

   to see logs. ([advancedweb.hu][3])

---

## 🔄 Log Flow

```
Spring Boot App (JSON logs)
      |
      v  TCP (5000)
  Logstash
      |
      v  Elasticsearch Indexes
      |
      v  Kibana (Search & Visualize)
```

---

## 📌 Tips & Common Issues

✔ **Make sure containers are on the same Docker network.**
✔ **If logs don’t appear**:

* Check Logstash container logs (`docker logs logstash`).
* Confirm network connectivity.
* Use `stdout` output in Logstash to debug.
  ✔ Structured (JSON) logging helps Kibana parse fields (timestamps, levels). ([sgitario.github.io][4])

---

## 🧠 Next Enhancements

✅ Add **Filebeat** instead of direct Logstash appender for resilience.
✅ Add **security (Elastic passwords)**.
✅ Use **Grok filters** in Logstash for unstructured logs. ([discuss.elastic.co][5])

---

If you want, I can also provide that as a **GitHub-ready starter repo** with all files prewritten — just let me know!

[1]: https://gcore.com/learning/docker-centralized-logging-with-elk-stack/?utm_source=chatgpt.com "Docker Centralized Logging with ELK Stack - Gcore"
[2]: https://dev.to/devaaai/achieving-comprehensive-observability-in-spring-boot-microservices-with-the-elk-stack-and-2hg2?utm_source=chatgpt.com "Achieving Comprehensive Observability in Spring Boot Microservices with the ELK Stack and OpenTelemetry - DEV Community"
[3]: https://advancedweb.hu/how-to-integrate-a-containerized-application-with-the-elastic-stack/?utm_source=chatgpt.com "How to integrate a containerized application with the Elastic Stack - Advanced Web Machinery"
[4]: https://sgitario.github.io/visualize-structured-logs-in-elk-stack-with-dockers/?utm_source=chatgpt.com "Visualize structured logs in ELK stack – Jose Carvajal – Passionate Software Engineer"
[5]: https://discuss.elastic.co/t/cannot-connect-logstash-in-filebeat-in-my-spring-boot-elk/302578?utm_source=chatgpt.com "Cannot connect logstash in filebeat in my Spring Boot ELK - Logstash - Discuss the Elastic Stack"
