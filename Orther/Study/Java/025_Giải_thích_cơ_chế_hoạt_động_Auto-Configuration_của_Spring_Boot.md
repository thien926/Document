# Giải thích cơ chế hoạt động Auto-Configuration của Spring Boot

## Auto-Configuration là gì?

Auto-Configuration là một trong những tính năng quan trọng nhất của Spring Boot, cho phép hệ thống tự động cấu hình ứng dụng dựa trên:

* Các dependency có trong classpath.
* Các Bean đã được khai báo.
* Các thuộc tính cấu hình trong `application.properties` hoặc `application.yml`.

Mục tiêu của Auto-Configuration là giảm tối đa lượng cấu hình thủ công mà lập trình viên phải thực hiện.

---

# Cơ chế hoạt động của Auto-Configuration

## 1. Kích hoạt Auto-Configuration

Thông thường, lớp khởi động của Spring Boot được đánh dấu bằng annotation:

```java
@SpringBootApplication
```

Annotation này bao gồm ba annotation khác:

```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan
```

Trong đó:

```java
@EnableAutoConfiguration
```

là annotation chịu trách nhiệm kích hoạt cơ chế tự động cấu hình.

Khi ứng dụng khởi động, Spring Boot sẽ bắt đầu quá trình tìm kiếm các cấu hình mặc định phù hợp.

---

## 2. Tìm kiếm các lớp Auto-Configuration

Sau khi được kích hoạt, Spring Boot sẽ quét các file cấu hình được khai báo trong các thư viện đã được thêm vào dự án.

Các lớp Auto-Configuration chủ yếu nằm trong:

```text
spring-boot-autoconfigure
```

Mỗi lớp Auto-Configuration phụ trách cấu hình cho một chức năng cụ thể, chẳng hạn:

* DataSource.
* Spring MVC.
* Embedded Tomcat.
* Jackson.
* JPA.
* Security.

Ví dụ:

* `DataSourceAutoConfiguration`
* `WebMvcAutoConfiguration`
* `JacksonAutoConfiguration`
* `SecurityAutoConfiguration`

---

## 3. Đánh giá các điều kiện (@Conditional...)

Đây là phần quan trọng nhất của Auto-Configuration.

Mỗi lớp Auto-Configuration đều đi kèm với các annotation điều kiện (`@Conditional...`) để quyết định có nên tạo Bean hay không.

### @ConditionalOnClass

Chỉ kích hoạt cấu hình khi một class cụ thể tồn tại trong classpath.

Ví dụ:

```java
@ConditionalOnClass(DataSource.class)
```

Nếu dependency JDBC hoặc JPA đã được thêm vào dự án thì Spring Boot mới cấu hình DataSource.

---

### @ConditionalOnMissingBean

Chỉ tạo Bean nếu Bean cùng kiểu chưa được định nghĩa bởi lập trình viên.

Ví dụ:

```java
@ConditionalOnMissingBean
```

Nếu ứng dụng chưa tự khai báo:

```java
@Bean
public ObjectMapper objectMapper() {
    ...
}
```

thì Spring Boot sẽ tự tạo một `ObjectMapper` mặc định.

---

### @ConditionalOnProperty

Chỉ kích hoạt cấu hình khi một thuộc tính cấu hình có giá trị phù hợp.

Ví dụ:

```java
@ConditionalOnProperty(
        prefix = "spring.cache",
        name = "type")
```

Spring Boot sẽ cấu hình Cache dựa trên giá trị được khai báo trong:

```properties
spring.cache.type=redis
```

---

### @ConditionalOnWebApplication

Chỉ kích hoạt khi ứng dụng là ứng dụng web.

Ví dụ:

```java
@ConditionalOnWebApplication
```

Nếu dự án chứa:

```xml
spring-boot-starter-web
```

thì các Bean liên quan tới:

* DispatcherServlet.
* Spring MVC.
* Tomcat.

sẽ được tự động cấu hình.

---

## 4. Tạo Bean tương ứng

Sau khi các điều kiện được thỏa mãn, Spring Boot sẽ tự động tạo các Bean cần thiết và đưa chúng vào IoC Container.

Ví dụ:

Nếu dự án có:

