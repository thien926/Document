## Các kiểu JOIN thường gặp và sự khác biệt giữa chúng

JOIN được sử dụng để kết hợp dữ liệu từ nhiều bảng dựa trên cột liên kết giữa chúng.

| Loại JOIN                       | Kết quả trả về                                                                                                                      |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `INNER JOIN`                    | Chỉ lấy các dòng có dữ liệu khớp ở cả hai bảng.                                                                                     |
| `LEFT JOIN`                     | Lấy toàn bộ dữ liệu của bảng bên trái và các dòng khớp ở bảng bên phải. Nếu không khớp, các cột của bảng phải sẽ có giá trị `NULL`. |
| `RIGHT JOIN`                    | Lấy toàn bộ dữ liệu của bảng bên phải và các dòng khớp ở bảng bên trái. Nếu không khớp, các cột của bảng trái sẽ có giá trị `NULL`. |
| `FULL JOIN` (`FULL OUTER JOIN`) | Lấy tất cả các dòng của cả hai bảng. Nếu không có dữ liệu khớp thì phía còn lại sẽ có giá trị `NULL`.                               |

## Tổng kết

* **INNER JOIN**: Chỉ lấy dữ liệu xuất hiện ở cả hai bảng.
* **LEFT JOIN**: Giữ toàn bộ dữ liệu của bảng bên trái.
* **RIGHT JOIN**: Giữ toàn bộ dữ liệu của bảng bên phải.
* **FULL JOIN**: Giữ toàn bộ dữ liệu của cả hai bảng.
* Các dòng không tìm thấy bản ghi tương ứng sẽ được điền giá trị `NULL`.

## Ví dụ

Giả sử có hai bảng:

### Bảng `Department`

| department_id | department_name |
| ------------: | --------------- |
|             1 | IT              |
|             2 | HR              |
|             3 | Sales           |

### Bảng `Employee`

| employee_id | employee_name | department_id |
| ----------: | ------------- | ------------: |
|         101 | An            |             1 |
|         102 | Bình          |             2 |
|         103 | Cường         |             4 |

---

### 1. INNER JOIN

Chỉ lấy các bản ghi có `department_id` tồn tại ở cả hai bảng.

```sql
SELECT e.employee_name, d.department_name
FROM Employee e
INNER JOIN Department d
ON e.department_id = d.department_id;
```

Kết quả:

| employee_name | department_name |
| ------------- | --------------- |
| An            | IT              |
| Bình          | HR              |

* Nhân viên Cường có `department_id = 4` không tồn tại trong bảng `Department`, nên không được trả về.
* Phòng ban Sales không có nhân viên nên cũng không xuất hiện.

---

### 2. LEFT JOIN

Lấy toàn bộ dữ liệu của bảng bên trái (`Employee`).

```sql
SELECT e.employee_name, d.department_name
FROM Employee e
LEFT JOIN Department d
ON e.department_id = d.department_id;
```

Kết quả:

| employee_name | department_name |
| ------------- | --------------- |
| An            | IT              |
| Bình          | HR              |
| Cường         | NULL            |

* Tất cả nhân viên đều được giữ lại.
* Cường không có phòng ban tương ứng nên giá trị `department_name` là `NULL`.

---

### 3. RIGHT JOIN

Lấy toàn bộ dữ liệu của bảng bên phải (`Department`).

```sql
SELECT e.employee_name, d.department_name
FROM Employee e
RIGHT JOIN Department d
ON e.department_id = d.department_id;
```

Kết quả:

| employee_name | department_name |
| ------------- | --------------- |
| An            | IT              |
| Bình          | HR              |
| NULL          | Sales           |

* Tất cả các phòng ban đều được giữ lại.
* Phòng ban Sales chưa có nhân viên nên `employee_name` có giá trị `NULL`.

---

### 4. FULL JOIN

Lấy toàn bộ dữ liệu của cả hai bảng.

```sql
SELECT e.employee_name, d.department_name
FROM Employee e
FULL OUTER JOIN Department d
ON e.department_id = d.department_id;
```

Kết quả:

| employee_name | department_name |
| ------------- | --------------- |
| An            | IT              |
| Bình          | HR              |
| Cường         | NULL            |
| NULL          | Sales           |

* Giữ lại tất cả nhân viên và tất cả phòng ban.
* Những bản ghi không khớp sẽ được điền giá trị `NULL`.

## Minh họa bằng tập hợp

```text
INNER JOIN
    A ∩ B

LEFT JOIN
    A

RIGHT JOIN
        B

FULL JOIN
    A ∪ B
```

Trong đó:

* **A**: Bảng bên trái.
* **B**: Bảng bên phải.

```
INNER JOIN ⊂ LEFT JOIN ⊂ FULL JOIN
INNER JOIN ⊂ RIGHT JOIN ⊂ FULL JOIN
```

> Trong thực tế, `INNER JOIN` và `LEFT JOIN` là hai loại JOIN được sử dụng nhiều nhất. `RIGHT JOIN` thường có thể thay thế bằng `LEFT JOIN` bằng cách đảo vị trí hai bảng.
