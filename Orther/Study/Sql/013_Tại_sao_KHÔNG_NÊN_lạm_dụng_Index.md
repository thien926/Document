# Tại sao KHÔNG NÊN lạm dụng Index?

Index không phải là **"càng nhiều càng tốt"**. Mặc dù Index giúp tăng tốc độ đọc dữ liệu, nhưng mỗi Index đều phải trả giá bằng bộ nhớ, hiệu năng ghi và chi phí bảo trì.

## Những nhược điểm của việc tạo quá nhiều Index

| Khía cạnh                             | Giải thích                                                                                                                                                         |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Tốn bộ nhớ / dung lượng đĩa           | Index là một cấu trúc dữ liệu riêng, lưu bản sao có thứ tự của các cột được đánh chỉ mục. Kích thước của Index có thể lớn bằng hoặc thậm chí lớn hơn bảng dữ liệu. |
| Làm chậm thao tác ghi                 | Khi thực hiện `INSERT`, `UPDATE`, `DELETE`, cơ sở dữ liệu phải cập nhật tất cả các Index liên quan. Càng nhiều Index, chi phí cập nhật càng cao.                   |
| Có thể khiến Query Optimizer chọn sai | Nếu tồn tại quá nhiều Index, Query Optimizer có thể chọn một Index không tối ưu, dẫn đến truy vấn chậm hơn.                                                        |
| Tăng chi phí bảo trì                  | Index có thể bị phân mảnh theo thời gian, cần thực hiện Rebuild hoặc Reorganize, gây tiêu tốn tài nguyên hệ thống.                                                 |

---

# Tổng kết

* Index giúp tăng tốc độ đọc dữ liệu nhưng không miễn phí.
* Mỗi Index đều làm tăng dung lượng lưu trữ.
* Quá nhiều Index sẽ làm giảm hiệu năng của `INSERT`, `UPDATE`, `DELETE`.
* Query Optimizer có thể chọn kế hoạch thực thi không tối ưu nếu có quá nhiều Index.
* Index cần được bảo trì định kỳ để tránh phân mảnh.
* Chỉ nên tạo Index cho những cột thường xuyên được sử dụng trong `WHERE`, `JOIN`, `ORDER BY`, `GROUP BY`.

---

# Ví dụ

Giả sử bảng `Employee`:

```text
Employee
--------------------------------
id
name
email
phone
address
department_id
salary
created_date
...
```

Nếu tạo quá nhiều Index:

```sql
CREATE INDEX idx_name ON Employee(name);
CREATE INDEX idx_email ON Employee(email);
CREATE INDEX idx_phone ON Employee(phone);
CREATE INDEX idx_address ON Employee(address);
CREATE INDEX idx_salary ON Employee(salary);
CREATE INDEX idx_created_date ON Employee(created_date);
...
```

Khi thêm một nhân viên mới:

```sql
INSERT INTO Employee
VALUES (...);
```

Database không chỉ ghi dữ liệu vào bảng:

```text
Ghi vào bảng Employee
```

mà còn phải cập nhật:

```text
idx_name
idx_email
idx_phone
idx_address
idx_salary
idx_created_date
...
```

Quá trình thực tế:

```text
INSERT
    ↓
Ghi dữ liệu vào bảng
    ↓
Cập nhật Index 1
    ↓
Cập nhật Index 2
    ↓
Cập nhật Index 3
    ↓
...
```

Do đó, thao tác ghi sẽ chậm hơn đáng kể.

---

## Ví dụ về Query Optimizer

Giả sử có hai Index:

```sql
CREATE INDEX idx_department
ON Employee(department_id);

CREATE INDEX idx_department_salary
ON Employee(department_id, salary);
```

Với truy vấn:

```sql
SELECT *
FROM Employee
WHERE department_id = 1
AND salary > 1000;
```

Index `(department_id, salary)` thường hiệu quả hơn.

Tuy nhiên, nếu tồn tại quá nhiều Index, Query Optimizer có thể chọn:

```text
idx_department
```

thay vì:

```text
idx_department_salary
```

làm cho truy vấn kém hiệu quả hơn.

---

## Khi nào KHÔNG nên tạo Index?

* Bảng có rất ít dữ liệu.
* Cột có ít giá trị khác nhau (ví dụ: `gender`, `status` chỉ có vài giá trị).
* Cột thường xuyên bị cập nhật.
* Cột hiếm khi xuất hiện trong `WHERE`, `JOIN`, `ORDER BY`, `GROUP BY`.
* Tạo nhiều Index có nội dung trùng lặp nhau.

## Quy tắc thực tế

```text
Ít Index
↓
INSERT / UPDATE / DELETE nhanh
SELECT chậm

Nhiều Index
↓
SELECT nhanh
INSERT / UPDATE / DELETE chậm
```

Mục tiêu không phải là tạo **nhiều Index nhất**, mà là tạo **đúng Index cần thiết nhất** để cân bằng giữa hiệu năng đọc và hiệu năng ghi.
