# String là gì? Vì sao String immutable?

## 1. String là gì?

### Khái niệm

`String` là một class trong Java dùng để biểu diễn chuỗi ký tự.

Chuỗi có thể chứa:

* Chữ cái.
* Chữ số.
* Ký tự đặc biệt.
* Khoảng trắng.

Ví dụ:

```java
String name = "Java";
String message = "Hello World";
String number = "123";
```

---

## 2. String immutable là gì?

### Khái niệm

`String immutable` nghĩa là sau khi một đối tượng String được tạo ra thì nội dung của nó không thể thay đổi.

Nếu thực hiện các thao tác như:

* Nối chuỗi.
* Thay thế ký tự.
* Chuyển sang chữ hoa, chữ thường.
* Cắt chuỗi.

Java sẽ không sửa đối tượng ban đầu mà tạo ra một đối tượng String mới.

---

## 3. Cách String hoạt động khi thay đổi nội dung

Giả sử:

```java
String str = "Java";
```

Lúc này tạo ra một object String chứa giá trị:

```text
"Java"
```

Khi thực hiện:

```java
str = str + " Core";
```

Java không thay đổi object `"Java"` ban đầu.

Thay vào đó:

1. Tạo object mới `"Java Core"`.
2. Biến `str` sẽ tham chiếu đến object mới này.

Object `"Java"` cũ vẫn tồn tại cho đến khi được Garbage Collector thu hồi.

---

## 4. Vì sao String được thiết kế immutable?

Java thiết kế String là immutable nhằm mang lại nhiều lợi ích quan trọng.

---

### 4.1. An toàn trong môi trường đa luồng (Thread-safe)

Do nội dung String không thể thay đổi nên nhiều luồng có thể cùng sử dụng một String mà không gây ra xung đột dữ liệu.

#### Lợi ích

* Không cần đồng bộ hóa (synchronization).
* Tránh lỗi race condition.
* Giảm chi phí xử lý trong chương trình đa luồng.

---

### 4.2. Hỗ trợ String Pool

Java lưu trữ các String literal trong vùng nhớ đặc biệt gọi là String Pool.

Nếu nhiều biến cùng chứa một giá trị giống nhau thì chúng có thể dùng chung một object.

Ví dụ:

```java
String s1 = "Java";
String s2 = "Java";
```

Do String là immutable nên cả hai biến đều có thể tham chiếu tới cùng một object mà không sợ bị thay đổi ngoài ý muốn.

Điều này giúp:

* Tiết kiệm bộ nhớ.
* Tăng hiệu suất.

---

### 4.3. Làm key trong HashMap ổn định hơn

String thường được sử dụng làm key trong HashMap.

Khi một key được đưa vào HashMap, giá trị hash của nó được tính toán dựa trên nội dung chuỗi.

Nếu String có thể thay đổi thì:

* HashCode của key sẽ thay đổi.
* HashMap sẽ không thể tìm lại phần tử tương ứng.

Tính immutable đảm bảo:

* HashCode không thay đổi.
* Key luôn ổn định.
* Việc tìm kiếm trong HashMap diễn ra chính xác.

---

### 4.4. Tránh bị sửa ngoài ý muốn

Nếu một phương thức nhận vào String, phương thức đó không thể thay đổi nội dung String gốc.

Điều này giúp:

* Dữ liệu an toàn hơn.
* Dễ bảo trì chương trình.
* Hạn chế các lỗi khó phát hiện.

---

## 5. So sánh String immutable và mutable

| Đặc điểm                           | String            | StringBuilder / StringBuffer         |
| ---------------------------------- | ----------------- | ------------------------------------ |
| Có thay đổi nội dung được không    | Không             | Có                                   |
| Tạo object mới khi sửa chuỗi       | Có                | Không                                |
| Tiết kiệm bộ nhớ khi nối nhiều lần | Không             | Có                                   |
| Thread-safe                        | Có (do immutable) | StringBuffer có, StringBuilder không |
| Hỗ trợ String Pool                 | Có                | Không                                |

---

# Ví dụ

## Ví dụ 1: String không thay đổi

```java
String s1 = "Java";

s1.concat(" Core");

System.out.println(s1);
```

Kết quả:

```text
Java
```

Mặc dù gọi `concat()`, chuỗi ban đầu vẫn không thay đổi vì phương thức này trả về một String mới.

---

## Ví dụ 2: Gán lại kết quả

```java
String s1 = "Java";

s1 = s1.concat(" Core");

System.out.println(s1);
```

Kết quả:

```text
Java Core
```

Object `"Java Core"` được tạo mới và biến `s1` trỏ đến object này.

---

## Ví dụ 3: String Pool

```java
String s1 = "Java";
String s2 = "Java";

System.out.println(s1 == s2);
```

Kết quả:

```text
true
```

Hai biến cùng tham chiếu tới một object trong String Pool.

---

## Ví dụ 4: Tạo String bằng new

```java
String s1 = "Java";
String s2 = new String("Java");

System.out.println(s1 == s2);
```

Kết quả:

```text
false
```

Do `new String()` luôn tạo ra object mới trên Heap.

---

## Ví dụ 5: String làm key trong HashMap

```java
Map<String, Integer> map = new HashMap<>();

map.put("Java", 1);

System.out.println(map.get("Java"));
```

Kết quả:

```text
1
```

Nhờ tính immutable, giá trị hash của `"Java"` luôn ổn định nên HashMap có thể tìm lại phần tử một cách chính xác.

---

# Tổng kết

* `String` là class dùng để biểu diễn chuỗi ký tự trong Java.
* `String` là immutable, nghĩa là nội dung của object không thể thay đổi sau khi được tạo ra.
* Khi thực hiện các thao tác sửa chuỗi, Java sẽ tạo ra một object String mới thay vì thay đổi object cũ.
* Tính immutable giúp:

  * An toàn trong môi trường đa luồng.
  * Hỗ trợ String Pool để tiết kiệm bộ nhớ.
  * Làm key trong HashMap ổn định hơn.
  * Tránh việc dữ liệu bị thay đổi ngoài ý muốn.
* Nếu cần thay đổi chuỗi nhiều lần, nên sử dụng `StringBuilder` hoặc `StringBuffer` để đạt hiệu suất tốt hơn.
