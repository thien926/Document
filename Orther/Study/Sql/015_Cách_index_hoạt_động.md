# Cách Index hoạt động

Index là một cấu trúc dữ liệu riêng được Database tạo ra để lưu:

```text
Giá trị cột
        ↓
Con trỏ đến vị trí bản ghi trong bảng
```

Khi truy vấn, Database sẽ tìm trong Index trước, sau đó mới truy cập đến dữ liệu thật trong bảng.

---

# 1. B-Tree Index (phổ biến nhất)

B-Tree là cây cân bằng, mỗi node chứa nhiều khóa và các con trỏ đến node con.

Ví dụ:

Giả sử cột `ID` có các giá trị:

```text
1, 4, 7, 10, 14, 18, 22, 25, 30, 35
```

Cây B-Tree đơn giản:

```text
             [14]
            /    \
        [4,10]  [25,30]
        / | \    / | \
      [1][4][7][18][22][25] ...
```

(Các leaf node chứa con trỏ đến hàng dữ liệu.)

---

## Cách tìm kiếm ID = 18

### Bước 1

Bắt đầu từ node gốc:

```text
[14]
```

So sánh:

```text
18 > 14
```

⇒ đi sang nhánh phải.

---

### Bước 2

Đến node:

```text
[25,30]
```

So sánh:

```text
18 < 25
```

⇒ đi sang nhánh trái.

---

### Bước 3

Đến node lá:

```text
[18]
```

Tìm thấy khóa 18.

↓

Lấy con trỏ tới hàng dữ liệu tương ứng.

---

## Độ phức tạp

Mỗi lần tìm kiếm chỉ cần đi qua vài tầng của cây:

```text
O(log n)
```

Ngay cả khi bảng có hàng triệu bản ghi, chiều cao cây thường chỉ khoảng 3–4 tầng.

---

## B-Tree hỗ trợ rất tốt

### So sánh bằng

```sql
WHERE ID = 18
```

### So sánh lớn hơn, nhỏ hơn

```sql
WHERE ID > 10

WHERE ID <= 50
```

### BETWEEN

```sql
WHERE ID BETWEEN 10 AND 25
```

### ORDER BY

```sql
ORDER BY ID
```

### LIKE tiền tố

```sql
WHERE Name LIKE 'abc%'
```

---

# 2. Hash Index

Hash Index sử dụng hàm băm (Hash Function).

Thay vì lưu theo dạng cây, Database sẽ:

```text
Giá trị
    ↓
Hash Function
    ↓
Bucket
    ↓
Con trỏ tới dữ liệu
```

Ví dụ:

```text
hash('a@b.com') = 5
```

Bảng hash:

| Bucket | Email                     |
| ------ | ------------------------- |
| 1      | [x@z.com](mailto:x@z.com) |
| 3      | [m@n.com](mailto:m@n.com) |
| 5      | [a@b.com](mailto:a@b.com) |

---

## Cách tìm email = '[a@b.com](mailto:a@b.com)'

### Bước 1

Tính:

```text
hash('a@b.com') = 5
```

### Bước 2

Truy cập trực tiếp bucket số 5.

### Bước 3

Kiểm tra giá trị trong bucket.

### Bước 4

Lấy con trỏ đến hàng dữ liệu.

---

## Độ phức tạp

Trung bình:

```text
O(1)
```

Rất nhanh.

---

## Hạn chế của Hash Index

Không hỗ trợ:

### Tìm kiếm theo khoảng

```sql
WHERE ID > 100
```

### BETWEEN

```sql
WHERE ID BETWEEN 10 AND 20
```

### ORDER BY

```sql
ORDER BY ID
```

### LIKE tiền tố

```sql
WHERE Name LIKE 'abc%'
```

Hash Index phù hợp chủ yếu với:

```sql
=
IN
```

---

# Tóm tắt

| Loại Index | Cách tìm kiếm | Độ phức tạp     | Hỗ trợ tìm khoảng |
| ---------- | ------------- | --------------- | ----------------- |
| B-Tree     | Duyệt cây     | O(log n)        | Có                |
| Hash       | Băm → Bucket  | O(1) trung bình | Không             |

## Tổng kết

* **B-Tree Index** là loại Index phổ biến nhất và được hầu hết các DBMS sử dụng mặc định.
* B-Tree hỗ trợ rất tốt các truy vấn `=`, `<`, `>`, `BETWEEN`, `ORDER BY`, `LIKE 'abc%'`.
* **Hash Index** nhanh hơn trong trường hợp tìm kiếm chính xác (`=` hoặc `IN`) với độ phức tạp trung bình **O(1)**.
* Hash Index không hỗ trợ tìm kiếm theo khoảng do dữ liệu không được lưu theo thứ tự.
* Khi nói đến "Index" trong MySQL, SQL Server, PostgreSQL hay Oracle, thông thường người ta đang nói tới **B-Tree Index**.
