# Spring Framework là gì?

**Spring Framework** là một framework mã nguồn mở phổ biến dành cho nền tảng Java, được thiết kế nhằm đơn giản hóa quá trình phát triển các ứng dụng doanh nghiệp (Enterprise Applications).

Spring cung cấp nhiều cơ chế và thư viện hỗ trợ lập trình viên xây dựng các ứng dụng:

* Web Application
* RESTful API
* Hệ thống doanh nghiệp (Enterprise System)
* Các ứng dụng tích hợp cơ sở dữ liệu

Một trong những ưu điểm lớn nhất của Spring là giúp giảm sự phụ thuộc giữa các thành phần trong hệ thống, từ đó làm cho mã nguồn dễ mở rộng, dễ kiểm thử và dễ bảo trì hơn.

---

# Hai nguyên lý cốt lõi của Spring Framework

Spring Framework được xây dựng dựa trên hai nguyên lý quan trọng:

1. **Inversion of Control (IoC) và Dependency Injection (DI)**
2. **Aspect-Oriented Programming (AOP)**

---

# 1. Inversion of Control (IoC) và Dependency Injection (DI)

## Inversion of Control (IoC)

### Khái niệm

IoC (Inversion of Control) là cơ chế chuyển quyền kiểm soát việc tạo và quản lý đối tượng từ lập trình viên sang Spring Framework.

Thay vì tự khởi tạo các đối tượng bằng từ khóa `new`, Spring sẽ chịu trách nhiệm:

* Tạo đối tượng.
* Quản lý vòng đời của đối tượng.
* Cung cấp đối tượng cho các thành phần khác khi cần.

---

### Không sử dụng IoC

```java
public class UserService {

    private UserRepository repository =
            new UserRepository();

}
```

Trong trường hợp này:

* `UserService` tự tạo ra `UserRepository`.
* Hai lớp phụ thuộc chặt chẽ vào nhau.
* Khó mở rộng và khó kiểm thử.

---

### Sử dụng IoC

```java
@Service
public class UserService {

    private final UserRepository repository;

    public UserService(UserRepository repository) {
        this.repository = repository;
    }

}
```

Spring sẽ:

1. Tạo `UserRepository`.
2. Tạo `UserService`.
3. Tự động truyền `UserRepository` vào `UserService`.

Lập trình viên không cần:

```java
new UserRepository()
```

---

## Dependency Injection (DI)

### Khái niệm

Dependency Injection là cơ chế Spring tự động "tiêm" (inject) các đối tượng phụ thuộc vào nhau.

Ví dụ:

```text
UserService
    ↓
UserRepository
```

`UserService` cần sử dụng `UserRepository`.

Spring sẽ tự động cung cấp `UserRepository` cho `UserService`.

---

### Ví dụ

#### UserRepository

```java
@Repository
public class UserRepository {

}
```

#### UserService

```java
@Service
public class UserService {

    private final UserRepository repository;

    public UserService(UserRepository repository) {
        this.repository = repository;
    }

}
```

Spring tự động thực hiện:

```java
UserRepository repository = new UserRepository();

UserService service =
        new UserService(repository);
```

---

## Lợi ích của IoC và DI

* Giảm sự phụ thuộc giữa các lớp.
* Tăng khả năng tái sử dụng mã nguồn.
* Dễ mở rộng hệ thống.
* Dễ kiểm thử (Unit Test).
* Không cần quản lý thủ công việc tạo đối tượng.

---

# 2. Aspect-Oriented Programming (AOP)

## Khái niệm

AOP (Aspect-Oriented Programming) là kỹ thuật giúp tách các chức năng mang tính hệ thống ra khỏi luồng xử lý nghiệp vụ chính của ứng dụng.

Các chức năng này thường bao gồm:

* Logging.
* Bảo mật.
* Quản lý transaction.
* Kiểm tra quyền truy cập.
* Theo dõi và ghi nhận hoạt động của hệ thống.