```xml
spring-boot-starter-web
```

Spring Boot sẽ tự động cấu hình:

* Embedded Tomcat.
* DispatcherServlet.
* Jackson ObjectMapper.
* Spring MVC.
* MessageConverter.

Người phát triển không cần phải cấu hình thủ công như trong Spring Framework truyền thống.

---

## 5. Cơ chế ưu tiên cấu hình

Spring Boot luôn ưu tiên cấu hình do lập trình viên cung cấp.

Nguyên tắc:

```text
Custom Configuration > Auto-Configuration
```

Nếu lập trình viên tự định nghĩa Bean:

```java
@Bean
public ObjectMapper objectMapper() {
    return new ObjectMapper();
}
```

thì Bean này sẽ được sử dụng thay cho Bean mặc định mà Spring Boot định tạo.

Nhờ đó, Auto-Configuration vừa cung cấp cấu hình mặc định, vừa cho phép tùy biến dễ dàng.

---

# Quy trình hoạt động tổng quát

```text
@SpringBootApplication
          │
          ▼
@EnableAutoConfiguration
          │
          ▼
Tìm các lớp Auto-Configuration
(DataSourceAutoConfiguration,
WebMvcAutoConfiguration,...)
          │
          ▼
Kiểm tra các annotation @Conditional...
          │
          ▼
Điều kiện thỏa mãn?
          │
      ┌── Yes ──► Tạo Bean
      │
      └── No ───► Bỏ qua
          │
          ▼
Đưa Bean vào IoC Container
```

---

# Ba bước trả lời phỏng vấn ngắn gọn

### Bước 1: Kích hoạt

`@EnableAutoConfiguration` (nằm trong `@SpringBootApplication`) sẽ bật cơ chế tự động cấu hình khi ứng dụng khởi động.

### Bước 2: Tìm kiếm

Spring Boot đọc các file cấu hình mặc định (`.imports` hoặc `spring.factories`) trong các Starter và tìm ra những lớp Auto-Configuration tương ứng.

### Bước 3: Quyết định

Các annotation điều kiện `@Conditional...` sẽ kiểm tra:

* Thư viện cần thiết có tồn tại hay không (`@ConditionalOnClass`).
* Lập trình viên đã tự cấu hình Bean hay chưa (`@ConditionalOnMissingBean`).
* Thuộc tính cấu hình có phù hợp hay không (`@ConditionalOnProperty`).
* Có phải ứng dụng web hay không (`@ConditionalOnWebApplication`).

Nếu điều kiện được thỏa mãn, Spring Boot sẽ tự động tạo Bean tương ứng.

---

# Tổng kết

* Auto-Configuration giúp giảm đáng kể lượng cấu hình thủ công trong Spring Boot.
* `@EnableAutoConfiguration` là annotation kích hoạt cơ chế này.
* Spring Boot sẽ tìm các lớp Auto-Configuration từ các Starter đã được thêm vào dự án.
* Các annotation `@Conditional...` quyết định cấu hình nào sẽ được áp dụng.
* Nếu lập trình viên tự định nghĩa Bean, cấu hình tùy chỉnh sẽ được ưu tiên hơn cấu hình mặc định.
* Nhờ Auto-Configuration, Spring Boot thực hiện triết lý **"Convention over Configuration"**, giúp việc phát triển ứng dụng trở nên nhanh chóng và đơn giản hơn.

# Ví dụ

### Dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

Khi ứng dụng khởi động:

1. `@EnableAutoConfiguration` được kích hoạt.
2. Spring Boot phát hiện `spring-boot-starter-web`.
3. `WebMvcAutoConfiguration` được nạp.
4. `@ConditionalOnClass(Servlet.class)` được kiểm tra.
5. Điều kiện thỏa mãn.
6. Spring Boot tự động tạo:

* Embedded Tomcat.
* DispatcherServlet.
* Jackson ObjectMapper.
* MessageConverter.
* Spring MVC.

Nhờ đó, chỉ cần viết:

```java
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello Spring Boot";
    }

}
```

là ứng dụng đã có thể chạy được mà không cần cấu hình XML hay khai báo Servlet như trong Spring Framework truyền thống.
