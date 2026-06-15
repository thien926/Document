## Bean trong Spring là gì?

## 1. Khái niệm Bean trong Spring

Bean là một đối tượng (**Object**) được khởi tạo, cấu hình và quản lý vòng đời bởi **Spring IoC Container**, thay vì lập trình viên tự tạo bằng từ khóa `new`.

Nói cách khác, mọi đối tượng được Spring quản lý bên trong **ApplicationContext** đều được gọi là **Bean**.

Ví dụ, các lớp được đánh dấu bằng:

* `@Service`
* `@Repository`
* `@Controller`
* `@RestController`
* `@Component`

khi ứng dụng khởi động sẽ được Spring tạo ra và lưu vào IoC Container dưới dạng Bean.

---

## 2. Bean được quản lý bởi Spring IoC Container

Spring sử dụng **IoC Container (ApplicationContext)** để:

### Khởi tạo đối tượng

Spring tự tạo instance của class.

Thay vì:

```java
UserService userService = new UserService();
```

Spring sẽ tự tạo:

```java
UserService userService;
```

và lưu đối tượng này vào ApplicationContext.

---

### Cấu hình đối tượng

Spring có thể tự động:

* Gán giá trị cho thuộc tính.
* Nạp dependency cần thiết.
* Thực hiện các cấu hình được khai báo.

---

### Quản lý vòng đời đối tượng

Spring chịu trách nhiệm:

1. Tạo Bean.
2. Khởi tạo Bean.
3. Đưa Bean vào ApplicationContext.
4. Cung cấp Bean cho các nơi cần sử dụng.
5. Hủy Bean khi ứng dụng kết thúc (nếu cần).

---

## 3. ApplicationContext là nơi chứa các Bean

ApplicationContext có thể hiểu như một "kho chứa" các Bean của ứng dụng.

Khi ứng dụng Spring Boot khởi động:

1. Spring quét các class.
2. Tìm các annotation như:

* `@Component`
* `@Service`
* `@Repository`
* `@Controller`
* `@RestController`

3. Tạo object tương ứng.
4. Đưa các object này vào ApplicationContext.

Từ thời điểm đó, các Bean có thể được sử dụng ở bất kỳ đâu thông qua cơ chế Dependency Injection.

---

## 4. Các Bean phổ biến trong Spring

### 4.1 `@Service`

Dùng cho lớp xử lý nghiệp vụ (Business Logic).

```java
@Service
public class UserService {

}
```

Spring sẽ tạo một Bean kiểu `UserService`.

---

### 4.2 `@Repository`

Dùng cho lớp truy cập cơ sở dữ liệu.

```java
@Repository
public class UserRepository {

}
```

Spring tạo Bean `UserRepository`.

---

### 4.3 `@Controller`

Dùng cho Controller trong ứng dụng MVC.

```java
@Controller
public class HomeController {

}
```

Spring tạo Bean `HomeController`.

---

### 4.4 `@RestController`

Dùng cho REST API.

```java
@RestController
public class UserController {

}
```

Spring tạo Bean `UserController`.

---

### 4.5 `@Component`

Là annotation tổng quát để đánh dấu một class trở thành Bean.

```java
@Component
public class EmailUtil {

}
```

Spring tạo Bean `EmailUtil`.

---

## 5. Bean có thể được Inject vào nơi cần sử dụng

Sau khi Bean được tạo, Spring có thể tự động đưa Bean đó vào các class khác thông qua Dependency Injection.

Khi một class cần một Bean khác, Spring sẽ lấy Bean tương ứng từ ApplicationContext và cung cấp cho class đó.

Nhờ vậy:

* Không cần tự tạo đối tượng bằng `new`.
* Giảm sự phụ thuộc giữa các class.
* Dễ mở rộng và bảo trì hệ thống.

---

## 6. Điểm khác nhau giữa Object thông thường và Bean

| Object thông thường                | Bean                         |
| ---------------------------------- | ---------------------------- |
| Tự tạo bằng `new`                  | Do Spring tạo                |
| Lập trình viên tự quản lý          | Spring quản lý               |
| Không nằm trong ApplicationContext | Nằm trong ApplicationContext |
| Không hỗ trợ Dependency Injection  | Có thể được Inject           |
| Tự quản lý vòng đời                | Spring quản lý vòng đời      |

---

## Ví dụ

### Ví dụ 1: Bean `UserService`

```java
@Service
public class UserService {

    public void saveUser() {
        System.out.println("Save user");
    }
}
```

Khi ứng dụng khởi động:

1. Spring phát hiện annotation `@Service`.
2. Tạo object `UserService`.
3. Đưa object này vào ApplicationContext.
4. Đối tượng đó trở thành một Bean.

---

### Ví dụ 2: Bean được Inject vào Controller

```java
@Service
public class UserService {

    public String getUser() {
        return "Tom";
    }
}
```

```java
@RestController
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping("/user")
    public String getUser() {
        return userService.getUser();
    }
}
```

Quá trình diễn ra:

1. Spring tạo Bean `UserService`.
2. Spring tạo Bean `UserController`.
3. Khi tạo `UserController`, Spring thấy cần `UserService`.
4. Spring lấy Bean `UserService` trong ApplicationContext.
5. Bean này được đưa vào thuộc tính `userService`.
6. `UserController` có thể sử dụng `UserService` mà không cần:

```java
UserService userService = new UserService();
```

---

## Tổng kết

* **Bean** là một đối tượng được **Spring IoC Container khởi tạo, cấu hình và quản lý vòng đời**.
* Mọi đối tượng được lưu trong **ApplicationContext** đều được gọi là Bean.
* Các annotation như `@Service`, `@Repository`, `@Controller`, `@RestController`, `@Component` thường tạo ra Bean.
* Spring tự động tạo và quản lý Bean thay vì lập trình viên phải dùng từ khóa `new`.
* Bean có thể được cung cấp cho các class khác thông qua **Dependency Injection**.
* Nhờ cơ chế Bean, ứng dụng trở nên ít phụ thuộc, dễ mở rộng và dễ bảo trì hơn.
