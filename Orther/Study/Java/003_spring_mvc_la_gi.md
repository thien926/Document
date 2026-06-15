# Spring MVC là gì?

**Spring MVC (Model-View-Controller)** là một module thuộc **Spring Framework**, được thiết kế chuyên biệt để xây dựng các ứng dụng Web và RESTful API theo mô hình kiến trúc **MVC (Model-View-Controller)**.

Spring MVC cung cấp cơ chế phân tách rõ ràng giữa:

* Dữ liệu của ứng dụng (Model).
* Giao diện hiển thị (View).
* Thành phần xử lý yêu cầu và điều hướng (Controller).

Nhờ đó, mã nguồn trở nên:

* Dễ bảo trì.
* Dễ mở rộng.
* Dễ kiểm thử.
* Giảm sự phụ thuộc giữa các thành phần.

---

# Các thành phần chính của Spring MVC

Spring MVC được xây dựng dựa trên ba thành phần cốt lõi:

1. Controller
2. Model
3. View

---

# 1. Controller

## Khái niệm

Controller là thành phần chịu trách nhiệm:

* Tiếp nhận các yêu cầu (Request) từ người dùng.
* Xử lý yêu cầu.
* Gọi tầng nghiệp vụ (Service).
* Trả về dữ liệu hoặc View phù hợp.

Trong Spring MVC, Controller thường được đánh dấu bằng các annotation:

* `@Controller`
* `@RestController`

## Vai trò của Controller

Controller có nhiệm vụ:

1. Nhận HTTP Request.
2. Gọi Service xử lý nghiệp vụ.
3. Nhận kết quả từ Service.
4. Trả về dữ liệu hoặc View cho người dùng.

---

# 2. Model

## Khái niệm

Model đại diện cho dữ liệu của ứng dụng.

Thông thường, Model là các đối tượng Java (POJO) chứa:

* Thuộc tính (Properties).
* Getter/Setter.
* Các phương thức thao tác dữ liệu.

Model phản ánh các thực thể trong hệ thống.

Ví dụ:

* User
* Product
* Order
* Employee

## Vai trò của Model

Model chịu trách nhiệm:

* Lưu trữ dữ liệu.
* Trao đổi dữ liệu giữa các tầng.
* Đại diện cho các thực thể nghiệp vụ của hệ thống.

---

# 3. View

## Khái niệm

View định nghĩa cách dữ liệu được hiển thị cho người dùng.

Nó chịu trách nhiệm:

* Hiển thị giao diện.
* Trình bày dữ liệu nhận được từ Controller.

Spring MVC hỗ trợ nhiều công nghệ View khác nhau.

## Vai trò của View

View chỉ chịu trách nhiệm hiển thị dữ liệu.

View không chứa:

* Logic nghiệp vụ.
* Logic truy cập cơ sở dữ liệu.

Điều này giúp giao diện và nghiệp vụ được tách biệt rõ ràng.

---

# Mô hình MVC

Spring MVC áp dụng mô hình:

```text
Model - View - Controller
```

Trong đó:

* **Model**: dữ liệu của ứng dụng.
* **View**: giao diện hiển thị.
* **Controller**: xử lý yêu cầu và điều hướng.

---

## Luồng hoạt động của MVC

```text
Người dùng
    ↓
Controller
    ↓
Model
    ↓
View
    ↓
Người dùng
```

---

### Ví dụ

Người dùng truy cập:

```text
GET /users/1
```

Quá trình xử lý:

```text
Request
    ↓
UserController
    ↓
UserService
    ↓
User
    ↓
View hoặc JSON Response
    ↓
Client
```

---

## Lợi ích của mô hình MVC

Spring MVC giúp tách biệt:

| Thành phần | Vai trò                |
| ---------- | ---------------------- |
| Model      | Chứa dữ liệu           |
| View       | Hiển thị dữ liệu       |
| Controller | Điều khiển luồng xử lý |

Nhờ đó:

* Mã nguồn dễ đọc.
* Dễ bảo trì.
* Dễ mở rộng.
* Giảm sự phụ thuộc giữa các thành phần.

