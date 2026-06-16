## Các hàm tổng hợp (Aggregate Functions) cơ bản

Aggregate Function là các hàm dùng để thực hiện phép tính trên nhiều dòng dữ liệu và trả về một giá trị duy nhất. Chúng thường được sử dụng cùng với `GROUP BY`.

| Hàm       | Chức năng                |
| --------- | ------------------------ |
| `COUNT()` | Đếm số lượng bản ghi.    |
| `SUM()`   | Tính tổng các giá trị.   |
| `AVG()`   | Tính giá trị trung bình. |
| `MAX()`   | Tìm giá trị lớn nhất.    |
| `MIN()`   | Tìm giá trị nhỏ nhất.    |

## Tổng kết

* **COUNT()**: Đếm số lượng bản ghi hoặc số lượng giá trị khác `NULL`.
* **SUM()**: Tính tổng các giá trị số.
* **AVG()**: Tính giá trị trung bình của các giá trị số.
* **MAX()**: Tìm giá trị lớn nhất.
* **MIN()**: Tìm giá trị nhỏ nhất.
* Các hàm tổng hợp thường được sử dụng với `GROUP BY` để tính toán trên từng nhóm dữ liệu.

## Ví dụ

Giả sử bảng `Employee`:

| employee_id | employee_name | department | salary |
| ----------: | ------------- | ---------- | -----: |
|         101 | An            | IT         |   1000 |
|         102 | Bình          | IT         |   1500 |
|         103 | Cường         | HR         |   1200 |
|         104 | Dũng          | HR         |   1800 |
|         105 | Hùng          | HR         |   2000 |

### 1. COUNT() – Đếm số bản ghi

```sql
SELECT COUNT(*) AS total_employee
FROM Employee;
```

Kết quả:

| total_employee |
| -------------: |
|              5 |

---

### 2. SUM() – Tính tổng lương

```sql
SELECT SUM(salary) AS total_salary
FROM Employee;
```

Kết quả:

| total_salary |
| -----------: |
|         7500 |

---

### 3. AVG() – Tính lương trung bình

```sql
SELECT AVG(salary) AS average_salary
FROM Employee;
```

Kết quả:

| average_salary |
| -------------: |
|           1500 |

---

### 4. MAX() – Tìm mức lương cao nhất

```sql
SELECT MAX(salary) AS max_salary
FROM Employee;
```

Kết quả:

| max_salary |
| ---------: |
|       2000 |

---

### 5. MIN() – Tìm mức lương thấp nhất

```sql
SELECT MIN(salary) AS min_salary
FROM Employee;
```

Kết quả:

| min_salary |
| ---------: |
|       1000 |

---

### 6. Kết hợp với GROUP BY

Tính lương trung bình của từng phòng ban:

```sql
SELECT department, AVG(salary) AS average_salary
FROM Employee
GROUP BY department;
```

Kết quả:

| department | average_salary |
| ---------- | -------------: |
| IT         |           1250 |
| HR         |        1666.67 |

Ở đây:

* `GROUP BY department` chia dữ liệu thành các nhóm theo phòng ban.
* `AVG(salary)` tính lương trung bình của từng nhóm.

### Lưu ý

* `COUNT(*)` đếm tất cả các dòng, kể cả các dòng chứa giá trị `NULL`.
* `COUNT(column_name)` chỉ đếm các giá trị khác `NULL`.

Ví dụ:

```sql
SELECT COUNT(*) AS total_rows,
       COUNT(bonus) AS total_bonus
FROM Employee;
```

Nếu cột `bonus` có một số giá trị `NULL`, thì `COUNT(bonus)` sẽ nhỏ hơn `COUNT(*)`.
