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
```

## Why should I add both Actuator and Prometheus?

- **Actuator:** produces metrics (Metric collection via Micrometer: JVM, HTTP, CPU, memory, GC, custom metrics)
- **Micrometer Prometheus registry:** exposes them in Prometheus format at `/actuator/prometheus`.

| Configuration            | Metrics Collected | Prometheus Endpoint |
|--------------------------|-----------------|------------------|
| Only Actuator            | ✅ Yes          | ❌ No             |
| Only Prometheus registry | ❌ No           | ❌ No             |

## application.properties

Add the following in your `application.properties`:

```properties
server.port=8080
management.endpoints.web.exposure.include=health,info,metrics,prometheus
management.endpoint.health.show-details=always
```
## Controller
Add the following in your `controller package`:
```@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello Prometheus!";
    }
}
```
#### Once Spring boot application is ready, then go to browser and hit: 
```http://localhost:8080/hello
Hello Prometheus!
 See application is running fine...Then check actuator with prometheus..hit in browser
http://localhost:8080/actuator/prometheus
 HELP application_ready_time_seconds Time taken for the application to be ready to service requests
 TYPE application_ready_time_seconds gauge
 application_ready_time_seconds{main_application_class="com.niranjana.project.SpringPrometheusGrafanaApplication"} 1.501
 HELP application_started_time_seconds Time taken to start the application
 TYPE application_started_time_seconds gauge
application_started_time_seconds{main_application_class="com.niranjana.project.SpringPrometheusGrafanaApplication"} 1.438
 HELP disk_free_bytes Usable space for path
```
#### Note: But prometheus provides details are text file , Normally we can't understand ..So mostly use prometheuse server to see it graph..
#### But in my laptop prometheus server is not there..So that i want to run prometheus inside docker...

# To run prometheus inside docker
```First docker installation must be there in your laptop.
=>Then in your laptop,  Create a folder "prometheus-config-yml" folder and inside that folder create below .yml file ::-
global:
  scrape_interval: 5s

scrape_configs:
  - job_name: 'spring-prometheus-grafana'
    metrics_path: '/actuator/prometheus'
    static_configs:
      - targets: ['host.docker.internal:8080']

My file location:-C:\Workspace\prometheus-config-yml\prometheus.yml

=>then go to cmd->open that folder location then execute below command::-
:\Workspace\prometheus-config-yml>docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
