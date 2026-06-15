# Overloading là gì?

## 1. Khái niệm

**Overloading (Method Overloading)** là cơ chế **nạp chồng phương thức**, cho phép trong cùng một class tồn tại nhiều method có cùng tên nhưng khác danh sách tham số.

Việc khác nhau về danh sách tham số giúp Java phân biệt được cần gọi phương thức nào.

---

## 2. Điều kiện để Overloading

Các method phải có cùng tên nhưng khác ít nhất một trong các yếu tố sau:

### Khác số lượng tham số

Ví dụ:

* Một method nhận một tham số.
* Một method nhận hai tham số.

---

### Khác kiểu dữ liệu của tham số

Ví dụ:

* Một method nhận `int`.
* Một method nhận `double`.

---

### Khác thứ tự các tham số

Ví dụ:

* `(String, int)`
* `(int, String)`

---

## 3. Những yếu tố không tạo nên Overloading

### Chỉ khác kiểu trả về

Điều này không được coi là Overloading.

Ví dụ:

```java
int sum(int a, int b)

double sum(int a, int b)
```

Hai method trên sẽ gây lỗi biên dịch vì danh sách tham số hoàn toàn giống nhau.

---

## 4. Overloading được quyết định khi nào?

Overloading được quyết định tại **Compile Time**.

Vì vậy, Overloading còn được gọi là:

* Compile-time Polymorphism.
* Static Polymorphism.

Trình biên dịch sẽ dựa vào danh sách tham số để xác định method nào cần được gọi.

---

## 5. Mục đích của Overloading

### Tăng tính linh hoạt

Cho phép cùng một chức năng được sử dụng với nhiều kiểu dữ liệu khác nhau.

### Tăng khả năng đọc hiểu

Các method thực hiện cùng một công việc có thể dùng chung một tên.

### Giảm số lượng tên method

Không cần tạo nhiều tên khác nhau cho cùng một chức năng.

---

## 6. So sánh các trường hợp Overloading

| Trường hợp            | Có phải Overloading không? |
| --------------------- | -------------------------- |
| Khác số lượng tham số | Có                         |
| Khác kiểu tham số     | Có                         |
| Khác thứ tự tham số   | Có                         |
| Chỉ khác kiểu trả về  | Không                      |
| Khác tên method       | Không                      |

---

## Ví dụ

### Ví dụ 1: Khác số lượng tham số

```java
class Calculator {

    int sum(int a, int b) {
        return a + b;
    }

    int sum(int a, int b, int c) {
        return a + b + c;
    }
}
```

Sử dụng:

```java
Calculator cal = new Calculator();

System.out.println(cal.sum(10, 20));
System.out.println(cal.sum(10, 20, 30));
```

Kết quả:

```text
30
60
```

---

### Ví dụ 2: Khác kiểu tham số

```java
class Calculator {

    int sum(int a, int b) {
        return a + b;
    }

    double sum(double a, double b) {
        return a + b;
    }
}
```

Sử dụng:

```java
Calculator cal = new Calculator();

System.out.println(cal.sum(5, 3));
System.out.println(cal.sum(5.5, 3.2));
```

Kết quả:

```text
8
8.7
```

---

### Ví dụ 3: Khác thứ tự tham số

```java
class Printer {

    void print(String name, int age) {
        System.out.println(name + " " + age);
    }

    void print(int age, String name) {
        System.out.println(age + " " + name);
    }
}
```

---

### Ví dụ 4: Chỉ khác kiểu trả về (không hợp lệ)

```java
class Calculator {

    int sum(int a, int b) {
        return a + b;
    }

    // Lỗi biên dịch
    double sum(int a, int b) {
        return a + b;
    }
}
```

Vì danh sách tham số giống hệt nhau nên Java không thể phân biệt hai method này.

---

# Tổng kết

* Overloading là cơ chế nạp chồng method, cho phép nhiều method cùng tên tồn tại trong cùng một class.
* Các method phải khác danh sách tham số, có thể khác:

  * Số lượng tham số.
  * Kiểu dữ liệu của tham số.
  * Thứ tự tham số.
* Chỉ khác kiểu trả về không tạo thành Overloading.
* Overloading được quyết định tại thời điểm biên dịch (Compile Time), nên còn được gọi là **Compile-time Polymorphism** hoặc **Static Polymorphism**.
* Overloading giúp tăng tính linh hoạt, giảm số lượng tên method và làm cho mã nguồn dễ đọc hơn.
