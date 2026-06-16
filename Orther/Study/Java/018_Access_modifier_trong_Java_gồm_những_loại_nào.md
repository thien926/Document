# Access Modifier trong Java gồm những loại nào?

## Khái niệm

**Access Modifier (bộ phạm vi truy cập)** là các từ khóa dùng để quy định mức độ truy cập của class, thuộc tính (field), method hoặc constructor.

Java cung cấp bốn loại access modifier:

* `public`
* `private`
* `protected`
* `default` (không ghi từ khóa)

Việc lựa chọn access modifier phù hợp giúp chương trình:

* Đảm bảo tính đóng gói (Encapsulation).
* Hạn chế truy cập không cần thiết.
* Tăng tính an toàn.
* Giúp code dễ bảo trì và mở rộng.

---

# Các loại Access Modifier

## 1. public

### Khái niệm

`public` cho phép truy cập từ mọi nơi.

Các class, method hoặc biến được khai báo `public` có thể được sử dụng:

* Trong cùng class.
* Trong cùng package.
* Ở package khác.
* Bởi bất kỳ class nào.

### Phạm vi truy cập

* Cùng class: ✔
* Cùng package: ✔
* Khác package: ✔
* Class con ở package khác: ✔

---

## 2. private

### Khái niệm

`private` chỉ cho phép truy cập bên trong chính class đó.

Các class khác, kể cả class con, không thể truy cập trực tiếp thành phần được khai báo `private`.

### Phạm vi truy cập

* Cùng class: ✔
* Cùng package: ✘
* Khác package: ✘
* Class con ở package khác: ✘

### Đặc điểm

`private` thường được sử dụng để:

* Che giấu dữ liệu bên trong class.
* Bảo vệ trạng thái của đối tượng.
* Thực hiện nguyên lý Encapsulation.

---

## 3. protected

### Khái niệm

`protected` cho phép truy cập:

* Trong cùng package.
* Từ các class con (subclass), kể cả khi subclass nằm ở package khác.

### Phạm vi truy cập

* Cùng class: ✔
* Cùng package: ✔
* Khác package: ✘
* Class con ở package khác: ✔

### Đặc điểm

`protected` thường được sử dụng khi:

* Muốn cho phép class con kế thừa và sử dụng các thành phần của class cha.
* Hạn chế truy cập từ các class không liên quan.

---

## 4. default (Package-private)

### Khái niệm

Khi không khai báo access modifier nào, Java sử dụng phạm vi truy cập mặc định (default hay package-private).

Các thành phần này chỉ được truy cập trong cùng package.

### Phạm vi truy cập

* Cùng class: ✔
* Cùng package: ✔
* Khác package: ✘
* Class con ở package khác: ✘

### Đặc điểm

`default` thích hợp khi:

* Các class trong cùng package cần chia sẻ dữ liệu hoặc chức năng với nhau.
* Không muốn cho phép truy cập từ package khác.

---

# Bảng so sánh

| Access Modifier | Cùng class | Cùng package | Khác package | Subclass khác package |
| --------------- | ---------- | ------------ | ------------ | --------------------- |
| public          | ✔          | ✔            | ✔            | ✔                     |
| protected       | ✔          | ✔            | ✘            | ✔                     |
| default         | ✔          | ✔            | ✘            | ✘                     |
| private         | ✔          | ✘            | ✘            | ✘                     |

---

# Ví dụ

```java
class Person {

    public String name = "John";

    private int age = 25;

    protected String address = "Ha Noi";

    String phone = "0123456789"; // default
}
```

### Truy cập trong cùng class

```java
class Person {

    public String name;
    private int age;
    protected String address;
    String phone;

    void show() {
        System.out.println(name);
        System.out.println(age);
        System.out.println(address);
        System.out.println(phone);
    }
}
```

Tất cả đều truy cập được vì đang ở trong chính class đó.

---

### Ví dụ về `private`

```java
class Person {
    private int age = 25;
}

class Test {
    public static void main(String[] args) {

        Person p = new Person();

        // p.age;  // Lỗi biên dịch
    }
}
```

Do `age` là `private`, chỉ class `Person` mới được truy cập trực tiếp.

---

### Ví dụ về `protected`

```java
class Animal {
    protected void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {

    void bark() {
        sound(); // truy cập được
    }
}
```

`Dog` là class con nên có thể sử dụng method `sound()`.

---

# Tổng kết

* Access Modifier dùng để kiểm soát phạm vi truy cập của class, field, method và constructor.
* Java có bốn loại access modifier: `public`, `private`, `protected` và `default`.
* `public` cho phép truy cập từ mọi nơi.
* `private` chỉ cho phép truy cập trong chính class.
* `protected` cho phép truy cập trong cùng package và từ subclass ở package khác.
* `default` chỉ cho phép truy cập trong cùng package.
* Chọn access modifier phù hợp giúp tăng tính đóng gói, an toàn và khả năng bảo trì của chương trình.
