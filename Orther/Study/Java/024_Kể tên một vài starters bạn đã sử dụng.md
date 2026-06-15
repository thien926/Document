# Một vài Spring Boot Starters thường sử dụng

## 1. spring-boot-starter-web

### Mục đích

Starter này được sử dụng để xây dựng các ứng dụng web và RESTful API.

### Thành phần chính

Khi thêm dependency này, Spring Boot sẽ tự động bổ sung các thư viện cần thiết như:

* Spring MVC.
* Embedded Tomcat (máy chủ web nhúng).
* Jackson để chuyển đổi dữ liệu JSON.
* Các thư viện hỗ trợ HTTP và xử lý request/response.

### Chức năng chính

#### Xây dựng REST API

Cho phép tạo các endpoint để trao đổi dữ liệu thông qua HTTP.

#### Tích hợp máy chủ web nhúng

Ứng dụng có thể chạy trực tiếp mà không cần cài đặt Tomcat riêng.

#### Xử lý dữ liệu JSON

Đối tượng Java được tự động chuyển đổi sang JSON và ngược lại.

---

## 2. spring-boot-starter-data-jpa

### Mục đích

Starter này được sử dụng để làm việc với cơ sở dữ liệu quan hệ thông qua Spring Data JPA và Hibernate.

### Thành phần chính

* Spring Data JPA.
* Hibernate.
* Các thư viện JDBC cần thiết.

### Chức năng chính

#### Thao tác với cơ sở dữ liệu

Hỗ trợ các thao tác CRUD (Create, Read, Update, Delete).

#### Ánh xạ đối tượng

Các bảng trong cơ sở dữ liệu được ánh xạ thành các Entity trong Java.

#### Giảm lượng mã nguồn

Nhiều câu lệnh SQL phổ biến được tự động sinh thông qua Repository.

---

## 3. spring-boot-starter-security

### Mục đích

Starter này được sử dụng để bổ sung các tính năng bảo mật cho ứng dụng.

### Thành phần chính

* Spring Security.
* Các cơ chế xác thực (Authentication).
* Các cơ chế phân quyền (Authorization).

### Chức năng chính

#### Xác thực người dùng

Kiểm tra danh tính người dùng trước khi cho phép truy cập hệ thống.

#### Phân quyền

Giới hạn quyền truy cập vào từng chức năng hoặc tài nguyên.

#### Bảo vệ ứng dụng

Giúp ngăn chặn các truy cập trái phép và tăng cường tính an toàn cho hệ thống.

---

## 4. spring-boot-starter-test

### Mục đích

Starter này cung cấp các thư viện cần thiết để kiểm thử ứng dụng Spring Boot.

### Thành phần chính

* JUnit.
* Mockito.
* Spring Test.
* Các thư viện hỗ trợ kiểm thử khác.

### Chức năng chính

#### Unit Test

Kiểm tra từng thành phần riêng lẻ của ứng dụng.

#### Integration Test

Kiểm tra sự phối hợp giữa nhiều thành phần với nhau.

#### Mock dữ liệu

Cho phép mô phỏng các đối tượng hoặc dịch vụ phụ thuộc nhằm phục vụ việc kiểm thử.

---

## 5. spring-boot-starter-actuator

### Mục đích

Starter này được sử dụng để giám sát và quản lý ứng dụng trong môi trường thực tế (Production).

### Thành phần chính

Cung cấp nhiều endpoint phục vụ việc theo dõi trạng thái hệ thống.

### Chức năng chính

#### Theo dõi tình trạng ứng dụng

Kiểm tra ứng dụng đang hoạt động bình thường hay gặp sự cố.

#### Thu thập thông tin hệ thống

Hiển thị thông tin về bộ nhớ, CPU, các Bean, cấu hình và nhiều thông tin khác.

#### Hỗ trợ vận hành

Giúp quản trị viên dễ dàng giám sát và bảo trì ứng dụng.

---

# Tổng kết

| Starter                          | Mục đích chính                                           |
| -------------------------------- | -------------------------------------------------------- |
| **spring-boot-starter-web**      | Xây dựng ứng dụng Web và RESTful API                     |
| **spring-boot-starter-data-jpa** | Làm việc với cơ sở dữ liệu quan hệ bằng JPA và Hibernate |
| **spring-boot-starter-security** | Bổ sung các chức năng xác thực và phân quyền             |
| **spring-boot-starter-test**     | Cung cấp các thư viện phục vụ kiểm thử ứng dụng          |
| **spring-boot-starter-actuator** | Giám sát và quản lý ứng dụng trong môi trường Production |

# Ví dụ

### Khai báo dependency trong Maven

```xml
<dependencies>

    <!-- Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- JPA -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- Security -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>

    <!-- Test -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

    <!-- Actuator -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>

</dependencies>
```

Ví dụ trên thể hiện một cấu hình phổ biến cho ứng dụng Spring Boot, trong đó:

* `spring-boot-starter-web` dùng để xây dựng REST API.
* `spring-boot-starter-data-jpa` dùng để truy cập cơ sở dữ liệu.
* `spring-boot-starter-security` dùng để bảo mật hệ thống.
* `spring-boot-starter-test` dùng để viết Unit Test và Integration Test.
* `spring-boot-starter-actuator` dùng để giám sát và quản lý ứng dụng khi triển khai thực tế.
