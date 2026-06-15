# Các loại Exception và cách xử lý Exception

## 1. Các loại Exception phổ biến

Trong Java, các ngoại lệ (Exception) được chia thành nhiều loại khác nhau nhằm giúp lập trình viên xác định nguyên nhân lỗi và có cách xử lý phù hợp. Ba nhóm phổ biến nhất gồm:

* Checked Exception
* Unchecked Exception (Runtime Exception)
* Error

---

## 1.1. Checked Exception (Ngoại lệ được kiểm tra)

### Khái niệm

Checked Exception là những ngoại lệ mà trình biên dịch (Compiler) bắt buộc lập trình viên phải xử lý ngay tại thời điểm biên dịch (Compile-time).

Nếu không xử lý bằng `try-catch` hoặc không khai báo bằng `throws`, chương trình sẽ không biên dịch được.

### Đặc điểm

* Được kiểm tra trong quá trình biên dịch.
* Bắt buộc phải xử lý.
* Thường liên quan đến các thao tác bên ngoài chương trình như:

  * Đọc/ghi file.
  * Kết nối cơ sở dữ liệu.
  * Truy cập mạng.

### Một số Checked Exception phổ biến

| Exception              | Ý nghĩa                            |
| ---------------------- | ---------------------------------- |
| IOException            | Lỗi đọc hoặc ghi dữ liệu           |
| FileNotFoundException  | Không tìm thấy file                |
| SQLException           | Lỗi khi thao tác với cơ sở dữ liệu |
| ClassNotFoundException | Không tìm thấy lớp được yêu cầu    |

---

## 1.2. Unchecked Exception (Runtime Exception)

### Khái niệm

Unchecked Exception là các ngoại lệ xảy ra trong lúc chương trình đang chạy (Runtime).

Trình biên dịch không bắt buộc lập trình viên phải xử lý, tuy nhiên vẫn nên kiểm tra và xử lý để tránh chương trình bị dừng đột ngột.

### Đặc điểm

* Xảy ra khi chương trình đang thực thi.
* Không bắt buộc phải dùng `try-catch`.
* Chủ yếu xuất phát từ lỗi logic hoặc dữ liệu không hợp lệ.

### Một số Unchecked Exception phổ biến

| Exception                 | Ý nghĩa                                        |
| ------------------------- | ---------------------------------------------- |
| NullPointerException      | Truy cập đối tượng có giá trị `null`           |
| IndexOutOfBoundsException | Truy cập vượt quá giới hạn mảng hoặc danh sách |
| ArithmeticException       | Lỗi tính toán, ví dụ chia cho 0                |
| NumberFormatException     | Chuyển đổi chuỗi sang số không hợp lệ          |

---

## 1.3. Error (Lỗi hệ thống)

### Khái niệm

Error là những lỗi nghiêm trọng xảy ra trong JVM hoặc hệ thống, thông thường chương trình không thể tự xử lý được bằng code.

### Đặc điểm

* Không nên bắt bằng `try-catch`.
* Thường liên quan đến tài nguyên hoặc môi trường thực thi.
* Có thể khiến chương trình dừng hoàn toàn.

### Một số Error phổ biến

| Error               | Ý nghĩa                             |
| ------------------- | ----------------------------------- |
| OutOfMemoryError    | JVM hết bộ nhớ                      |
| StackOverflowError  | Tràn ngăn xếp do gọi đệ quy quá sâu |
| VirtualMachineError | JVM gặp lỗi nghiêm trọng            |

---

# 2. Cách xử lý Exception hiệu quả

## 2.1. Khối try-catch (Bắt và xử lý lỗi)

### Mục đích

Khối `try-catch` dùng để phát hiện và xử lý ngoại lệ, giúp chương trình tiếp tục hoạt động thay vì bị dừng đột ngột.

### Cấu trúc

```java
try {
    // Đoạn mã có thể phát sinh lỗi
}
catch (Exception e) {
    // Xử lý lỗi
}
```

### Hoạt động

1. Mã trong khối `try` được thực thi.
2. Nếu xảy ra Exception:

   * Chương trình dừng phần còn lại của khối `try`.
   * Chuyển sang khối `catch`.
3. Sau khi xử lý xong, chương trình tiếp tục chạy.

---

## 2.2. Khối finally (Dọn dẹp tài nguyên)

### Mục đích

Khối `finally` dùng để giải phóng tài nguyên sau khi thực hiện xong, bất kể có xảy ra Exception hay không.

### Đặc điểm

