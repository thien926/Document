# Các cách để định nghĩa một Bean trong Spring

Bean là một đối tượng được Spring IoC Container tạo ra, quản lý và lưu trữ trong `ApplicationContext`. Có nhiều cách để khai báo Bean, trong đó ba cách phổ biến nhất là sử dụng Annotation, sử dụng `@Configuration` kết hợp `@Bean`, và cấu hình bằng XML.

---

# 1. Sử dụng các Annotation chuyên dụng

Đây là cách được sử dụng phổ biến nhất trong Spring Boot hiện nay.

Spring sẽ thực hiện **Component Scanning** để quét các class trong project. Nếu phát hiện các annotation phù hợp, Spring sẽ tự động tạo Bean tương ứng.

## Các Annotation thường dùng

### `@Component`

Là annotation tổng quát dùng để đánh dấu một class là Bean do Spring quản lý.

---

### `@Service`

Được sử dụng cho lớp xử lý nghiệp vụ (Business Logic).

Ngoài việc tạo Bean giống `@Component`, annotation này còn giúp mã nguồn dễ đọc và thể hiện rõ vai trò của class.

---

### `@Repository`

Được sử dụng cho lớp truy cập dữ liệu (DAO, Repository).

Ngoài chức năng tạo Bean, `@Repository` còn hỗ trợ cơ chế chuyển đổi Exception của tầng database sang Exception của Spring.

---

### `@Controller`

Được sử dụng cho các lớp xử lý request trong ứng dụng Web MVC.

---

## Cơ chế hoạt động

Khi ứng dụng khởi động:

1. Spring Boot thực hiện Component Scanning.
2. Tìm các class có annotation như:

* `@Component`
* `@Service`
* `@Repository`
* `@Controller`

3. Tạo đối tượng tương ứng.
4. Đưa đối tượng đó vào `ApplicationContext`.
5. Đối tượng này trở thành một Bean của Spring.

---

# 2. Sử dụng `@Configuration` và `@Bean`

Đây là cách khai báo Bean ở cấp Method.

Cách này thường được sử dụng khi:

* Muốn tự cấu hình Bean.
* Bean thuộc thư viện bên ngoài và không thể sửa source code để thêm annotation.
* Cần khởi tạo đối tượng với các tham số đặc biệt.

---

## 2.1 Annotation `@Configuration`

`@Configuration` được đặt trên class để thông báo rằng đây là lớp cấu hình của Spring.

---

## 2.2 Annotation `@Bean`

`@Bean` được đặt trên một phương thức bên trong class cấu hình.

Giá trị trả về của phương thức sẽ trở thành Bean trong Spring Container.

---

## Cơ chế hoạt động

Khi ứng dụng khởi động:

1. Spring phát hiện class có `@Configuration`.
2. Tìm các phương thức có annotation `@Bean`.
3. Thực thi các phương thức đó.
4. Lấy đối tượng trả về.
5. Đăng ký đối tượng vào Spring Container dưới dạng Bean.

---

# 3. Định nghĩa Bean bằng file XML

Đây là cách cấu hình truyền thống của Spring trước khi Annotation trở nên phổ biến.

Bean được khai báo thông qua các thẻ `<bean>` trong file XML.

```xml
<beans>
    <bean
        id="emailSender"
        class="com.example.EmailSender"/>
</beans>
```

Trong đó:

* `id`: tên của Bean.
* `class`: class sẽ được khởi tạo.

---

## Cơ chế hoạt động

Khi ứng dụng khởi động:

1. Spring đọc file XML.
2. Phân tích các thẻ `<bean>`.
3. Tạo đối tượng tương ứng.
4. Đưa các đối tượng đó vào IoC Container.
5. Các đối tượng này trở thành Bean của Spring.

---

# So sánh các cách định nghĩa Bean

| Cách khai báo                             | Mức độ phổ biến | Cấu hình            | Trường hợp sử dụng                                       |
| ----------------------------------------- | --------------- | ------------------- | -------------------------------------------------------- |
| Annotation (`@Component`, `@Service`,...) | Rất phổ biến    | Ít                  | Hầu hết các ứng dụng Spring Boot                         |
| `@Configuration` + `@Bean`                | Phổ biến        | Linh hoạt           | Bean cần cấu hình thủ công hoặc thuộc thư viện bên ngoài |
| XML `<bean>`                              | Ít dùng         | Tách biệt khỏi code | Các dự án Spring cũ hoặc yêu cầu cấu hình bằng XML       |

---

# Ví dụ

## Ví dụ 1: Sử dụng `@Service`

```java
@Service
public class UserService {

    public void createUser() {
        System.out.println("Create user");
    }

}
```

Spring sẽ tự động phát hiện và tạo Bean `UserService`.

---

## Ví dụ 2: Sử dụng `@Configuration` và `@Bean`

```java
@Configuration
public class AppConfig {

    @Bean
    public UserService userService() {
        return new UserService();
    }

}
```

Bean được tạo thông qua phương thức:

```java
userService()
```

---

## Ví dụ 3: Sử dụng XML

```xml
<beans>

    <bean
        id="userService"
        class="com.example.UserService"/>

</beans>
```

Spring sẽ khởi tạo đối tượng `UserService` và quản lý nó như một Bean.

---

# Tổng kết

* Bean có thể được định nghĩa theo ba cách chính:

  1. **Sử dụng Annotation** (`@Component`, `@Service`, `@Repository`, `@Controller`), Spring tự động phát hiện thông qua Component Scanning.

  2. **Sử dụng `@Configuration` kết hợp `@Bean`**, trong đó đối tượng trả về của phương thức có `@Bean` sẽ được đăng ký vào Spring Container.

  3. **Sử dụng file XML với thẻ `<bean>`**, đây là cách cấu hình truyền thống của Spring.

* Trong các ứng dụng Spring Boot hiện đại, **Annotation là cách được sử dụng nhiều nhất**.

* `@Configuration` và `@Bean` thường được dùng khi cần cấu hình Bean một cách linh hoạt hoặc khi làm việc với các thư viện bên ngoài.

* Cấu hình XML hiện nay ít được sử dụng và chủ yếu xuất hiện trong các dự án Spring đời cũ.
