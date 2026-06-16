# Phân biệt Query, View, Stored Procedure và Function trong SQL

## 1. Query (Câu truy vấn)

### Khái niệm

Query là đoạn mã SQL cơ bản nhất được sử dụng để giao tiếp trực tiếp với cơ sở dữ liệu. Thông thường, Query là các câu lệnh `SELECT`, nhưng cũng có thể là `INSERT`, `UPDATE`, `DELETE`.

### Đặc điểm

* Chỉ tồn tại trong lúc thực thi.
* Được DBMS phân tích và thực thi mỗi khi chạy.
* Thích hợp cho các thao tác đơn giản hoặc truy vấn tạm thời.
* Không được lưu lại dưới dạng đối tượng trong cơ sở dữ liệu.

---

## 2. View (Bảng ảo)

### Khái niệm

View là một câu lệnh `SELECT` được lưu lại trong cơ sở dữ liệu dưới dạng một đối tượng và có thể được sử dụng giống như một bảng thông thường.

View không lưu dữ liệu thực tế mà chỉ lưu định nghĩa của câu truy vấn.

### Đặc điểm

* Hoạt động như một bảng ảo.
* Không chiếm thêm không gian lưu trữ dữ liệu (ngoại trừ Materialized View).
* Dữ liệu trong View luôn đồng bộ với bảng gốc.
* Giúp che giấu các cột nhạy cảm.
* Đơn giản hóa các câu truy vấn phức tạp, đặc biệt là các câu lệnh JOIN.

### Tác dụng

* Bảo mật dữ liệu.
* Tái sử dụng các câu truy vấn phức tạp.
* Giúp người dùng thao tác dễ dàng hơn.
* Giảm độ phức tạp của hệ thống.

---

## 3. Stored Procedure (Thủ tục lưu trữ)

### Khái niệm

Stored Procedure là một tập hợp nhiều câu lệnh SQL được biên dịch và lưu trữ sẵn trong cơ sở dữ liệu.

Ngoài SQL, Procedure còn hỗ trợ:

* Biến
* Điều kiện `IF...ELSE`
* Vòng lặp `WHILE`
* Transaction
* Xử lý ngoại lệ

### Đặc điểm

* Chứa nhiều câu lệnh SQL.

* Được biên dịch trước, giúp tăng hiệu năng.

* Có thể thay đổi dữ liệu bằng:

  * `INSERT`
  * `UPDATE`
  * `DELETE`

* Có thể có hoặc không trả về dữ liệu.

* Thường dùng để xử lý nghiệp vụ phức tạp.

### Tác dụng

* Xử lý nghiệp vụ tập trung trên Database.
* Tăng hiệu năng nhờ cơ chế biên dịch trước.
* Hạn chế việc truyền nhiều câu lệnh SQL từ ứng dụng xuống Database.
* Tái sử dụng logic nghiệp vụ.

---

## 4. Function (Hàm)

### Khái niệm

Function là một khối mã được thiết kế để thực hiện tính toán và bắt buộc phải trả về kết quả.

Kết quả trả về có thể là:

* Một giá trị duy nhất (**Scalar Function**).
* Một bảng dữ liệu (**Table-Valued Function**).

### Đặc điểm

* Bắt buộc phải có `RETURN`.

* Có thể được gọi trực tiếp trong câu lệnh `SELECT`.

* Thường dùng để:

  * Tính toán.
  * Xử lý chuỗi.
  * Xử lý ngày tháng.
  * Chuyển đổi dữ liệu.

* Không được phép làm thay đổi dữ liệu của bảng gốc (`INSERT`, `UPDATE`, `DELETE`) trong hầu hết các hệ quản trị cơ sở dữ liệu.

### Tác dụng

* Đóng gói các phép tính thường dùng.
* Tái sử dụng logic tính toán.
* Làm cho câu lệnh SQL dễ đọc hơn.

---

# Tổng kết