---

# DispatcherServlet

## Khái niệm

**DispatcherServlet** là thành phần trung tâm (Front Controller) của Spring MVC.

Nó chịu trách nhiệm:

* Nhận tất cả các HTTP Request từ người dùng.
* Xác định Controller phù hợp.
* Chuyển request đến Controller tương ứng.
* Nhận kết quả trả về.
* Chuyển tiếp đến View hoặc trả về Response.

---

## Luồng xử lý của DispatcherServlet

```text
Client
    ↓
DispatcherServlet
    ↓
Controller
    ↓
Model
    ↓
View
    ↓
Response
```

---

### Ví dụ

Người dùng gửi:

```http
GET /users/1
```

DispatcherServlet sẽ:

1. Nhận request.
2. Tìm phương thức phù hợp trong Controller.
3. Gọi phương thức đó.
4. Nhận dữ liệu trả về.
5. Chuyển dữ liệu thành View hoặc JSON.
6. Trả Response cho Client.

---

## Vai trò của DispatcherServlet

DispatcherServlet đóng vai trò là trung tâm điều phối của toàn bộ ứng dụng Spring MVC.

Nó giúp:

* Tập trung xử lý Request.
* Điều hướng tới Controller phù hợp.
* Quản lý luồng xử lý của ứng dụng.

---

# Annotation-based Configuration

## Khái niệm

Spring MVC hỗ trợ cấu hình dựa trên Annotation thay vì cấu hình bằng XML.

Các Annotation phổ biến dùng để định nghĩa endpoint và xử lý HTTP Request bao gồm:

* `@RequestMapping`
* `@GetMapping`
* `@PostMapping`

---

# @RequestMapping

Được sử dụng để ánh xạ URL tới Controller hoặc Method.

Ví dụ:

```java
@RestController
@RequestMapping("/users")
public class UserController {

}
```

Mọi endpoint trong Controller đều bắt đầu bằng:

```text
/users
```

---

# @GetMapping

Được sử dụng để xử lý HTTP GET.

Ví dụ:

```java
@GetMapping("/{id}")
public User getUser() {

    return new User(1L, "Tom");

}
```

Request:

```http
GET /users/1
```

---

# @PostMapping

Được sử dụng để xử lý HTTP POST.

Ví dụ:

```java
@PostMapping
public String createUser() {

    return "Success";

}
```

Request:

```http
POST /users
```

---

## Ví dụ hoàn chỉnh

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @GetMapping("/{id}")
    public User getUser() {

        return new User(1L, "Tom");

    }

    @PostMapping
    public String createUser() {

        return "Created";

    }

}
```

Các endpoint:

| HTTP Method | URL      |
| ----------- | -------- |
| GET         | /users/1 |
| POST        | /users   |

---

# Tổng kết

Spring MVC là module của Spring Framework dùng để xây dựng ứng dụng Web theo mô hình **Model-View-Controller (MVC)**.

Nhờ đó, mã nguồn trở nên:

* Dễ bảo trì.
* Dễ mở rộng.
* Dễ kiểm thử.
* Giảm sự phụ thuộc giữa các thành phần.

Các thành phần chính của Spring MVC gồm:

| Thành phần                     | Chức năng                                                                                       |
| ------------------------------ | ----------------------------------------------------------------------------------------------- |
| Controller                     | Xử lý yêu cầu từ người dùng và trả về dữ liệu                                                   |
| Model                          | Đại diện cho dữ liệu của ứng dụng                                                               |
| View                           | Định nghĩa cách dữ liệu được hiển thị                                                           |
| DispatcherServlet              | Thành phần trung tâm tiếp nhận và điều phối Request                                             |
| Annotation-based Configuration | Cấu hình endpoint thông qua các annotation như `@RequestMapping`, `@GetMapping`, `@PostMapping` |

Nhờ mô hình MVC, Spring MVC giúp tách biệt rõ ràng giữa dữ liệu, giao diện và luồng xử lý, từ đó làm cho ứng dụng dễ bảo trì, dễ mở rộng và dễ phát triển hơn.
