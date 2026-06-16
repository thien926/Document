# Index là gì?

**Index (Chỉ mục)** là một cấu trúc dữ liệu đặc biệt (thường là **B-Tree**, một số hệ quản trị còn hỗ trợ **Hash**) được tạo trên một hoặc nhiều cột của bảng nhằm tăng tốc độ truy vấn dữ liệu.

Index lưu trữ các giá trị của cột theo thứ tự nhất định, kèm theo con trỏ đến vị trí thực tế của các bản ghi trong bảng. Nhờ đó, cơ sở dữ liệu có thể tìm kiếm dữ liệu nhanh hơn mà không cần phải quét toàn bộ bảng (Full Table Scan).

## Phân loại Index phổ biến

| Loại Index          | Đặc điểm                                                                                                              |
| ------------------- | --------------------------------------------------------------------------------------------------------------------- |
| Clustered Index     | Sắp xếp dữ liệu vật lý của bảng theo thứ tự của Index. Mỗi bảng chỉ có một Clustered Index.                           |
| Non-Clustered Index | Lưu riêng với dữ liệu, chứa giá trị cột và con trỏ đến dữ liệu thực tế. Một bảng có thể có nhiều Non-Clustered Index. |
| Unique Index        | Đảm bảo các giá trị trong cột là duy nhất.                                                                            |
| Composite Index     | Được tạo trên nhiều cột.                                                                                              |
| Full-Text Index     | Hỗ trợ tìm kiếm văn bản.                                                                                              |
| Hash Index          | Dựa trên hàm băm, phù hợp với các phép so sánh bằng (`=`).                                                            |

---

# Tổng kết

* **Index** là cấu trúc dữ liệu giúp tăng tốc độ truy vấn.
* Thường được cài đặt bằng **B-Tree**, một số hệ quản trị còn hỗ trợ **Hash Index**.
* Index lưu trữ giá trị của cột và con trỏ đến dữ liệu thực tế.
* Giúp giảm thời gian tìm kiếm dữ liệu và hạn chế việc quét toàn bộ bảng.
* Tăng tốc các thao tác `SELECT`, `WHERE`, `JOIN`, `ORDER BY`, `GROUP BY`.
* Đánh đổi bằng việc tiêu tốn thêm dung lượng lưu trữ và làm chậm các thao tác `INSERT`, `UPDATE`, `DELETE`.

## Ưu điểm và nhược điểm

| Ưu điểm                                            | Nhược điểm                                          |
| -------------------------------------------------- | --------------------------------------------------- |
| Tăng tốc độ truy vấn dữ liệu.                      | Tốn thêm không gian lưu trữ.                        |
| Tăng hiệu suất cho `JOIN`, `ORDER BY`, `GROUP BY`. | Làm chậm `INSERT`, `UPDATE`, `DELETE`.              |
| Giảm số lần quét toàn bộ bảng.                     | Quá nhiều Index có thể làm giảm hiệu năng tổng thể. |

---

# Ví dụ

Giả sử bảng `Employee`:

|  id | employee_name | email                                     |
| --: | ------------- | ----------------------------------------- |
|   1 | An            | [an@gmail.com](mailto:an@gmail.com)       |
|   2 | Bình          | [binh@gmail.com](mailto:binh@gmail.com)   |
|   3 | Cường         | [cuong@gmail.com](mailto:cuong@gmail.com) |
| ... | ...           | ...                                       |

Truy vấn:

```sql
SELECT *
FROM Employee
WHERE email = 'binh@gmail.com';
```

### Không có Index

```text
Bảng Employee
---------------------------------
An
Bình
Cường
...
↓
Duyệt từng dòng (Full Table Scan)
↓
Tìm thấy Bình
```

Độ phức tạp gần:

```text
O(n)
```

---

### Có Index trên cột `email`

```sql
CREATE INDEX idx_employee_email
ON Employee(email);
```

Cơ sở dữ liệu sẽ tạo cấu trúc như:

```text
Index(email)

an@gmail.com      → Row 1
binh@gmail.com    → Row 2
cuong@gmail.com   → Row 3
...
```

Khi thực hiện:

```sql
SELECT *
FROM Employee
WHERE email = 'binh@gmail.com';
```

Database sẽ:

```text
Tìm trong Index
↓
Lấy Row 2
↓
Trả về dữ liệu
```

Độ phức tạp của B-Tree xấp xỉ:

```text
O(log n)
```

---

## Composite Index

Tạo Index trên nhiều cột:

```sql
CREATE INDEX idx_dept_salary
ON Employee(department_id, salary);
```

Index được sắp xếp theo:

```text
(department_id, salary)
```

Các truy vấn sau sẽ tận dụng được Index:

```sql
SELECT *
FROM Employee
WHERE department_id = 1;
```

```sql
SELECT *
FROM Employee
WHERE department_id = 1
  AND salary > 1000;
```

Nhưng truy vấn:

```sql
SELECT *
FROM Employee
WHERE salary > 1000;
```

thường sẽ **không tận dụng được Index một cách hiệu quả**, do tuân theo nguyên tắc **Leftmost Prefix**.

---

## Khi nào nên tạo Index?

Nên tạo Index cho các cột:

* Thường xuyên xuất hiện trong `WHERE`.
* Thường được dùng để `JOIN`.
* Thường dùng trong `ORDER BY`.
* Thường dùng trong `GROUP BY`.
* Là `PRIMARY KEY`, `UNIQUE KEY`, `FOREIGN KEY`.

## Khi nào không nên tạo Index?

* Bảng có rất ít dữ liệu.
* Các cột thường xuyên bị cập nhật.
* Các cột có ít giá trị khác nhau (ví dụ: giới tính chỉ có Nam/Nữ).
* Tạo quá nhiều Index trên cùng một bảng.

> Trong thực tế, **Clustered Index** và **Non-Clustered Index** là hai loại Index quan trọng nhất. Một hệ quản trị cơ sở dữ liệu thường mặc định tạo Index cho `PRIMARY KEY`, vì đây là cột được truy cập nhiều nhất.
