# final, finally, finalize() khác nhau ra sao?

## 1. `final`

`final` là một từ khóa (keyword) trong Java dùng để **ngăn việc thay đổi hoặc kế thừa**.

### Chức năng

Tùy vào vị trí sử dụng, `final` có ý nghĩa khác nhau:

#### Đối với biến (`final variable`)

* Chỉ được gán giá trị một lần.
* Không thể gán lại giá trị sau khi đã khởi tạo.

#### Đối với phương thức (`final method`)

* Không thể bị ghi đè (override) ở lớp con.

#### Đối với lớp (`final class`)

* Không thể được kế thừa bởi lớp khác.

### Mục đích

* Bảo vệ dữ liệu không bị thay đổi ngoài ý muốn.
* Hạn chế việc mở rộng hoặc thay đổi hành vi của lớp và phương thức.
* Tăng tính ổn định và an toàn của chương trình.

---

## 2. `finally`

`finally` là một block trong cơ chế xử lý ngoại lệ (Exception Handling).

### Chức năng

* Chứa các đoạn mã cần được thực thi sau khối `try` hoặc `catch`.
* Thường được dùng để giải phóng tài nguyên.

### Đặc điểm

* Được thực thi sau `try` hoặc `catch`.
* Vẫn được thực hiện dù có xảy ra exception hay không.
* Thường dùng để:

  * Đóng file.
  * Đóng kết nối cơ sở dữ liệu.
  * Giải phóng tài nguyên hệ thống.

### Mục đích

Đảm bảo các thao tác dọn dẹp tài nguyên luôn được thực hiện, tránh gây rò rỉ tài nguyên.

---

## 3. `finalize()`

`finalize()` là một phương thức của lớp `Object`.

### Chức năng

* Được gọi trước khi object bị Garbage Collector (GC) thu hồi.

### Đặc điểm

* Không đảm bảo chính xác thời điểm được gọi.
* Có thể không được gọi trước khi chương trình kết thúc.
* Phụ thuộc vào cơ chế Garbage Collector.

### Trạng thái hiện nay

* `finalize()` đã lỗi thời (deprecated).
* Không còn được khuyến nghị sử dụng trong các phiên bản Java hiện đại.

### Lý do không nên sử dụng

* Không đảm bảo thời điểm thực thi.
* Có thể gây ảnh hưởng đến hiệu năng.
* Làm cho việc quản lý bộ nhớ trở nên khó dự đoán.

---

## 4. So sánh `final`, `finally`, `finalize()`

| Tiêu chí                     | final                      | finally                          | finalize()                   |
| ---------------------------- | -------------------------- | -------------------------------- | ---------------------------- |
| Loại                         | Keyword                    | Block                            | Method                       |
| Mục đích                     | Ngăn thay đổi hoặc kế thừa | Dọn dẹp tài nguyên sau try/catch | Xử lý trước khi object bị GC |
| Vị trí sử dụng               | Biến, method, class        | Trong xử lý exception            | Trong lớp Object             |
| Có luôn được thực thi không? | Không áp dụng              | Thông thường có                  | Không đảm bảo                |
| Khuyến nghị sử dụng          | Có                         | Có                               | Không                        |
| Trạng thái hiện nay          | Được sử dụng rộng rãi      | Được sử dụng rộng rãi            | Đã lỗi thời (deprecated)     |

---

## Ví dụ

### Ví dụ về `final`

```java
final int MAX_SIZE = 100;

// MAX_SIZE = 200; // Lỗi biên dịch
```

---

#### Method `final`

```java
class Animal {
    final void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {

    // Lỗi biên dịch
    // void sound() { }
}
```

---

#### Class `final`

```java
final class Utility {

}

// Lỗi biên dịch
// class MyUtility extends Utility { }
```

---

### Ví dụ về `finally`

```java
try {
    int result = 10 / 0;
} catch (Exception e) {
    System.out.println("Có lỗi xảy ra");
} finally {
    System.out.println("Khối finally luôn được thực thi");
}
```

Kết quả:

```text
Có lỗi xảy ra
Khối finally luôn được thực thi
```

---

### Ví dụ về `finalize()`

```java
class Student {

    @Override
    protected void finalize() throws Throwable {
        System.out.println("Object sắp bị GC thu hồi");
    }
}
```

Phương thức này có thể được gọi trước khi đối tượng bị Garbage Collector thu hồi, tuy nhiên thời điểm gọi không được đảm bảo và hiện nay không còn được khuyến nghị sử dụng.

---

# Tổng kết

* `final` là keyword dùng để ngăn thay đổi:

  * Biến `final` không thể gán lại.
  * Method `final` không thể bị override.
  * Class `final` không thể bị kế thừa.

* `finally` là block trong xử lý exception, thường được sử dụng để giải phóng tài nguyên và được thực thi sau khối `try/catch`.

* `finalize()` là phương thức của lớp `Object`, được gọi trước khi object bị Garbage Collector thu hồi, nhưng không đảm bảo thời điểm thực thi và đã lỗi thời trong Java hiện đại.

* `final`, `finally` và `finalize()` hoàn toàn khác nhau về bản chất và mục đích sử dụng. Trong đó, `final` và `finally` vẫn được sử dụng phổ biến, còn `finalize()` không còn được khuyến nghị sử dụng.
