# Optimize Query là gì?

**Optimize Query (Tối ưu truy vấn)** là quá trình cải thiện câu lệnh truy vấn cơ sở dữ liệu (thường là SQL) nhằm giúp truy vấn thực thi nhanh hơn, sử dụng ít tài nguyên hệ thống hơn (CPU, RAM, I/O) và giảm thời gian phản hồi.

Mục tiêu của việc tối ưu truy vấn là giúp ứng dụng hoạt động ổn định và hiệu quả, đặc biệt khi xử lý lượng dữ liệu lớn hoặc số lượng người dùng đồng thời cao.

Ví dụ, một truy vấn lấy danh sách đơn hàng có thể mất **10 giây** trên bảng chứa 1 triệu bản ghi. Sau khi tối ưu, thời gian thực thi có thể giảm xuống chỉ còn **0.1 giây**.

## Các cách tối ưu truy vấn phổ biến

| Cách tối ưu                   | Mục đích                                         |
| ----------------------------- | ------------------------------------------------ |
| Tạo Index phù hợp             | Giảm Full Table Scan, tăng tốc tìm kiếm dữ liệu. |
| Chỉ lấy các cột cần thiết     | Giảm lượng dữ liệu phải đọc và truyền đi.        |
| Hạn chế Subquery phức tạp     | Giảm số lần thực thi không cần thiết.            |
| Sử dụng JOIN hợp lý           | Tránh lặp truy vấn hoặc xử lý dư thừa.           |
| Dùng `EXPLAIN` để phân tích   | Kiểm tra kế hoạch thực thi của Database.         |
| Tránh hàm trên cột được Index | Giúp Database tận dụng Index hiệu quả.           |
| Phân trang dữ liệu            | Giảm số lượng bản ghi phải xử lý mỗi lần.        |

---

# Tổng kết

* Optimize Query là quá trình làm cho truy vấn chạy nhanh hơn và sử dụng ít tài nguyên hơn.
* Mục tiêu là giảm thời gian phản hồi và tăng hiệu năng hệ thống.
* Việc tối ưu không chỉ liên quan đến câu lệnh SQL mà còn bao gồm thiết kế Index và cấu trúc dữ liệu.
* Công cụ quan trọng nhất để phân tích truy vấn là `EXPLAIN` (hoặc Execution Plan).
* Tối ưu truy vấn đặc biệt quan trọng với các hệ thống có dữ liệu lớn.

---

# Ví dụ

Giả sử bảng `Orders` chứa 1 triệu bản ghi.

## Truy vấn chưa tối ưu

```sql
SELECT *
FROM Orders
WHERE customer_id = 100;
```

Nếu cột `customer_id` chưa có Index, Database phải:

```text
Full Table Scan
↓
Đọc 1.000.000 dòng
↓
Tìm các dòng có customer_id = 100
↓
Trả kết quả
```

Thời gian thực thi có thể:

```text
10 giây
```

---

## Sau khi tạo Index

```sql
CREATE INDEX idx_customer_id
ON Orders(customer_id);
```

Truy vấn:

```sql
SELECT *
FROM Orders
WHERE customer_id = 100;
```

Quá trình:

```text
Tìm trong Index
↓
Lấy vị trí dữ liệu
↓
Đọc các bản ghi cần thiết
↓
Trả kết quả
```

Thời gian thực thi có thể giảm xuống:

```text
0.1 giây
```

---

## Chỉ lấy các cột cần thiết

### Chưa tối ưu

```sql
SELECT *
FROM Orders
WHERE customer_id = 100;
```

Database phải đọc toàn bộ các cột:

```text
order_id
customer_id
amount
address
description
created_date
...
```

### Tối ưu hơn

```sql
SELECT order_id, amount
FROM Orders
WHERE customer_id = 100;
```

Chỉ đọc những dữ liệu cần thiết, giúp giảm I/O và tăng tốc độ truy vấn.

---

## Dùng EXPLAIN để phân tích

```sql
EXPLAIN
SELECT *
FROM Orders
WHERE customer_id = 100;
```

Nếu kết quả:

| type | key  |
| ---- | ---- |
| ALL  | NULL |

thì Database đang thực hiện:

```text
Full Table Scan
```

Nếu kết quả:

| type | key             |
| ---- | --------------- |
| ref  | idx_customer_id |

thì truy vấn đang sử dụng:

```text
Index idx_customer_id
```

---

## Quy trình tối ưu truy vấn

```text
Viết câu SQL
      ↓
Phân tích bằng EXPLAIN
      ↓
Xác định điểm nghẽn
      ↓
Tạo hoặc điều chỉnh Index
      ↓
Viết lại câu SQL nếu cần
      ↓
Kiểm tra lại hiệu năng
```

> **Tối ưu truy vấn không phải là viết SQL "ngắn hơn", mà là giúp Database đọc ít dữ liệu nhất và thực hiện ít công việc nhất để trả về kết quả mong muốn.**
