# JpaRepository là gì?

## Khái niệm

`JpaRepository` là một interface cốt lõi của Spring Data JPA, cung cấp sẵn các phương thức chuẩn để thực hiện các thao tác CRUD (Create, Read, Update, Delete) và phân trang dữ liệu từ cơ sở dữ liệu mà lập trình viên không cần tự viết câu lệnh SQL hoặc cài đặt Repository thủ công.

`JpaRepository` kế thừa nhiều interface khác của Spring Data, nhờ đó cung cấp rất nhiều chức năng sẵn có.

---

# Cấu trúc kế thừa

```text
Repository
    ↑
CrudRepository
    ↑
PagingAndSortingRepository
    ↑
JpaRepository
```

Nhờ kế thừa từ các interface trên, `JpaRepository` hỗ trợ:

* CRUD dữ liệu.
* Sắp xếp dữ liệu.
* Phân trang dữ liệu.
* Thao tác với Entity theo chuẩn JPA.
* Batch Insert, Batch Delete.
* Flush dữ liệu xuống Database.

---

# Khai báo JpaRepository

Một Repository thông thường được định nghĩa bằng cách kế thừa `JpaRepository`:

```java
public interface UserRepository
        extends JpaRepository<User, Long> {

}
```

Trong đó:

* `User`: Entity cần thao tác.
* `Long`: Kiểu dữ liệu của khóa chính (Primary Key).

Spring Data JPA sẽ tự động tạo implementation cho interface này khi ứng dụng khởi động.

Lập trình viên không cần viết:

```java
public class UserRepositoryImpl {
    ...
}
```

---

# Các chức năng chính

## 1. CRUD dữ liệu

`JpaRepository` cung cấp sẵn các phương thức cơ bản:

### Thêm hoặc cập nhật dữ liệu

```java
save()
```

Nếu Entity chưa tồn tại:

* Thực hiện INSERT.

Nếu Entity đã tồn tại:

* Thực hiện UPDATE.

---

### Tìm theo ID

```java
findById()
```

Trả về:

```java
Optional<T>
```

để tránh lỗi `NullPointerException`.

---

### Lấy toàn bộ dữ liệu

```java
findAll()
```

Trả về danh sách các Entity.

---

### Xóa dữ liệu

```java
delete()
deleteById()
deleteAll()
```

---

## 2. Phân trang và sắp xếp

Nhờ kế thừa từ `PagingAndSortingRepository`, `JpaRepository` hỗ trợ:

### Phân trang

```java
findAll(Pageable pageable)
```

Giúp lấy dữ liệu theo từng trang thay vì đọc toàn bộ bảng.

---

### Sắp xếp

```java
findAll(Sort sort)
```

Cho phép sắp xếp dữ liệu theo một hoặc nhiều cột.

---

## 3. Các chức năng riêng của JpaRepository

Ngoài CRUD thông thường, `JpaRepository` còn cung cấp thêm:

### flush()

Đồng bộ dữ liệu từ Persistence Context xuống Database ngay lập tức.

```java
flush()
```

---

### saveAndFlush()

Lưu Entity và thực hiện flush ngay sau đó.

```java
saveAndFlush()
```

---

### deleteAllInBatch()

Xóa nhiều bản ghi bằng Batch.

```java
deleteAllInBatch()
```

Giúp tăng hiệu năng khi xử lý số lượng lớn dữ liệu.

---

### getReferenceById()

Lấy Proxy của Entity mà chưa truy vấn Database ngay lập tức.

```java
getReferenceById()
```

Dữ liệu chỉ được truy vấn khi thực sự cần sử dụng.

---

# Query Method

Một ưu điểm lớn của Spring Data JPA là khả năng tự sinh câu truy vấn dựa trên tên phương thức.

Ví dụ:

```java
findByName()
```

Spring Data JPA sẽ tự tạo câu lệnh tương đương:

```sql
SELECT * FROM user
WHERE name = ?
```

---

### Tìm theo Email

```java
findByEmail(String email)
```

---

### Tìm theo tuổi lớn hơn

```java
findByAgeGreaterThan(Integer age)
```

---

### Tìm theo trạng thái

```java
findByStatus(String status)
```

---

### Kiểm tra sự tồn tại

```java
existsByEmail(String email)
```

---

# Lợi ích của JpaRepository

## Giảm lượng mã nguồn

Không cần:

* Viết DAO.
* Viết JDBC.
* Viết SQL CRUD cơ bản.

---

## Tăng tốc độ phát triển

Spring Data JPA tự sinh implementation cho Repository.

Lập trình viên chỉ cần khai báo interface.

---

## Dễ bảo trì

Mã nguồn ngắn gọn, rõ ràng và ít trùng lặp.

---

## Hỗ trợ phân trang và sắp xếp

Không cần tự xử lý logic phân trang thủ công.

---

## Tích hợp chặt chẽ với JPA và Hibernate

Tận dụng được các tính năng:

* Persistence Context.
* Transaction.
* Lazy Loading.
* Dirty Checking.
* Cascade.

---

# Tổng kết

* `JpaRepository` là interface cốt lõi của Spring Data JPA.
* Nó cung cấp sẵn các thao tác CRUD, phân trang và sắp xếp dữ liệu.
* Spring Data JPA sẽ tự động sinh implementation cho Repository khi ứng dụng khởi động.
* Lập trình viên không cần tự viết SQL cho các thao tác phổ biến.
* `JpaRepository` kế thừa từ `CrudRepository` và `PagingAndSortingRepository`, đồng thời bổ sung thêm nhiều chức năng đặc thù của JPA như `flush()`, `saveAndFlush()` và `deleteAllInBatch()`.
* Một trong những ưu điểm lớn nhất của `JpaRepository` là khả năng tự sinh câu truy vấn thông qua tên phương thức, giúp giảm đáng kể lượng mã nguồn cần viết.

# Ví dụ

### Entity

```java
@Entity
public class User {

    @Id
    private Long id;

    private String name;

    private String email;

}
```

### Repository

```java
public interface UserRepository
        extends JpaRepository<User, Long> {

    Optional<User> findByEmail(String email);

    List<User> findByName(String name);

    boolean existsByEmail(String email);

}
```

### Sử dụng

```java
@Service
@RequiredArgsConstructor
public class UserService {

    private final UserRepository userRepository;

    public User createUser(User user) {
        return userRepository.save(user);
    }

    public Optional<User> getUser(Long id) {
        return userRepository.findById(id);
    }

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }

}
```

Toàn bộ các thao tác CRUD ở trên đều được Spring Data JPA tự động thực hiện mà không cần viết bất kỳ câu lệnh SQL nào.
