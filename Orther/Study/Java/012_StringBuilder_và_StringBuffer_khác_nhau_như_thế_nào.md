## StringBuilder và StringBuffer khác nhau như thế nào?

### 1. Điểm giống nhau

`StringBuilder` và `StringBuffer` đều được sử dụng để xử lý chuỗi **mutable** (chuỗi có thể thay đổi).

#### Chức năng

* Cho phép thêm (`append()`), chèn (`insert()`), xóa (`delete()`), thay thế (`replace()`) nội dung chuỗi.
* Thay đổi trực tiếp trên đối tượng hiện tại, không tạo ra nhiều đối tượng `String` mới.

#### Lợi ích

* Giảm số lượng object được tạo ra.
* Tiết kiệm bộ nhớ.
* Tăng hiệu năng khi phải thao tác với chuỗi nhiều lần.

---

### 2. StringBuilder

#### Đặc điểm

* Không được đồng bộ hóa (không sử dụng `synchronized`).
* Không bảo đảm an toàn khi nhiều luồng cùng truy cập.

#### Hiệu năng

* Nhanh hơn `StringBuffer`.
* Phù hợp với môi trường đơn luồng (single-thread).

#### Khi nào nên sử dụng?

Sử dụng `StringBuilder` khi:

* Chương trình chỉ có một luồng xử lý.
* Không có nhiều thread cùng thao tác trên một chuỗi.
* Cần hiệu năng cao.

---

### 3. StringBuffer

#### Đặc điểm

* Các phương thức được đồng bộ hóa bằng `synchronized`.
* Bảo đảm an toàn khi nhiều luồng cùng truy cập.

#### Hiệu năng

* Chậm hơn `StringBuilder`.
* Tốn thêm chi phí cho việc đồng bộ hóa.

#### Khi nào nên sử dụng?

Sử dụng `StringBuffer` khi:

* Có nhiều thread cùng thao tác trên một đối tượng chuỗi.
* Cần đảm bảo tính nhất quán của dữ liệu trong môi trường đa luồng.

---

### 4. So sánh StringBuilder và StringBuffer

| Tiêu chí                     | StringBuilder | StringBuffer |
| ---------------------------- | ------------- | ------------ |
| Khả năng thay đổi chuỗi      | Có            | Có           |
| Tạo nhiều object String mới  | Không         | Không        |
| Đồng bộ hóa (`synchronized`) | Không         | Có           |
| Thread-safe                  | Không         | Có           |
| Hiệu năng                    | Nhanh hơn     | Chậm hơn     |
| Môi trường phù hợp           | Đơn luồng     | Đa luồng     |

---

## Ví dụ

### Ví dụ với StringBuilder

```java
StringBuilder sb = new StringBuilder();

sb.append("Hello");
sb.append(" ");
sb.append("Java");

System.out.println(sb);
```

Kết quả:

```text
Hello Java
```

Trong quá trình thêm chuỗi, đối tượng `sb` được thay đổi trực tiếp mà không tạo ra nhiều đối tượng `String` mới.

---

### Ví dụ với StringBuffer

```java
StringBuffer sb = new StringBuffer();

sb.append("Hello");
sb.append(" ");
sb.append("Java");

System.out.println(sb);
```

Kết quả:

```text
Hello Java
```

Cách sử dụng gần như giống hệt `StringBuilder`, nhưng các phương thức của `StringBuffer` được đồng bộ hóa để hỗ trợ môi trường đa luồng.

---

## Tổng kết

* `StringBuilder` và `StringBuffer` đều dùng để xử lý chuỗi mutable, giúp thay đổi nội dung chuỗi mà không tạo quá nhiều object `String` mới.
* `StringBuilder` không sử dụng `synchronized`, do đó có hiệu năng cao hơn và thích hợp cho môi trường đơn luồng.
* `StringBuffer` sử dụng `synchronized`, bảo đảm an toàn trong môi trường đa luồng nhưng thường chậm hơn.
* Trong hầu hết các trường hợp thông thường, `StringBuilder` là lựa chọn ưu tiên. Chỉ nên dùng `StringBuffer` khi cần chia sẻ đối tượng chuỗi giữa nhiều thread.
