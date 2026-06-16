## Sự khác nhau giữa WHERE và HAVING

| Tiêu chí                                                   | WHERE                                       | HAVING                                                 |
| ---------------------------------------------------------- | ------------------------------------------- | ------------------------------------------------------ |
| Thời điểm thực hiện                                        | Lọc dữ liệu trước khi thực hiện `GROUP BY`. | Lọc dữ liệu sau khi đã thực hiện `GROUP BY`.           |
| Đối tượng áp dụng                                          | Các dòng dữ liệu riêng lẻ.                  | Các nhóm dữ liệu sau khi được gom nhóm.                |
| Sử dụng hàm tổng hợp (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`) | Không sử dụng trực tiếp các hàm tổng hợp.   | Có thể sử dụng các hàm tổng hợp.                       |
| Thường đi kèm với                                          | `SELECT`, `UPDATE`, `DELETE`.               | `GROUP BY`.                                            |
| Hiệu năng                                                  | Tốt hơn vì dữ liệu được lọc sớm.            | Chậm hơn do phải thực hiện gom nhóm trước rồi mới lọc. |

## Tổng kết

* **WHERE** dùng để lọc các bản ghi trước khi thực hiện việc gom nhóm dữ liệu.
* **HAVING** dùng để lọc các nhóm dữ liệu sau khi đã thực hiện `GROUP BY`.
* **WHERE** không dùng trực tiếp với các hàm tổng hợp như `COUNT()`, `SUM()`, `AVG()`,...
* **HAVING** thường được sử dụng cùng `GROUP BY` để lọc kết quả của các hàm tổng hợp.
* Trong một câu lệnh SQL có thể sử dụng đồng thời cả `WHERE` và `HAVING`.

## Ví dụ

Bảng `Employee`:

| employee_id | department | salary |
| ----------- | ---------- | -----: |
| 101         | IT         |   1000 |
| 102         | IT         |   1500 |
| 103         | HR         |   1200 |
| 104         | HR         |   1800 |
| 105         | HR         |   2000 |

### 1. Sử dụng WHERE

Lấy những nhân viên có lương lớn hơn 1200:

```sql
SELECT *
FROM Employee
WHERE salary > 1200;
```

`WHERE` sẽ lọc từng dòng dữ liệu trước khi thực hiện bất kỳ thao tác gom nhóm nào.

### 2. Sử dụng HAVING

Lấy các phòng ban có số lượng nhân viên lớn hơn 2:

```sql
SELECT department, COUNT(*) AS total_employee
FROM Employee
GROUP BY department
HAVING COUNT(*) > 2;
```

Kết quả:

| department | total_employee |
| ---------- | -------------: |
| HR         |              3 |

Ở đây:

1. `GROUP BY department` tạo ra hai nhóm: `IT` và `HR`.
2. `COUNT(*)` đếm số nhân viên của từng nhóm.
3. `HAVING COUNT(*) > 2` chỉ giữ lại nhóm `HR`.

### 3. Kết hợp WHERE và HAVING

Tìm các phòng ban có nhiều hơn 1 nhân viên với mức lương từ 1200 trở lên:

```sql
SELECT department, COUNT(*) AS total_employee
FROM Employee
WHERE salary >= 1200
GROUP BY department
HAVING COUNT(*) > 1;
```

Thứ tự thực hiện:

1. `WHERE salary >= 1200` → lọc các nhân viên có lương từ 1200 trở lên.
2. `GROUP BY department` → gom nhóm theo phòng ban.
3. `HAVING COUNT(*) > 1` → chỉ giữ lại các phòng ban có nhiều hơn 1 nhân viên.

> **Thứ tự xử lý:** `FROM` → `WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `ORDER BY`.
