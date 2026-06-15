# So sánh Java 8, Java 17 và Java 21

## Java 8 (2014)

### 1. Lambda Expression

Cho phép viết hàm ngắn gọn, giảm Anonymous Class.

**Trước Java 8**

```java
new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello");
    }
}).start();
```

**Java 8**

```java
new Thread(() -> System.out.println("Hello"))
        .start();
```

---

### 2. Stream API

Hỗ trợ xử lý dữ liệu theo phong cách Functional Programming.

**Ví dụ**

```java
List<String> result = users.stream()
        .filter(u -> u.getAge() > 18)
        .map(User::getName)
        .toList();
```

Lợi ích:

* Code ngắn gọn hơn
* Dễ đọc
* Hỗ trợ xử lý dữ liệu dạng pipeline

---

### 3. Optional

Giảm nguy cơ NullPointerException.

**Ví dụ**

```java
Optional<User> user = findUser();

user.ifPresent(System.out::println);
```

Thay vì:

```java
if(user != null){
    System.out.println(user);
}
```

---

### 4. Date Time API

Thay thế các API cũ như Date và Calendar.

**Ví dụ**

```java
LocalDate date = LocalDate.now();

LocalDateTime dateTime =
        LocalDateTime.now();

LocalTime time = LocalTime.now();
```

Ưu điểm:

* Dễ sử dụng
* Immutable
* Thread-safe

---

## Java 17 (LTS)

### 1. var

Cho phép compiler tự suy luận kiểu dữ liệu.

**Java 8**

```java
String name = "Thien";
```

**Java 17**

```java
var name = "Thien";
```

Compiler vẫn hiểu:

```java
String name = "Thien";
```

---

### 2. Text Blocks

Viết chuỗi nhiều dòng dễ đọc hơn.

**Java 8**

```java
String sql =
        "SELECT * \n" +
        "FROM users \n" +
        "WHERE age > 18";
```

**Java 17**

```java
String sql = """
        SELECT *
        FROM users
        WHERE age > 18
        """;
```

---

### 3. Record

Giảm boilerplate code cho DTO hoặc Model chỉ chứa dữ liệu.

**Java 8**

```java
public class User {

    private Long id;
    private String name;

    // constructor
    // getter
    // setter
    // equals
    // hashCode
    // toString
}
```

**Java 17**

```java
public record User(
        Long id,
        String name
) {}
```

Record tự sinh:

* Constructor
* Getter
* equals()
* hashCode()
* toString()

---

### 4. Sealed Class

Giới hạn class nào được phép kế thừa.

```java
public sealed class Animal
        permits Dog, Cat {
}
```

Chỉ:

```java
Dog
Cat
```

được phép kế thừa Animal.

---

### 5. Pattern Matching for instanceof

Giảm việc ép kiểu thủ công.

**Java 8**

```java
if(obj instanceof User){
    User user = (User) obj;

    System.out.println(user.getName());
}
```

**Java 17**

```java
if(obj instanceof User user){
    System.out.println(user.getName());
}
```

---

## Java 21 (LTS)

### 1. Virtual Threads

Virtual Thread là loại thread nhẹ được JVM quản lý.

**Platform Thread**

```text
1 Java Thread
↕
1 OS Thread
```

**Virtual Thread**

```text
N Virtual Threads
↕
Một số lượng nhỏ OS Threads
```

Ưu điểm:

* Tạo thread rất rẻ
* Hỗ trợ hàng chục nghìn hoặc hàng triệu thread
* Hiệu quả với I/O (Database, HTTP Call, File I/O)

**Ví dụ**

```java
Thread.startVirtualThread(() -> {
    System.out.println("Hello");
});
```

Hoặc:

```java
try(var executor =
        Executors.newVirtualThreadPerTaskExecutor()) {

}
```

---

### 2. Pattern Matching for switch

**Java 8**

```java
if(obj instanceof String){

}
else if(obj instanceof Integer){

}
```

**Java 21**

```java
switch(obj){

    case String s ->
            System.out.println(s);

    case Integer i ->
            System.out.println(i);

    default ->
            System.out.println("Unknown");
}
```

Code ngắn gọn và dễ đọc hơn.

---

### 3. Record Pattern

Cho phép destructuring dữ liệu từ Record.

**Java 17**

```java
User user =
        new User(1L, "Thien");

Long id = user.id();
String name = user.name();
```

**Java 21**

```java
if(user instanceof User(
        Long id,
        String name)) {

    System.out.println(name);
}
```

Không cần gọi getter thủ công.

---

### 4. Sequenced Collections

Bổ sung API truy cập phần tử đầu và cuối.

**Java 21**

```java
list.getFirst();

list.getLast();
```

Thay vì:

```java
list.get(0);

list.get(list.size() - 1);
```

Code rõ ràng và dễ hiểu hơn.

---

# Tóm tắt nhanh khi phỏng vấn

## Java 8

* Lambda Expression
* Stream API
* Optional
* Date Time API

## Java 17

* var
* Text Blocks
* Record
* Sealed Class
* Pattern Matching for instanceof

## Java 21

* Virtual Threads
* Pattern Matching for switch
* Record Pattern
* Sequenced Collections

Điểm nổi bật nhất:

* Java 8: Functional Programming
* Java 17: Giảm Boilerplate Code
* Java 21: Virtual Threads giúp xử lý đồng thời hiệu quả hơn cho các ứng dụng I/O-bound