| Tiêu chí                    | Query             | View                 | Stored Procedure | Function  |
| --------------------------- | ----------------- | -------------------- | ---------------- | --------- |
| Được lưu trong Database     | ❌                 | ✅                    | ✅                | ✅         |
| Chứa nhiều câu lệnh SQL     | ❌                 | ❌                    | ✅                | Có thể    |
| Có tham số                  | ❌                 | ❌                    | ✅                | ✅         |
| Có thể dùng trong SELECT    | ✅                 | ✅                    | ❌                | ✅         |
| Có thể INSERT/UPDATE/DELETE | ✅                 | Một số trường hợp    | ✅                | ❌         |
| Bắt buộc trả về giá trị     | ❌                 | ❌                    | ❌                | ✅         |
| Phù hợp với                 | Truy vấn đơn giản | Tái sử dụng truy vấn | Xử lý nghiệp vụ  | Tính toán |
| Hỗ trợ IF, WHILE            | ❌                 | ❌                    | ✅                | Hạn chế   |
| Được biên dịch trước        | ❌                 | ❌                    | ✅                | ✅         |

## Cách ghi nhớ nhanh

```text
Query
↓
Viết và chạy ngay.

View
↓
Lưu một câu SELECT để dùng lại như bảng.

Stored Procedure
↓
Lưu một chương trình SQL hoàn chỉnh.

Function
↓
Lưu một phép tính và luôn phải trả về kết quả.
```

---

# Ví dụ

## 1. Query

Tìm nhân viên thuộc phòng IT:

```sql
SELECT *
FROM NhanVien
WHERE PhongBan = 'IT';
```

---

## 2. View

Tạo View chứa nhân viên phòng IT:

```sql
CREATE VIEW View_NhanVienIT AS
SELECT Id, HoTen
FROM NhanVien
WHERE PhongBan = 'IT';
```

Sử dụng:

```sql
SELECT *
FROM View_NhanVienIT;
```

---

## 3. Stored Procedure

Tăng lương cho toàn bộ nhân viên của một phòng ban:

```sql
CREATE PROCEDURE TangLuongPhongBan
    @TenPhong NVARCHAR(50),
    @PhanTram INT
AS
BEGIN
    UPDATE NhanVien
    SET Luong = Luong * (1 + @PhanTram / 100.0)
    WHERE PhongBan = @TenPhong;
END;
```

Gọi Procedure:

```sql
EXEC TangLuongPhongBan 'IT', 10;
```

---

## 4. Function

Tính thuế thu nhập của nhân viên:

```sql
CREATE FUNCTION TinhThueThuNhap
(
    @Luong DECIMAL(18,2)
)
RETURNS DECIMAL(18,2)
AS
BEGIN
    RETURN @Luong * 0.1;
END;
```

Sử dụng trong câu lệnh `SELECT`:

```sql
SELECT HoTen,
       dbo.TinhThueThuNhap(Luong) AS ThuePhaiNop
FROM NhanVien;
```

---

## Ví dụ thực tế

Giả sử hệ thống bán hàng:

* **Query**: Tìm đơn hàng của khách hàng A.

```sql
SELECT *
FROM Orders
WHERE CustomerId = 1;
```

* **View**: Tạo bảng ảo chứa thông tin đơn hàng và khách hàng.

```sql
CREATE VIEW View_OrderDetail AS
SELECT o.Id,
       c.CustomerName,
       o.TotalAmount
FROM Orders o
JOIN Customers c
ON o.CustomerId = c.Id;
```

* **Stored Procedure**: Xử lý nghiệp vụ tạo đơn hàng.

```sql
EXEC CreateOrder @CustomerId = 1;
```

* **Function**: Tính thuế VAT.

```sql
SELECT dbo.CalculateVAT(TotalAmount)
FROM Orders;
```

> **Query dùng để truy vấn, View dùng để lưu truy vấn, Stored Procedure dùng để xử lý nghiệp vụ, còn Function dùng để tính toán và luôn phải trả về kết quả.**
