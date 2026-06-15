# Spring Boot là gì?

**Spring Boot** là một framework mã nguồn mở dựa trên ngôn ngữ Java, được xây dựng trên nền tảng **Spring Framework**, nhằm đơn giản hóa quá trình khởi tạo và phát triển các ứng dụng chạy độc lập, sẵn sàng triển khai trong môi trường production.

Spring Boot là một module mở rộng thuộc hệ sinh thái Spring Framework, giúp lập trình viên giảm thiểu các cấu hình phức tạp và lượng mã cấu hình (boilerplate code).

Thay vì mất nhiều thời gian để cấu hình:

* Spring MVC
* Tomcat
* Jackson
* Logging
* Dependency

Spring Boot cho phép tạo và chạy một ứng dụng Web hoàn chỉnh chỉ trong vài phút.

---

## Mục tiêu của Spring Boot

Spring Boot được tạo ra nhằm:

* Đơn giản hóa việc cấu hình Spring Framework.
* Giảm lượng mã cấu hình thủ công.
* Giúp phát triển ứng dụng nhanh hơn.
* Hỗ trợ xây dựng các ứng dụng độc lập (Standalone Application).
* Sẵn sàng triển khai trong môi trường production.

---

# Các tính năng cốt lõi của Spring Boot

Spring Boot cung cấp nhiều tính năng mạnh mẽ, trong đó quan trọng nhất là:

1. Auto-configuration
2. Starter Dependencies
3. Embedded Server
4. Spring Boot Actuator

---

# 1. Auto-configuration (Tự động cấu hình)

## Khái niệm

Auto-configuration là cơ chế giúp Spring Boot tự động cấu hình các thành phần của ứng dụng dựa trên:

* Các thư viện (dependencies) được thêm vào dự án.
* Các thiết lập trong file cấu hình.

Nhờ đó, lập trình viên không cần cấu hình thủ công nhiều thành phần như trong Spring Framework truyền thống.

---

## Ví dụ

Giả sử thêm dependency:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

Spring Boot sẽ tự động cấu hình:

* Spring MVC
* DispatcherServlet
* Jackson JSON
* Embedded Tomcat
* Message Converter

mà không cần cấu hình bằng XML hoặc Java Config.

---

## Không có Auto-configuration

Lập trình viên phải tự cấu hình:

```java
@Configuration
@EnableWebMvc
@ComponentScan
public class WebConfig {

}
```

và nhiều thành phần khác.

---

## Có Auto-configuration

Chỉ cần:

```java
@SpringBootApplication
public class Application {

}
```

Spring Boot sẽ tự động thực hiện phần lớn các cấu hình cần thiết.

---

## Lợi ích

* Giảm cấu hình thủ công.
* Giảm boilerplate code.
* Tăng tốc độ phát triển ứng dụng.
* Dễ bảo trì.

---

# 2. Starter Dependencies

## Khái niệm

Starter Dependencies là các gói thư viện được Spring Boot gom nhóm sẵn cho từng mục đích sử dụng.

Thay vì phải khai báo riêng lẻ hàng chục thư viện cùng với phiên bản tương ứng, lập trình viên chỉ cần thêm một starter phù hợp.

Spring Boot sẽ tự động quản lý các phiên bản tương thích giữa các thư viện.

---

## Ví dụ

### Xây dựng Web Application hoặc REST API

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

Starter này bao gồm:

* Spring MVC
* Jackson
* Validation
* Embedded Tomcat
* Logging

---

### Làm việc với JPA và Hibernate

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

---

### Bảo mật

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

---

## Một số Starter phổ biến

| Starter                        | Chức năng                          |
| ------------------------------ | ---------------------------------- |
| spring-boot-starter-web        | Xây dựng Web Application, REST API |
| spring-boot-starter-data-jpa   | Làm việc với JPA và Hibernate      |
| spring-boot-starter-security   | Bảo mật ứng dụng                   |
| spring-boot-starter-test       | Kiểm thử ứng dụng                  |
| spring-boot-starter-validation | Validation dữ liệu                 |
| spring-boot-starter-websocket  | Hỗ trợ WebSocket                   |

