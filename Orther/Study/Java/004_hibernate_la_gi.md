# Hibernate là gì?

## Khái niệm

**Hibernate** là một framework **ORM (Object-Relational Mapping)** mã nguồn mở dành cho Java.

ORM là kỹ thuật ánh xạ (mapping) giữa:

* **Bảng trong cơ sở dữ liệu (Table)** ↔ **Đối tượng Java (Object)**
* **Cột (Column)** ↔ **Thuộc tính (Field)**
* **Dòng dữ liệu (Row)** ↔ **Đối tượng (Instance)**

Nhờ đó, lập trình viên có thể thao tác với dữ liệu thông qua các đối tượng Java thay vì làm việc trực tiếp với các bảng trong cơ sở dữ liệu.

---

## Chức năng

Chức năng chính của Hibernate là:

> Tự động chuyển đổi dữ liệu giữa cơ sở dữ liệu và các đối tượng Java (Entity).

## Lợi ích

Hibernate giúp lập trình viên thao tác với dữ liệu hoàn toàn bằng code Java.

### Không cần viết SQL thủ công

Nhờ đó:

* Mã nguồn ngắn gọn hơn.
* Giảm số lượng câu lệnh SQL phải viết thủ công.
* Tập trung xử lý nghiệp vụ bằng Java thay vì thao tác trực tiếp với Database.

---

## Bản chất của Hibernate

Hibernate là một **implementation cụ thể của chuẩn JPA (Java Persistence API)**.

Quan hệ giữa chúng:

```text
JPA
 ↑
Hibernate
```

Trong đó:

### JPA

Là một chuẩn (Specification) định nghĩa cách thao tác với dữ liệu trong Java.

JPA chỉ đưa ra các interface và quy tắc, ví dụ:

```java
EntityManager
Query
PersistenceContext
```

JPA không tự thực hiện việc truy cập Database.

---

### Hibernate

Hibernate hiện thực hóa (implementation) các đặc tả của JPA.

Nói cách khác:

* JPA là chuẩn.
* Hibernate là framework thực thi chuẩn đó.

Ví dụ:

```java
@Autowired
private EntityManager entityManager;
```

Bên dưới, thực chất Hibernate sẽ đảm nhận việc:

* Sinh SQL.
* Kết nối Database.
* Thực hiện truy vấn.
* Chuyển đổi dữ liệu giữa bảng và Entity.

---

## Ví dụ tổng quát

Bảng trong Database:

```text
users
-------------------
id
name
email
```

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

Lưu dữ liệu:

```java
User user = new User();

user.setName("Nguyễn Văn A");
user.setEmail("a@gmail.com");

repository.save(user);
```

Hibernate sẽ tự sinh:

```sql
INSERT INTO users(name, email)
VALUES ('Nguyễn Văn A', 'a@gmail.com');
```

Khi truy vấn:

```java
User user = repository.findById(1L).orElse(null);
```

Hibernate sẽ tự sinh:

```sql
SELECT *
FROM users
WHERE id = 1;
```

---

# Tổng kết

* **Hibernate** là một framework ORM mã nguồn mở dành cho Java.
* Chức năng chính của Hibernate là **tự động chuyển đổi giữa bảng trong cơ sở dữ liệu và các đối tượng Java (Entity)**.
* Lập trình viên thao tác với dữ liệu thông qua code Java thay vì phải viết các câu lệnh SQL như `SELECT`, `INSERT`, `UPDATE`, `DELETE` một cách thủ công.
* Hibernate giúp mã nguồn ngắn gọn hơn và tập trung vào xử lý nghiệp vụ.
* **Hibernate là implementation của JPA**, tức là JPA là chuẩn còn Hibernate là framework cụ thể thực hiện chuẩn đó.
* Trong các ứng dụng Spring Boot hiện nay, Hibernate thường là ORM mặc định được sử dụng phía dưới JPA để làm việc với cơ sở dữ liệu.
