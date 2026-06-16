# Java có hỗ trợ đa kế thừa không?

## 1. Khái niệm

Đa kế thừa (Multiple Inheritance) là khả năng một lớp có thể kế thừa từ nhiều lớp cha khác nhau.

Ví dụ:

```text
Class C kế thừa từ Class A và Class B
```

---

## 2. Java có hỗ trợ đa kế thừa class không?

Java **không hỗ trợ đa kế thừa class**.

Điều này có nghĩa là:

* Một class chỉ có thể `extends` duy nhất một class cha.
* Không thể kế thừa trực tiếp từ nhiều class cùng lúc.

Ví dụ dưới đây không được phép:

```java
class A {

}

class B {

}

// Không hợp lệ
class C extends A, B {

}
```

---

## 3. Lý do Java không hỗ trợ đa kế thừa class

### Tránh xung đột và sự mơ hồ

Nếu hai class cha cùng chứa một phương thức giống nhau, class con sẽ không biết phải sử dụng phương thức của class nào.

Điều này dẫn đến vấn đề được gọi là **Diamond Problem**.

### Diamond Problem

Giả sử:

* Class `B` kế thừa từ `A`.
* Class `C` kế thừa từ `A`.
* Class `D` đồng thời kế thừa từ `B` và `C`.

```text
        A
       / \
      B   C
       \ /
        D
```

Nếu `B` và `C` đều ghi đè cùng một phương thức của `A`, thì khi `D` gọi phương thức đó sẽ xảy ra sự mơ hồ:

* Gọi phương thức của `B`?
* Hay gọi phương thức của `C`?

Để tránh vấn đề này, Java không cho phép đa kế thừa giữa các class.

---

## 4. Java hỗ trợ đa kế thừa về hành vi thông qua Interface

Mặc dù không hỗ trợ đa kế thừa class, Java vẫn cho phép một class triển khai nhiều interface.

### Đặc điểm

* Một class có thể `implements` nhiều interface.
* Nhờ đó có thể kế thừa nhiều hành vi khác nhau.
* Không gây ra sự mơ hồ như đa kế thừa class.

### Cú pháp

```java
class MyClass implements InterfaceA, InterfaceB {

}
```

---

## 5. So sánh kế thừa class và interface

| Tiêu chí                                           | Class                         | Interface       |
| -------------------------------------------------- | ----------------------------- | --------------- |
| Một class có thể kế thừa nhiều thành phần cùng lúc | Không                         | Có              |
| Từ khóa sử dụng                                    | `extends`                     | `implements`    |
| Số lượng được phép                                 | Một class cha                 | Nhiều interface |
| Có nguy cơ xảy ra Diamond Problem                  | Có                            | Được Java xử lý |
| Mục đích                                           | Kế thừa thuộc tính và hành vi | Kế thừa hành vi |

---

## Ví dụ

### Ví dụ về kế thừa class

```java
class Animal {

}

class Dog extends Animal {

}
```

`Dog` chỉ được phép kế thừa một class là `Animal`.

---

### Ví dụ đa kế thừa class (không hợp lệ)

```java
class A {

}

class B {

}

// Lỗi biên dịch
class C extends A, B {

}
```

---

### Ví dụ implements nhiều interface

```java
interface Flyable {

    void fly();
}

interface Swimmable {

    void swim();
}

class Duck implements Flyable, Swimmable {

    @Override
    public void fly() {
        System.out.println("Flying...");
    }

    @Override
    public void swim() {
        System.out.println("Swimming...");
    }
}
```

Sử dụng:

```java
Duck duck = new Duck();

duck.fly();
duck.swim();
```

Kết quả:

```text
Flying...
Swimming...
```

Class `Duck` có thể triển khai đồng thời nhiều interface, từ đó sở hữu nhiều hành vi khác nhau.

---

# Tổng kết

* Java không hỗ trợ đa kế thừa giữa các class, nghĩa là một class chỉ được `extends` duy nhất một class cha.
* Mục đích chính là tránh sự mơ hồ và xung đột, đặc biệt là vấn đề **Diamond Problem**.
* Java vẫn hỗ trợ đa kế thừa về hành vi thông qua interface.
* Một class có thể `implements` nhiều interface cùng lúc.
* Nhờ cơ chế này, Java vừa tránh được các vấn đề của đa kế thừa class, vừa vẫn cung cấp khả năng mở rộng và tái sử dụng hành vi một cách linh hoạt.