* Luôn được thực thi.
* Thường dùng để:

  * Đóng file.
  * Đóng kết nối cơ sở dữ liệu.
  * Giải phóng tài nguyên.

### Cấu trúc

```java
try {
    // Code
}
catch (Exception e) {
    // Xử lý lỗi
}
finally {
    // Dọn dẹp tài nguyên
}
```

---

## 2.3. Từ khóa throw (Tự ném lỗi)

### Mục đích

`throw` được sử dụng khi lập trình viên muốn chủ động tạo và phát sinh một Exception.

### Đặc điểm

* Tạo ra Exception theo điều kiện mong muốn.
* Giúp kiểm tra tính hợp lệ của dữ liệu.
* Có thể ném cả Checked Exception và Runtime Exception.

### Cú pháp

```java
throw new ExceptionType("Thông báo lỗi");
```

---

## 2.4. Từ khóa throws (Khai báo ném lỗi)

### Mục đích

`throws` được sử dụng để khai báo rằng một phương thức có khả năng phát sinh Exception và chuyển trách nhiệm xử lý cho nơi gọi phương thức đó.

### Đặc điểm

* Không xử lý lỗi trực tiếp.
* Chỉ khai báo khả năng phát sinh lỗi.
* Thường dùng với Checked Exception.

### Cú pháp

```java
public void methodName() throws ExceptionType {
    // Code
}
```

---

# Ví dụ

## Ví dụ 1: Sử dụng try-catch

```java
try {
    int result = 10 / 0;
}
catch (ArithmeticException e) {
    System.out.println("Không thể chia cho 0");
}
```

Kết quả:

```text
Không thể chia cho 0
```

---

## Ví dụ 2: Sử dụng finally

```java
try {
    System.out.println("Thực hiện công việc");
}
catch (Exception e) {
    System.out.println("Có lỗi xảy ra");
}
finally {
    System.out.println("Đóng tài nguyên");
}
```

Kết quả:

```text
Thực hiện công việc
Đóng tài nguyên
```

Khối `finally` vẫn được thực thi dù có lỗi hay không.

---

## Ví dụ 3: Sử dụng throw

```java
int age = -1;

if (age < 0) {
    throw new IllegalArgumentException("Tuổi không hợp lệ");
}
```

Kết quả:

```text
Exception in thread "main"
java.lang.IllegalArgumentException: Tuổi không hợp lệ
```

---

## Ví dụ 4: Sử dụng throws

```java
import java.io.IOException;

public void readFile() throws IOException {
    // Đọc file
}
```

Phương thức `readFile()` có thể phát sinh `IOException`, việc xử lý sẽ được chuyển cho nơi gọi phương thức.

---

## Ví dụ 5: Checked Exception

```java
import java.io.FileReader;
import java.io.IOException;

try {
    FileReader file = new FileReader("data.txt");
}
catch (IOException e) {
    System.out.println("Không thể mở file");
}
```

`IOException` là Checked Exception nên bắt buộc phải được xử lý.

---

## Ví dụ 6: Unchecked Exception

```java
String name = null;

System.out.println(name.length());
```

Kết quả:

```text
Exception in thread "main"
java.lang.NullPointerException
```

`NullPointerException` là Runtime Exception, trình biên dịch không bắt buộc phải xử lý.

---

## Ví dụ 7: Error

```java
public static void recursion() {
    recursion();
}
```

Gọi:

```java
recursion();
```

Có thể dẫn đến:

```text
java.lang.StackOverflowError
```

Đây là lỗi nghiêm trọng của hệ thống và thường không xử lý bằng `try-catch`.

---

# Tổng kết

* **Checked Exception** là các ngoại lệ được kiểm tra ở thời điểm biên dịch và bắt buộc phải xử lý, ví dụ: `IOException`, `SQLException`.
* **Unchecked Exception (Runtime Exception)** xảy ra trong lúc chương trình chạy, không bắt buộc xử lý nhưng nên kiểm tra để tránh lỗi, ví dụ: `NullPointerException`, `IndexOutOfBoundsException`, `ArithmeticException`.
* **Error** là các lỗi nghiêm trọng của hệ thống hoặc JVM như `OutOfMemoryError`, `StackOverflowError`, thường không thể xử lý bằng code thông thường.
* **try-catch** dùng để bắt và xử lý ngoại lệ.
* **finally** dùng để dọn dẹp tài nguyên và luôn được thực thi.
* **throw** dùng để chủ động phát sinh một Exception.
* **throws** dùng để khai báo rằng phương thức có thể phát sinh Exception và chuyển việc xử lý cho nơi gọi phương thức.
