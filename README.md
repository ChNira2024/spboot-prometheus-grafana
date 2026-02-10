# Prometheus Setup in Spring-Boot

## Dependencies
Add the following dependencies in your `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webmvc</artifactId>
</dependency>
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>

## why should i add both actuator and prometheus?
Actuator = produces metrics(Metric collection via Micrometer (JVM, HTTP, CPU, memory, GC, custom metrics)
Micrometer Prometheus registry = exposes them in Prometheus format(Converts Micrometer metrics into Prometheus’s text format and then Exposes them at /actuator/prometheus

Only Actuator
Metrics collected ✅
Prometheus endpoint ❌

Only Prometheus registry
Prometheus format available ❌ (no metrics source)
No actuator endpoints ❌



## application.properties
Add the following in your `application.properties`: 

```properties
server.port=8080 
management.endpoints.web.exposure.include=health,info,metrics,prometheus
management.endpoint.health.show-details=always 
