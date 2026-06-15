# Spring Data JPA là gì?

## Khái niệm

**Spring Data JPA** là một mô-đun nằm trong hệ sinh thái **Spring Framework**.

Nó được xây dựng dựa trên JPA và cung cấp một lớp trừu tượng cấp cao giúp đơn giản hóa việc thao tác với cơ sở dữ liệu trong các ứng dụng Java.

Thay vì phải tự viết nhiều đoạn mã xử lý dữ liệu, lập trình viên chỉ cần định nghĩa các interface Repository, Spring Data JPA sẽ tự động tạo phần cài đặt tương ứng.

---

## Chức năng

Chức năng chính của Spring Data JPA là:

> Cung cấp một lớp abstraction nằm trên JPA nhằm giảm thiểu tối đa lượng code mẫu (boilerplate code).

Spring Data JPA giúp:

* Không cần tự cài đặt Repository.
* Không cần viết các thao tác CRUD cơ bản.
* Không cần tự tạo EntityManager để truy vấn dữ liệu.
* Tự động sinh phần code truy cập Database.

---

### Ví dụ

Giả sử có Entity:

```java
@Entity
@Table(name = "users")
public class User {

    @Id
    private Long id;

    private String name;

    private String email;
}
```

Nếu dùng JPA thuần, lập trình viên phải tự thao tác với `EntityManager`.

Trong Spring Data JPA, chỉ cần tạo interface:

```java
public interface UserRepository
        extends JpaRepository<User, Long> {

}
```

Spring Data JPA sẽ tự động tạo phần cài đặt phía sau.

---

## Lợi ích

### Chỉ cần kế thừa JpaRepository   

### Tự động cung cấp các hàm CRUD

Sau khi kế thừa `JpaRepository`, Repository sẽ có sẵn rất nhiều phương thức.

#### save()

Dùng để thêm mới hoặc cập nhật dữ liệu.

#### findById()

Tìm dữ liệu theo khóa chính.

#### findAll()

Lấy toàn bộ dữ liệu.

#### delete()

Xóa dữ liệu.

Nhờ đó, lập trình viên gần như không phải viết code CRUD cơ bản.

---

## Tính năng đặc biệt: Tạo truy vấn tự động từ tên hàm

Một trong những tính năng mạnh nhất của Spring Data JPA là:

> Tự động sinh câu truy vấn dựa trên tên phương thức.

Lập trình viên chỉ cần khai báo tên hàm theo quy tắc của Spring Data JPA.

Ví dụ:

```java
public interface UserRepository
        extends JpaRepository<User, Long> {

    User findByEmail(String email);

}
```

Không cần viết bất kỳ đoạn code cài đặt nào.

Khi gọi:

```java
User user =
        userRepository.findByEmail("a@gmail.com");
```

Spring Data JPA sẽ tự động sinh:

```sql
SELECT *
FROM users
WHERE email = 'a@gmail.com';
```

---

### Ví dụ khác

#### Tìm theo tên

```java
User findByName(String name);
```

SQL tương ứng:

```sql
SELECT *
FROM users
WHERE name = ?;
```

---

#### Tìm theo tên và email

```java
User findByNameAndEmail(
        String name,
        String email);
```

Spring Data JPA sinh:

```sql
SELECT *
FROM users
WHERE name = ?
AND email = ?;
```

---

#### Tìm tất cả User có cùng tên

```java
List<User> findByName(String name);
```

SQL:

```sql
SELECT *
FROM users
WHERE name = ?;
```

---

### Repository hoàn chỉnh

```java
public interface UserRepository
        extends JpaRepository<User, Long> {

    User findByEmail(String email);

    User findByNameAndEmail(
            String name,
            String email);

    List<User> findByName(String name);

}
```

Spring Data JPA sẽ tự động tạo phần cài đặt cho tất cả các phương thức trên mà không cần viết thêm code.

---

## Mối quan hệ với JPA và Hibernate

Có thể hình dung theo các tầng:

```text
Application
     ↓
Spring Data JPA
     ↓
JPA
     ↓
Hibernate
     ↓
Database
```

Trong đó:

* **Spring Data JPA** cung cấp abstraction và Repository.
* **JPA** định nghĩa các chuẩn thao tác dữ liệu.
* **Hibernate** hiện thực hóa chuẩn JPA và sinh SQL.
* **Database** là nơi lưu trữ dữ liệu thực tế.

---

## Ví dụ tổng quát

Entity:

```java
@Entity
@Table(name = "users")
public class User {

    @Id
    private Long id;

    private String name;

    private String email;
}
```

Repository:

```java
public interface UserRepository
        extends JpaRepository<User, Long> {

    User findByEmail(String email);

}
```

Service:

```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public User getUserByEmail(String email) {
        return userRepository.findByEmail(email);
    }

}
```

Khi gọi:

```java
userService.getUserByEmail("a@gmail.com");
```

Spring Data JPA sẽ tự động tạo truy vấn:

```sql
SELECT *
FROM users
WHERE email = 'a@gmail.com';
```

Mà không cần viết:

* Repository implementation.
* EntityManager.
* JPQL.
* SQL thuần.

---

# Tổng kết

* **Spring Data JPA** là một mô-đun thuộc Spring Framework.
* Chức năng chính của nó là cung cấp một lớp abstraction nằm trên JPA để giảm tối đa boilerplate code.
* Chỉ cần định nghĩa interface kế thừa `JpaRepository`, Spring Data JPA sẽ tự động tạo phần cài đặt tương ứng.
* Các phương thức CRUD như `save()`, `findById()`, `findAll()`, `delete()` được cung cấp sẵn.
* Điểm đặc biệt của Spring Data JPA là khả năng **tự động tạo truy vấn dựa trên tên phương thức**, ví dụ `findByEmail()` sẽ tự sinh câu truy vấn tìm kiếm theo email.
* Spring Data JPA hoạt động phía trên JPA, còn Hibernate là implementation cụ thể của JPA dùng để sinh SQL và làm việc với Database.
