# Các cách tối ưu Query phổ biến

**Optimize Query** là quá trình cải thiện câu lệnh SQL để truy vấn chạy nhanh hơn, sử dụng ít tài nguyên hơn và giảm thời gian phản hồi của hệ thống.

## 1. Sử dụng Index hợp lý

Tạo Index trên các cột thường xuất hiện trong:

* `WHERE`
* `JOIN`
* `ORDER BY`
* `GROUP BY`

Tuy nhiên, không nên lạm dụng Index vì chúng làm tăng dung lượng lưu trữ và làm chậm các thao tác `INSERT`, `UPDATE`, `DELETE`.

---

## 2. Chỉ lấy các cột cần thiết (Tránh `SELECT *`)

`SELECT *` sẽ lấy tất cả các cột trong bảng, làm tăng:

* I/O
* Băng thông mạng
* Bộ nhớ sử dụng

Chỉ nên lấy những cột thực sự cần thiết.

---

## 3. Lọc dữ liệu càng sớm càng tốt

Giảm số lượng bản ghi phải xử lý ngay từ đầu bằng cách:

* Sử dụng `WHERE`.
* Đưa điều kiện lọc xuống trước các phép `JOIN`, `GROUP BY`, `ORDER BY`.

Việc này giúp giảm kích thước tập dữ liệu trung gian và tăng hiệu năng.

---

## 4. Tránh dùng hàm trên cột trong mệnh đề WHERE

Khi áp dụng hàm lên một cột, Database thường không thể tận dụng Index trên cột đó.

Ví dụ các hàm như:

* `DATE()`
* `YEAR()`
* `UPPER()`
* `LOWER()`

có thể khiến truy vấn chuyển sang Full Table Scan.

---

## 5. Tối ưu JOIN

* Đảm bảo các cột dùng để `JOIN` có Index.
* Hạn chế JOIN quá nhiều bảng.
* Có thể cân nhắc Denormalization nếu phải JOIN quá nhiều.

JOIN hiệu quả giúp giảm đáng kể thời gian thực thi truy vấn.

---

## 6. Phân tích Execution Plan

Sử dụng:

* `EXPLAIN` (MySQL, PostgreSQL)
* Execution Plan (SQL Server)

để xem Database thực thi truy vấn như thế nào.

Thông qua Execution Plan, có thể phát hiện:

* Table Scan
* Full Index Scan
* Missing Index
* Chi phí của từng bước thực thi

---

## 7. Giới hạn số lượng kết quả trả về

Nếu không cần toàn bộ dữ liệu, hãy sử dụng:

* `LIMIT` (MySQL, PostgreSQL)
* `TOP` (SQL Server)

Điều này giúp giảm:

* Bộ nhớ sử dụng.
* Thời gian phản hồi.
* Lượng dữ liệu truyền qua mạng.

---

## 8. Chia nhỏ truy vấn phức tạp

Một truy vấn quá lớn với nhiều:

* `JOIN`
* `Subquery`
* `GROUP BY`

có thể được tách thành nhiều truy vấn nhỏ hơn.

Có thể sử dụng:

* Temporary Table
* CTE (Common Table Expression)
* Materialized View

để lưu kết quả trung gian và cải thiện hiệu năng.

---

## 9. Sử dụng kiểu dữ liệu phù hợp

Việc lựa chọn kiểu dữ liệu đúng giúp:

* Giảm dung lượng lưu trữ.
* Tăng tốc độ so sánh và tính toán.

Ví dụ:

* `INT` nhanh hơn `VARCHAR` khi lưu số.
* `DATE`, `DATETIME` phù hợp cho dữ liệu thời gian.
* So sánh đúng kiểu dữ liệu giúp Database tận dụng Index tốt hơn.

---

## 10. Cân nhắc Normalization và Denormalization

### Normalization

* Giảm dư thừa dữ liệu.
* Dễ bảo trì.
* Có thể làm phát sinh nhiều `JOIN`.

### Denormalization

* Chấp nhận dư thừa dữ liệu.
* Giảm số lượng `JOIN`.
* Tăng tốc độ đọc dữ liệu.

Tùy vào bài toán mà lựa chọn mức độ chuẩn hóa phù hợp.