---

## Lợi ích

* Đơn giản hóa quản lý thư viện.
* Không phải tự quản lý phiên bản.
* Giảm xung đột dependency.
* Tăng tốc độ khởi tạo dự án.

---

# 3. Embedded Server (Máy chủ nhúng)

## Khái niệm

Spring Boot tích hợp sẵn máy chủ Web ngay trong ứng dụng.

Các máy chủ được hỗ trợ:

* Tomcat (mặc định)
* Jetty
* Undertow

Nhờ đó, ứng dụng có thể chạy trực tiếp mà không cần cài đặt Web Server bên ngoài.

---

## Ví dụ

Chỉ cần chạy:

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {

        SpringApplication.run(
                Application.class,
                args);

    }

}
```

Ứng dụng sẽ khởi động cùng với Tomcat nhúng.

---

## Chạy ứng dụng

```bash
java -jar app.jar
```

Tomcat sẽ được khởi động tự động:

```text
Tomcat started on port(s): 8080
```

---

## Ưu điểm

* Không cần cài đặt Tomcat riêng.
* Triển khai đơn giản.
* Dễ đóng gói thành một file JAR duy nhất.
* Phù hợp với Microservices và Cloud.

---

## Máy chủ được hỗ trợ

### Tomcat

Máy chủ mặc định của Spring Boot.

```text
spring-boot-starter-web
↓
Embedded Tomcat
```

---

### Jetty

Hiệu năng tốt, nhẹ.

---

### Undertow

Phù hợp với hệ thống cần hiệu suất cao.

---

# 4. Spring Boot Actuator

## Khái niệm

Spring Boot Actuator cung cấp các tính năng giám sát và quản lý ứng dụng trong môi trường production.

Actuator cho phép theo dõi:

* Tình trạng hoạt động của hệ thống.
* Hiệu năng.
* Log.
* Bộ nhớ.
* Metrics.
* Thread.
* Environment.

---

## Thêm dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>
        spring-boot-starter-actuator
    </artifactId>
</dependency>
```

---

## Health Check

URL:

```text
/actuator/health
```

Ví dụ:

```json
{
    "status": "UP"
}
```

Nếu:

```text
status = UP
```

nghĩa là ứng dụng đang hoạt động bình thường.

---

## Metrics

URL:

```text
/actuator/metrics
```

Cho phép theo dõi:

* CPU
* Memory
* JVM
* Request Count

---

## Environment

URL:

```text
/actuator/env
```

Hiển thị:

* application.properties
* biến môi trường
* cấu hình hệ thống

---

## Loggers

URL:

```text
/actuator/loggers
```

Cho phép xem và thay đổi mức log.

Ví dụ:

```text
INFO
DEBUG
WARN
ERROR
```

---

## Lợi ích

* Hỗ trợ monitoring.
* Theo dõi sức khỏe hệ thống.
* Phân tích hiệu năng.
* Hỗ trợ vận hành trong production.

---

# Tổng kết

Spring Boot là một framework được xây dựng trên Spring Framework nhằm đơn giản hóa quá trình khởi tạo và phát triển các ứng dụng Java.

Các tính năng cốt lõi quan trọng nhất của Spring Boot gồm:

| Tính năng            | Chức năng                                           |
| -------------------- | --------------------------------------------------- |
| Auto-configuration   | Tự động cấu hình các thành phần dựa trên dependency |
| Starter Dependencies | Cung cấp các gói thư viện được gom nhóm sẵn         |
| Embedded Server      | Tích hợp Tomcat, Jetty hoặc Undertow trong ứng dụng |
| Spring Boot Actuator | Hỗ trợ giám sát và theo dõi sức khỏe hệ thống       |

Nhờ các tính năng trên, Spring Boot giúp:

* Giảm cấu hình phức tạp.
* Giảm boilerplate code.
* Tăng tốc độ phát triển ứng dụng.
* Dễ triển khai.
* Sẵn sàng cho môi trường production.