---

## Ví dụ

Giả sử có phương thức:

```java
public void transferMoney() {

    // xử lý chuyển tiền

}
```

Ngoài nghiệp vụ chính, hệ thống còn cần:

* Ghi log.
* Kiểm tra quyền.
* Mở transaction.
* Commit transaction.

Nếu viết trực tiếp:

```java
public void transferMoney() {

    log.info("Start");

    checkPermission();

    beginTransaction();

    // xử lý chuyển tiền

    commit();

}
```

Logic nghiệp vụ và logic hệ thống bị trộn lẫn với nhau.

---

### Khi sử dụng AOP

```java
public void transferMoney() {

    // xử lý chuyển tiền

}
```

Các chức năng:

* Logging
* Security
* Transaction

được xử lý riêng biệt bởi Spring.

Nhờ đó:

* Mã nguồn rõ ràng hơn.
* Dễ bảo trì.
* Dễ tái sử dụng.

---

# Các mô-đun quan trọng của Spring Framework

Spring Framework được chia thành nhiều module khác nhau. Một số module quan trọng nhất gồm:

---

# 1. Spring Core Container

## Chức năng

Đây là module nền tảng của Spring Framework.

Spring Core Container cung cấp:

* Inversion of Control (IoC).
* Dependency Injection (DI).
* Quản lý vòng đời của các đối tượng (Bean).

# 2. Spring MVC

## Chức năng

Spring MVC sử dụng mô hình:

```text
Model - View - Controller
```

để xây dựng:

* Ứng dụng Web.
* RESTful API.

# 3. Spring Data / Data Access

## Chức năng

Module này hỗ trợ làm việc với cơ sở dữ liệu thông qua:

* JDBC.
* ORM.
* Hibernate.

Giúp đơn giản hóa việc:

* Truy vấn dữ liệu.
* Thêm dữ liệu.
* Cập nhật dữ liệu.
* Xóa dữ liệu.

## Công nghệ được hỗ trợ

* JDBC
* Hibernate
* JPA
* ORM

---

# 4. Spring Security

## Chức năng

Spring Security cung cấp cơ chế bảo mật toàn diện cho ứng dụng.

Bao gồm:

### Authentication (Xác thực)

Trả lời câu hỏi:

> Người dùng là ai?

Ví dụ:

* Username/Password.
* JWT.
* OAuth2.

---

### Authorization (Phân quyền)

Trả lời câu hỏi:

> Người dùng được phép làm gì?

Ví dụ:

* ADMIN được phép xóa dữ liệu.
* USER chỉ được xem dữ liệu.

Chỉ người dùng có quyền `ADMIN` mới được phép thực hiện phương thức này.

---

# Tổng kết

Spring Framework là framework Java phổ biến dùng để phát triển các ứng dụng doanh nghiệp.

Hai nguyên lý cốt lõi của Spring gồm:

1. **IoC (Inversion of Control) và DI (Dependency Injection)**

* Tự động quản lý và tiêm các đối tượng phụ thuộc.
* Giảm sự phụ thuộc giữa các lớp.

2. **AOP (Aspect-Oriented Programming)**

* Tách các chức năng mang tính hệ thống khỏi nghiệp vụ chính.
* Giúp mã nguồn dễ bảo trì và tái sử dụng.

Các module quan trọng nhất:

| Module                    | Chức năng                                                    |
| ------------------------- | ------------------------------------------------------------ |
| Spring Core Container     | Cung cấp IoC và DI                                           |
| Spring MVC                | Xây dựng ứng dụng Web và RESTful API                         |
| Spring Data / Data Access | Hỗ trợ truy cập cơ sở dữ liệu thông qua JDBC, ORM, Hibernate |
| Spring Security           | Xác thực và phân quyền người dùng                            |

Tài liệu trên tập trung đúng vào những nội dung cốt lõi và quan trọng nhất của Spring Framework.