---

# Tổng kết

| Cách tối ưu                            | Mục đích                                              |
| -------------------------------------- | ----------------------------------------------------- |
| Sử dụng Index hợp lý                   | Tăng tốc tìm kiếm dữ liệu                             |
| Tránh `SELECT *`                       | Giảm I/O và băng thông mạng                           |
| Lọc dữ liệu sớm                        | Giảm lượng dữ liệu cần xử lý                          |
| Tránh hàm trong `WHERE`                | Giúp Database tận dụng Index                          |
| Tối ưu JOIN                            | Giảm chi phí kết hợp bảng                             |
| Phân tích Execution Plan               | Tìm điểm nghẽn của truy vấn                           |
| Giới hạn kết quả trả về                | Giảm thời gian phản hồi                               |
| Chia nhỏ truy vấn phức tạp             | Dễ tối ưu và bảo trì                                  |
| Chọn kiểu dữ liệu phù hợp              | Tăng hiệu năng xử lý                                  |
| Cân nhắc Normalization/Denormalization | Cân bằng giữa hiệu năng đọc và tính nhất quán dữ liệu |

> Nguyên tắc quan trọng nhất khi tối ưu Query là **giảm lượng dữ liệu mà Database phải đọc, xử lý và trả về càng nhiều càng tốt**.

---

# Ví dụ

## 1. Sử dụng Index

### Chưa tối ưu

```sql
SELECT *
FROM users
WHERE email = 'abc@example.com';
```

### Tạo Index

```sql
CREATE INDEX idx_users_email
ON users(email);
```

---

## 2. Tránh `SELECT *`

### Không nên

```sql
SELECT *
FROM users
WHERE active = 1;
```

### Nên

```sql
SELECT id, name, email
FROM users
WHERE active = 1;
```

---

## 3. Tránh dùng hàm trong WHERE

### Không tối ưu

```sql
SELECT *
FROM orders
WHERE DATE(created_at) = '2025-01-01';
```

### Tối ưu hơn

```sql
SELECT *
FROM orders
WHERE created_at >= '2025-01-01'
  AND created_at < '2025-01-02';
```

---

## 4. Tối ưu JOIN

```sql
SELECT o.id,
       c.customer_name
FROM orders o
JOIN customers c
    ON o.customer_id = c.id;
```

Tạo Index:

```sql
CREATE INDEX idx_orders_customer_id
ON orders(customer_id);
```

---

## 5. Phân tích Execution Plan

```sql
EXPLAIN
SELECT *
FROM orders
WHERE customer_id = 123;
```

Nếu kết quả:

| type | key  |
| ---- | ---- |
| ALL  | NULL |

thì Database đang thực hiện:

```text
Full Table Scan
```

Nếu:

| type | key             |
| ---- | --------------- |
| ref  | idx_customer_id |

thì truy vấn đang sử dụng Index.

---

## 6. Giới hạn số lượng bản ghi

```sql
SELECT *
FROM products
ORDER BY created_at DESC
LIMIT 10;
```

Chỉ lấy 10 sản phẩm mới nhất thay vì toàn bộ dữ liệu.

---

## 7. Sử dụng kiểu dữ liệu phù hợp

### Nên

```sql
CREATE TABLE users (
    id INT,
    age INT
);
```

### Không nên

```sql
CREATE TABLE users (
    id VARCHAR(20),
    age VARCHAR(10)
);
```

Ngoài ra, nên so sánh đúng kiểu:

```sql
SELECT *
FROM users
WHERE id = 1;
```

thay vì:

```sql
SELECT *
FROM users
WHERE id = '1';
```

---

## 8. Kiểm tra và tối ưu theo quy trình

```text
Viết SQL
    ↓
Phân tích bằng EXPLAIN / Execution Plan
    ↓
Tìm điểm nghẽn
    ↓
Tạo hoặc điều chỉnh Index
    ↓
Tối ưu WHERE, JOIN, SELECT
    ↓
Đo lại hiệu năng
```

> **Không tối ưu theo cảm tính. Hãy sử dụng EXPLAIN hoặc Execution Plan để xác định chính xác điểm nghẽn trước khi tối ưu.**
