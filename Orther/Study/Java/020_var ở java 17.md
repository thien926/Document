# `var` trong Java 17

## 1. `var` là gì?

`var` là cú pháp cho phép Java tự suy luận kiểu dữ liệu của biến dựa trên giá trị khởi tạo.

Mục tiêu của `var` là giảm bớt việc phải khai báo kiểu dữ liệu dài dòng, giúp mã nguồn ngắn gọn và dễ đọc hơn.

Kiểu dữ liệu thực tế của biến vẫn được xác định tại thời điểm biên dịch (compile time).

---

# 2. Lý do Java bổ sung `var`

## 2.1. Giảm bớt Boilerplate Code

Trong Java truyền thống, kiểu dữ liệu thường phải xuất hiện ở cả bên trái và bên phải của phép gán, gây ra sự lặp lại không cần thiết.

`var` được đưa vào để loại bỏ phần khai báo kiểu dữ liệu dư thừa này, giúp mã nguồn ngắn gọn hơn.

### Lợi ích

* Giảm số lượng ký tự phải viết.
* Tăng khả năng đọc code.
* Hạn chế sự lặp lại không cần thiết.

---

## 2.2. Tăng sự tập trung vào tên biến

Khi đọc code, điều quan trọng nhất là hiểu biến đó được sử dụng để làm gì.

Tên biến thường mang nhiều ý nghĩa hơn việc nhìn thấy kiểu dữ liệu quá chi tiết.

Sử dụng `var` giúp người đọc tập trung vào:

* Tên biến.
* Giá trị khởi tạo.
* Ý nghĩa của biến trong chương trình.

Thay vì bị phân tán bởi những kiểu dữ liệu dài.

### Lợi ích

* Code dễ đọc hơn.
* Logic chương trình được thể hiện rõ ràng hơn.
* Tăng tính trực quan khi bảo trì mã nguồn.

---

## 2.3. Hỗ trợ các kiểu dữ liệu phức tạp

Trong thực tế, nhiều cấu trúc dữ liệu có kiểu khai báo rất dài, đặc biệt khi sử dụng:

* Generic.
* Collection lồng nhau.
* Stream API.

Nếu khai báo đầy đủ kiểu dữ liệu, mã nguồn có thể trở nên khó đọc.

`var` giúp phần khai báo ngắn gọn hơn nhưng vẫn giữ nguyên kiểu dữ liệu thực tế mà compiler suy luận được.

### Lợi ích

* Làm code gọn hơn.
* Giảm sự rối mắt khi làm việc với Generic phức tạp.
* Dễ bảo trì hơn.

---

# 3. Những điều cần lưu ý về `var`

## 3.1. Java không trở thành ngôn ngữ Dynamic Type

`var` chỉ là cú pháp hỗ trợ compiler suy luận kiểu dữ liệu.

Sau khi biên dịch:

* `var` sẽ biến mất.
* Compiler thay thế nó bằng kiểu dữ liệu thực sự.

Do đó, Java vẫn là ngôn ngữ kiểu tĩnh (Static Typing).

Một biến được suy luận là kiểu nào thì chỉ có thể chứa dữ liệu thuộc kiểu đó.

### Ví dụ

Nếu compiler xác định biến là `String` thì biến đó không thể gán lại thành `int`.

Điều này khác với các ngôn ngữ Dynamic Type như:

* JavaScript.
* Python.

---

## 3.2. Chỉ sử dụng được với Local Variable

`var` chỉ được dùng cho:

* Biến cục bộ trong phương thức (Local Variable).
* Biến trong vòng lặp `for`.
* Biến trong khối lệnh.

Không thể dùng `var` để khai báo:

* Thuộc tính của class (Instance Variable).
* Biến static.
* Kiểu trả về của phương thức.
* Kiểu tham số của phương thức.

---

# Tổng kết

* `var` cho phép compiler tự suy luận kiểu dữ liệu của biến.
* Mục tiêu chính của `var` là giảm Boilerplate Code và làm mã nguồn ngắn gọn hơn.
* `var` giúp người đọc tập trung vào tên biến và giá trị khởi tạo thay vì kiểu dữ liệu dài dòng.
* `var` đặc biệt hữu ích khi làm việc với Generic hoặc các cấu trúc dữ liệu phức tạp.
* Java vẫn là ngôn ngữ kiểu tĩnh, `var` không biến Java thành ngôn ngữ Dynamic Type.
* Sau khi biên dịch, `var` sẽ được thay thế bằng kiểu dữ liệu thực sự.
* `var` chỉ sử dụng được cho biến cục bộ và không thể dùng để khai báo thuộc tính của class.

# Ví dụ

### Giảm Boilerplate Code

```java
// Cách cũ
BufferedReader br =
        new BufferedReader(new FileReader("file.txt"));

// Sử dụng var
var br =
        new BufferedReader(new FileReader("file.txt"));
```

---

### Tập trung vào tên biến

```java
// Cách cũ
DateTimeFormatter formatter =
        DateTimeFormatter.ofPattern("yyyy-MM-dd");

// Sử dụng var
var formatter =
        DateTimeFormatter.ofPattern("yyyy-MM-dd");
```

---

### Với Generic phức tạp

```java
// Cách cũ
Map<String, List<Map<Integer, String>>> complexMap =
        new HashMap<>();

// Sử dụng var
var complexMap =
        new HashMap<String, List<Map<Integer, String>>>();
```

---

### Java vẫn là Static Typing

```java
var name = "Thiện"; // Compiler suy luận là String

name = "Nguyễn";   // Hợp lệ

name = 100;        // Lỗi biên dịch
```

---

### Sử dụng trong vòng lặp `for`

```java
for (var i = 0; i < 5; i++) {
    System.out.println(i);
}
```

---

### Không thể dùng làm thuộc tính của class

```java
public class User {

    // Lỗi biên dịch
    var name = "John";

}
```

`var` chỉ được phép sử dụng bên trong phương thức hoặc các khối lệnh cục bộ.
