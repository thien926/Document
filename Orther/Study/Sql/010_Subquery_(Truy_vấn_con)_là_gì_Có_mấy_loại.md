# Subquery (Truy vấn con) là gì? Có mấy loại?

**Subquery (Truy vấn con)** là một câu lệnh `SELECT` được lồng bên trong một câu lệnh SQL khác (`SELECT`, `INSERT`, `UPDATE`, `DELETE`). Truy vấn con luôn được đặt trong cặp dấu ngoặc `()` và được thực thi trước, sau đó kết quả của nó sẽ được sử dụng bởi truy vấn chính.

## Phân loại theo kết quả trả về

| Loại            | Kết quả trả về                     | Thường dùng với           |
| --------------- | ---------------------------------- | ------------------------- |
| Scalar Subquery | 1 hàng, 1 cột (1 giá trị duy nhất) | `=`, `<`, `>`, `<=`, `>=` |
| Row Subquery    | 1 hàng, nhiều cột                  | So sánh nhiều cột         |
| Column Subquery | Nhiều hàng, 1 cột                  | `IN`, `ANY`, `ALL`        |
| Table Subquery  | Nhiều hàng, nhiều cột              | `FROM`, bảng tạm          |

---

## Phân loại theo cách thức thực thi

| Loại                    | Đặc điểm                                                                          |
| ----------------------- | --------------------------------------------------------------------------------- |
| Non-Correlated Subquery | Chạy độc lập, chỉ thực hiện một lần.                                              |
| Correlated Subquery     | Phụ thuộc vào truy vấn chính, được thực hiện lại cho mỗi hàng của truy vấn ngoài. |

---

# Tổng kết

### Theo kết quả trả về

* **Scalar Subquery**: Trả về một giá trị duy nhất.
* **Row Subquery**: Trả về một hàng gồm nhiều cột.
* **Column Subquery**: Trả về nhiều hàng nhưng chỉ một cột.
* **Table Subquery**: Trả về một bảng tạm gồm nhiều hàng, nhiều cột.

### Theo cách thức thực thi

* **Non-Correlated Subquery**: Chạy một lần, độc lập với truy vấn chính.
* **Correlated Subquery**: Chạy lặp lại cho từng hàng của truy vấn chính.

---

# Ví dụ

Giả sử bảng `Employee`:

| id | employee_name | department_id | salary |
| -: | ------------- | ------------: | -----: |
|  1 | An            |             1 |   1000 |
|  2 | Bình          |             1 |   1500 |
|  3 | Cường         |             2 |   2000 |
|  4 | Dũng          |             2 |   2500 |

Bảng `Department`:

| id | department_name | city   |
| -: | --------------- | ------ |
|  1 | IT              | Hà Nội |
|  2 | HR              | TP.HCM |

---

## 1. Scalar Subquery

Tìm nhân viên có lương lớn hơn mức lương trung bình:

```sql
SELECT employee_name, salary
FROM Employee
WHERE salary >
(
    SELECT AVG(salary)
    FROM Employee
);
```

Subquery:

```sql
SELECT AVG(salary)
FROM Employee;
```

trả về một giá trị duy nhất:

```text
1750
```

---

## 2. Row Subquery

Tìm nhân viên cùng phòng ban và cùng mức lương với nhân viên "An":

```sql
SELECT employee_name, department_id, salary
FROM Employee
WHERE (department_id, salary) =
(
    SELECT department_id, salary
    FROM Employee
    WHERE employee_name = 'An'
);
```

Subquery trả về một hàng:

```text
(1, 1000)
```

---

## 3. Column Subquery

Tìm nhân viên thuộc các phòng ban ở Hà Nội:

```sql
SELECT employee_name
FROM Employee
WHERE department_id IN
(
    SELECT id
    FROM Department
    WHERE city = 'Hà Nội'
);
```

Subquery trả về:

```text
1
```

Sau đó truy vấn chính trở thành:

```sql
SELECT employee_name
FROM Employee
WHERE department_id IN (1);
```

---

## 4. Table Subquery

Tìm phòng ban có mức lương trung bình cao nhất:

```sql
SELECT MAX(avg_salary)
FROM
(
    SELECT department_id,
           AVG(salary) AS avg_salary
    FROM Employee
    GROUP BY department_id
) AS temp;
```

Subquery tạo ra bảng tạm:

| department_id | avg_salary |
| ------------: | ---------: |
|             1 |       1250 |
|             2 |       2250 |

Sau đó truy vấn ngoài lấy giá trị lớn nhất:

```text
2250
```

---

## 5. Non-Correlated Subquery

Tìm nhân viên có lương lớn hơn lương của nhân viên "An":

```sql
SELECT employee_name, salary
FROM Employee
WHERE salary >
(
    SELECT salary
    FROM Employee
    WHERE employee_name = 'An'
);
```

Subquery:

```sql
SELECT salary
FROM Employee
WHERE employee_name = 'An';
```

chỉ được thực hiện **một lần**.

---

## 6. Correlated Subquery

Tìm các nhân viên có mức lương lớn hơn mức lương trung bình của phòng ban mà họ thuộc về:

```sql
SELECT e.employee_name,
       e.salary,
       e.department_id
FROM Employee e
WHERE e.salary >
(
    SELECT AVG(salary)
    FROM Employee
    WHERE department_id = e.department_id
);
```

Quá trình thực hiện:

```text
Lấy nhân viên An
    ↓
Tính AVG lương phòng ban của An
    ↓
So sánh

Lấy nhân viên Bình
    ↓
Tính AVG lương phòng ban của Bình
    ↓
So sánh

...
```

Subquery sẽ được thực hiện lại cho từng nhân viên.

---

> **Lưu ý:** Trong thực tế, **Scalar Subquery** và **Column Subquery** là hai loại được sử dụng nhiều nhất. Về cách thực thi, **Non-Correlated Subquery** thường có hiệu năng tốt hơn **Correlated Subquery**, vì Correlated Subquery phải chạy lặp lại nhiều lần.
