# Các cách DI (Dependency Injection) trong Spring

**Dependency Injection (DI)** là cơ chế Spring cung cấp đối tượng phụ thuộc (Dependency) cho một class thay vì class đó tự tạo bằng `new`. Điều này giúp code dễ mở rộng, dễ kiểm thử và giảm sự phụ thuộc giữa các thành phần.

Spring hỗ trợ 3 cách Inject Dependency phổ biến:

| Cách DI               | Annotation                                                | Khuyến nghị                            |
| --------------------- | --------------------------------------------------------- | -------------------------------------- |
| Constructor Injection | `@Autowired` trên constructor (hoặc bỏ qua từ Spring 4.3) | ⭐⭐⭐⭐⭐ Nên dùng                         |
| Setter Injection      | `@Autowired` trên setter                                  | ⭐⭐⭐ Dùng khi dependency không bắt buộc |
| Field Injection       | `@Autowired` trực tiếp trên biến                          | ⭐ Không khuyến khích                   |

---

# 1. Constructor Injection

Spring inject dependency thông qua constructor của class.

### Ví dụ

```java
@Service
public class UserService {

    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

Hoặc:

```java
@Service
public class UserService {

    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

(Từ Spring 4.3 trở lên, nếu class chỉ có một constructor thì không cần `@Autowired`.)

### Ưu điểm

* Dependency bắt buộc phải được khởi tạo.
* Có thể dùng `final`.
* Dễ viết Unit Test.
* Dễ phát hiện Circular Dependency.
* Immutable (không thay đổi dependency sau khi tạo object).

### Nhược điểm

* Constructor có thể dài nếu có quá nhiều dependency.

### Khi nào nên dùng?

* Hầu hết các trường hợp.
* Đây là cách được Spring khuyến nghị.

---

# 2. Setter Injection

Spring inject dependency thông qua phương thức setter.

### Ví dụ

```java
@Service
public class UserService {

    private UserRepository userRepository;

    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

### Ưu điểm

* Có thể thay đổi dependency sau khi object được tạo.
* Phù hợp với dependency không bắt buộc.

### Nhược điểm

* Object có thể ở trạng thái chưa hoàn chỉnh nếu setter chưa được gọi.
* Không thể dùng `final`.
* Khó đảm bảo tính bất biến (immutable).

### Khi nào nên dùng?

* Dependency tùy chọn (Optional Dependency).
* Khi muốn thay đổi dependency trong quá trình chạy.

Ví dụ:

```java
@Service
public class NotificationService {

    private EmailService emailService;

    @Autowired(required = false)
    public void setEmailService(EmailService emailService) {
        this.emailService = emailService;
    }
}
```

---

# 3. Field Injection

Spring inject trực tiếp vào biến thành viên.

### Ví dụ

```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;
}
```

### Ưu điểm

* Viết code ngắn gọn.
* Ít boilerplate.

### Nhược điểm

* Khó Unit Test.
* Không dùng được `final`.
* Dependency bị ẩn, khó nhận biết class phụ thuộc vào gì.
* Khó phát hiện Circular Dependency.
* Vi phạm nguyên tắc thiết kế tốt.

### Khi nào nên dùng?

* Demo, ví dụ nhỏ.
* Không nên dùng trong dự án thực tế.

---

# So sánh các cách DI

| Tiêu chí                         | Constructor Injection | Setter Injection | Field Injection |
| -------------------------------- | --------------------- | ---------------- | --------------- |
| Dependency bắt buộc              | ✔                     | ✖                | ✖               |
| Hỗ trợ `final`                   | ✔                     | ✖                | ✖               |
| Dễ Unit Test                     | ✔                     | Khá tốt          | Kém             |
| Immutable                        | ✔                     | ✖                | ✖               |
| Dễ phát hiện Circular Dependency | ✔                     | ✖                | ✖               |
| Có thể thay đổi dependency       | ✖                     | ✔                | ✖               |
| Khuyến nghị sử dụng              | ⭐⭐⭐⭐⭐                 | ⭐⭐⭐              | ⭐               |

---

# Thứ tự ưu tiên nên sử dụng

### 1. Constructor Injection ⭐⭐⭐⭐⭐

Phù hợp với gần như mọi trường hợp và là cách được Spring khuyến nghị.

```java
@Service
@RequiredArgsConstructor
public class UserService {

    private final UserRepository userRepository;
}
```

(Lombok `@RequiredArgsConstructor` sẽ tự sinh constructor.)

---

### 2. Setter Injection ⭐⭐⭐

Dùng cho dependency không bắt buộc hoặc cần thay đổi sau khi khởi tạo.

```java
@Autowired
public void setEmailService(EmailService emailService) {
    this.emailService = emailService;
}
```

---

### 3. Field Injection ⭐

Chỉ nên dùng trong ví dụ đơn giản hoặc code thử nghiệm.

```java
@Autowired
private UserRepository userRepository;
```

---

## Kết luận

Spring hỗ trợ ba cách Dependency Injection:

1. **Constructor Injection** → cách tốt nhất và được khuyến nghị.
2. **Setter Injection** → phù hợp với dependency tùy chọn.
3. **Field Injection** → đơn giản nhưng không được khuyến khích trong các dự án thực tế.

Trong các dự án Spring Boot hiện đại, **Constructor Injection kết hợp với Lombok `@RequiredArgsConstructor`** là cách được sử dụng phổ biến nhất.
