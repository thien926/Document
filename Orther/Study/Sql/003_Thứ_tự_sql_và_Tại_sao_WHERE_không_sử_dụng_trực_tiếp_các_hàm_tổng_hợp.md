## Tại sao `WHERE` không sử dụng trực tiếp các hàm tổng hợp (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`)?

`WHERE` dùng để lọc các bản ghi riêng lẻ trước khi dữ liệu được gom nhóm và trước khi các hàm tổng hợp được tính toán.

Trong khi đó, các hàm tổng hợp như `COUNT()`, `SUM()`, `AVG()`, `MAX()`, `MIN()` chỉ có kết quả sau khi SQL đã gom nhóm dữ liệu (`GROUP BY`). Vì vậy, tại thời điểm thực hiện `WHERE`, kết quả của các hàm tổng hợp vẫn chưa tồn tại.

Do đó:

* `WHERE` **không thể** sử dụng trực tiếp các hàm tổng hợp.
* Muốn lọc dựa trên kết quả của các hàm tổng hợp, phải sử dụng `HAVING`.

---

## Thứ tự thực hiện đầy đủ của một câu lệnh SQL

Mặc dù khi viết câu lệnh SQL, chúng ta thường viết theo thứ tự:

```sql
SELECT ...
FROM ...
JOIN ...
WHERE ...
GROUP BY ...
HAVING ...
ORDER BY ...
LIMIT ...
```

Nhưng SQL **không thực hiện theo thứ tự viết**, mà theo thứ tự sau:

| Thứ tự | Mệnh đề                                                | Chức năng                                           |
| ------ | ------------------------------------------------------ | --------------------------------------------------- |
| 1      | `FROM`                                                 | Chọn bảng dữ liệu nguồn.                            |
| 2      | `JOIN` + `ON`                                          | Kết hợp dữ liệu từ nhiều bảng.                      |
| 3      | `WHERE`                                                | Lọc các dòng dữ liệu.                               |
| 4      | `GROUP BY`                                             | Gom các dòng thành từng nhóm.                       |
| 5      | Các hàm tổng hợp (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`) | Tính toán trên từng nhóm.                           |
| 6      | `HAVING`                                               | Lọc các nhóm sau khi đã tính toán các hàm tổng hợp. |
| 7      | `SELECT`                                               | Chọn các cột cần hiển thị.                          |
| 8      | `DISTINCT`                                             | Loại bỏ các bản ghi trùng lặp.                      |
| 9      | `ORDER BY`                                             | Sắp xếp kết quả.                                    |
| 10     | `LIMIT` / `OFFSET`                                     | Giới hạn số lượng bản ghi trả về.                   |

---

## Minh họa

Giả sử có câu lệnh:

```sql
SELECT department,
       COUNT(*) AS total_employee
FROM Employee
WHERE salary >= 1000
GROUP BY department
HAVING COUNT(*) > 2
ORDER BY total_employee DESC
LIMIT 5;
```

SQL sẽ thực hiện như sau:

### Bước 1: `FROM`

Lấy dữ liệu từ bảng `Employee`.

### Bước 2: `WHERE`

```sql
WHERE salary >= 1000
```

Loại bỏ những nhân viên có lương nhỏ hơn 1000.

### Bước 3: `GROUP BY`

```sql
GROUP BY department
```

Gom các nhân viên theo từng phòng ban.

### Bước 4: Tính các hàm tổng hợp

```sql
COUNT(*)
```

Đếm số nhân viên trong từng phòng ban.

### Bước 5: `HAVING`

```sql
HAVING COUNT(*) > 2
```

Chỉ giữ lại những phòng ban có nhiều hơn 2 nhân viên.

### Bước 6: `SELECT`

```sql
SELECT department, COUNT(*) AS total_employee
```

Chọn các cột cần hiển thị.

### Bước 7: `ORDER BY`

```sql
ORDER BY total_employee DESC
```

Sắp xếp theo số lượng nhân viên giảm dần.

### Bước 8: `LIMIT`

```sql
LIMIT 5
```

Chỉ lấy 5 kết quả đầu tiên.

---

## Tại sao `WHERE COUNT(*) > 2` lại sai?

```sql
SELECT department, COUNT(*)
FROM Employee
WHERE COUNT(*) > 2
GROUP BY department;
```

Vì `WHERE` được thực hiện ở bước 3, trong khi `COUNT(*)` chỉ được tính ở bước 5.

Nói cách khác:

> **Khi `WHERE` chạy, SQL vẫn chưa biết `COUNT(*)` bằng bao nhiêu.**

Do đó phải viết:

```sql
SELECT department, COUNT(*)
FROM Employee
GROUP BY department
HAVING COUNT(*) > 2;
```

---

## Tổng kết

* `WHERE` lọc các dòng dữ liệu trước khi gom nhóm.
* `GROUP BY` tạo ra các nhóm dữ liệu.
* Các hàm tổng hợp (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`) được tính sau `GROUP BY`.
* `HAVING` dùng để lọc các nhóm dựa trên kết quả của các hàm tổng hợp.
* Vì `WHERE` được thực hiện trước khi các hàm tổng hợp được tính, nên `WHERE` không thể sử dụng trực tiếp `COUNT()`, `SUM()`, `AVG()`, `MAX()`, `MIN()`.
