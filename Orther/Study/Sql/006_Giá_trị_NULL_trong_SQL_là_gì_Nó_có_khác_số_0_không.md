## Giá trị NULL trong SQL là gì? Nó có khác số 0 không?

`NULL` biểu thị dữ liệu **không có giá trị**, **chưa biết** hoặc **chưa được xác định**. Nó không phải là chuỗi rỗng (`''`), khoảng trắng (`' '`), hay số `0`.

| Tiêu chí                   | NULL                                | 0                            |
| -------------------------- | ----------------------------------- | ---------------------------- |
| Ý nghĩa                    | Không có giá trị hoặc chưa xác định | Một giá trị số cụ thể        |
| Kiểu dữ liệu               | Không thuộc kiểu dữ liệu cụ thể nào | Kiểu số                      |
| Có thể thực hiện phép toán | Kết quả thường là `NULL`            | Có                           |
| So sánh bằng `=`           | Không                               | Có                           |
| Kiểm tra giá trị           | Dùng `IS NULL`, `IS NOT NULL`       | Dùng `=`, `<>`, `>`, `<`,... |

## Tổng kết

* **NULL** đại diện cho dữ liệu chưa có hoặc chưa xác định, không phải là số `0` hay chuỗi rỗng.
* Không thể kiểm tra `NULL` bằng toán tử `=` hoặc `<>`.
* Để kiểm tra giá trị `NULL`, phải sử dụng `IS NULL` hoặc `IS NOT NULL`.
* Các phép toán với `NULL` thường trả về kết quả `NULL`.
* Các hàm tổng hợp như `SUM()`, `AVG()`, `MAX()`, `MIN()` sẽ tự động bỏ qua các giá trị `NULL`.

## Ví dụ

Giả sử bảng `Employee`:

| employee_id | employee_name | bonus |
| ----------: | ------------- | ----: |
|         101 | An            |   500 |
|         102 | Bình          |  NULL |
|         103 | Cường         |  1000 |

### 1. So sánh với NULL bằng `=` là sai

```sql
SELECT *
FROM Employee
WHERE bonus = NULL;
```

Câu lệnh trên sẽ không trả về kết quả nào.

---

### 2. Cách đúng để tìm các bản ghi có giá trị NULL

```sql
SELECT *
FROM Employee
WHERE bonus IS NULL;
```

Kết quả:

| employee_id | employee_name | bonus |
| ----------: | ------------- | ----: |
|         102 | Bình          |  NULL |

---

### 3. Tìm các bản ghi không phải NULL

```sql
SELECT *
FROM Employee
WHERE bonus IS NOT NULL;
```

Kết quả:

| employee_id | employee_name | bonus |
| ----------: | ------------- | ----: |
|         101 | An            |   500 |
|         103 | Cường         |  1000 |

---

### 4. NULL khác với số 0

Giả sử bảng:

| employee_name | bonus |
| ------------- | ----: |
| An            |     0 |
| Bình          |  NULL |

* `bonus = 0`: nhân viên không được thưởng.
* `bonus = NULL`: chưa biết hoặc chưa nhập thông tin thưởng.

Do đó:

```sql
SELECT *
FROM Employee
WHERE bonus = 0;
```

Chỉ trả về các bản ghi có giá trị bằng `0`, không bao gồm các bản ghi có giá trị `NULL`.

---

### 5. NULL trong phép toán

```sql
SELECT 100 + NULL;
```

Kết quả:

```text
NULL
```

Bất kỳ phép toán nào với `NULL` thường sẽ cho kết quả là `NULL`.

---

### 6. NULL trong các hàm tổng hợp

Giả sử cột `bonus` có các giá trị:

| bonus |
| ----: |
|   500 |
|  NULL |
|  1000 |

```sql
SELECT
    COUNT(bonus),
    SUM(bonus),
    AVG(bonus)
FROM Employee;
```

Kết quả:

| COUNT(bonus) | SUM(bonus) | AVG(bonus) |
| -----------: | ---------: | ---------: |
|            2 |       1500 |        750 |

Các giá trị `NULL` sẽ được bỏ qua khi tính toán.
