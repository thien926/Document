# Overriding là gì?

## Khái niệm

**Overriding (ghi đè phương thức)** là cơ chế mà **class con định nghĩa lại method của class cha** với:

* Cùng tên method.
* Cùng danh sách tham số.
* Kiểu trả về giống hoặc tương thích với method của class cha.

Khi đó, method của class con sẽ thay thế cách triển khai của method trong class cha.

---

## Đặc điểm của Overriding

### 1. Method của class con ghi đè method của class cha

Class con có thể cung cấp cách xử lý riêng phù hợp với nhu cầu của nó, mặc dù vẫn giữ nguyên tên và tham số của method.

---

### 2. Phải có cùng chữ ký (Method Signature)

Để một method được coi là overriding, method trong class con phải có:

* Cùng tên.
* Cùng số lượng tham số.
* Cùng kiểu dữ liệu của các tham số.

Nếu khác danh sách tham số thì đó không phải là overriding mà là overloading.

---

### 3. Kiểu trả về phải tương thích

Method trong class con có thể:

* Trả về cùng kiểu với method của class cha.
* Trả về kiểu là class con của kiểu trả về trong class cha (covariant return type).

---

### 4. Là cơ chế của Runtime Polymorphism

Overriding là nền tảng của **đa hình lúc chạy (Runtime Polymorphism)**.

Việc gọi method nào không được quyết định khi biên dịch mà được quyết định khi chương trình đang chạy.

---

### 5. JVM quyết định method thực tế dựa trên object thật

Một biến tham chiếu kiểu class cha có thể trỏ tới object của class con.

Khi gọi method thông qua biến tham chiếu đó, JVM sẽ kiểm tra object thực sự đang được tạo thuộc class nào để quyết định method nào sẽ được thực thi.

Do đó:

* Reference cha không quyết định method được gọi.
* Object thực tế mới quyết định method được thực thi.

---

# Ví dụ

```java
class Animal {
    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}

public class Main {
    public static void main(String[] args) {

        Animal animal = new Dog();

        animal.sound();
    }
}
```

### Kết quả

```text
Dog barks
```

### Giải thích

* `sound()` được khai báo trong `Animal`.
* `Dog` định nghĩa lại `sound()`.
* Biến `animal` có kiểu `Animal`, nhưng object thực tế là `Dog`.
* Khi gọi `animal.sound()`, JVM sẽ thực hiện method `sound()` của `Dog`.

Đây chính là **Runtime Polymorphism**.

---

# Tổng kết

* Overriding là việc class con định nghĩa lại method của class cha.
* Method ghi đè phải có cùng tên và cùng danh sách tham số với method gốc.
* Kiểu trả về phải giống hoặc tương thích với method của class cha.
* Overriding là cơ chế tạo nên **Runtime Polymorphism**.
* Khi gọi method thông qua reference cha, JVM sẽ quyết định method được thực thi dựa trên object thực tế.
* Object thực tế quyết định method được gọi, không phải kiểu của biến tham chiếu.
