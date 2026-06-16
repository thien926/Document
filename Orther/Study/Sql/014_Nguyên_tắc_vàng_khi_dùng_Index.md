# Nguyên tắc vàng khi dùng Index

Index chỉ thực sự hiệu quả khi được tạo đúng vị trí. Việc tạo Index một cách tùy tiện có thể làm hệ thống chậm hơn thay vì nhanh hơn.

## Các nguyên tắc quan trọng

| Nguyên tắc                                                                                      | Giải thích                                                                                               |
| ----------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| Chỉ tạo Index trên các cột thường xuyên xuất hiện trong `WHERE`, `JOIN`, `ORDER BY`, `GROUP BY` | Đây là những thao tác thường cần tìm kiếm hoặc sắp xếp dữ liệu, nên hưởng lợi nhiều nhất từ Index.       |
| Ưu tiên cột có độ chọn lọc (Selectivity) cao                                                    | Cột có nhiều giá trị khác nhau giúp Index hiệu quả hơn.                                                  |
| Không nên tạo Index cho bảng quá nhỏ                                                            | Với số lượng bản ghi ít, Full Table Scan thường nhanh hơn việc sử dụng Index.                            |
| Tránh Index trên các cột thường xuyên thay đổi                                                  | Mỗi lần `UPDATE` sẽ phải cập nhật lại Index, làm giảm hiệu năng ghi.                                     |
| Luôn kiểm tra Execution Plan                                                                    | Không phải lúc nào Database cũng sử dụng Index. Cần dùng `EXPLAIN` hoặc công cụ tương đương để kiểm tra. |

---

# Tổng kết

* Tạo Index cho các cột thường xuyên được tìm kiếm hoặc dùng để liên kết dữ liệu.
* Ưu tiên các cột có nhiều giá trị khác nhau (độ chọn lọc cao).
* Không nên tạo Index cho bảng nhỏ hoặc cột ít giá trị phân biệt.
* Hạn chế Index trên các cột thường xuyên cập nhật.
* Không đoán, hãy sử dụng `EXPLAIN` để xác minh truy vấn có sử dụng Index hay không.

---

# Ví dụ

## 1. Index trên cột thường dùng trong WHERE

```sql
SELECT *
FROM Student
WHERE student_code = 'SV001';
```

Tạo Index:

```sql
CREATE INDEX idx_student_code
ON Student(student_code);
```

Truy vấn sẽ nhanh hơn đáng kể khi bảng có nhiều dữ liệu.

---

## 2. Độ chọn lọc cao và thấp

Giả sử bảng `Student`:

| Cột          | Số giá trị khác nhau |
| ------------ | -------------------- |
| student_code | 1.000.000            |
| email        | 1.000.000            |
| gender       | 2                    |
| status       | 3                    |

### Tốt cho Index

```text
student_code
email
phone
```

Vì:

```text
SV0001
SV0002
SV0003
...
```

Gần như mỗi dòng có một giá trị riêng.

### Không tốt cho Index

```text
gender
```

Chỉ có:

```text
Nam
Nữ
```

Nếu tìm:

```sql
SELECT *
FROM Student
WHERE gender = 'Nam';
```

Database có thể phải đọc một lượng lớn dữ liệu nên Index không mang lại nhiều lợi ích.

---

## 3. Không nên Index bảng nhỏ

Giả sử bảng chỉ có:

```text
500 dòng
```

Truy vấn:

```sql
SELECT *
FROM Department
WHERE id = 10;
```

Database có thể quét toàn bộ bảng rất nhanh.

```text
Full Table Scan
≈ vài mili giây
```

Lúc này chi phí duy trì Index có thể còn lớn hơn lợi ích mà nó mang lại.

---

## 4. Không nên Index cột thường xuyên UPDATE

Ví dụ:

```sql
UPDATE Employee
SET salary = salary + 100
WHERE department_id = 1;
```

Nếu `salary` có Index:

```sql
CREATE INDEX idx_salary
ON Employee(salary);
```

Database phải:

```text
Cập nhật dữ liệu
↓
Cập nhật Index
↓
Sắp xếp lại B-Tree
```

Việc này diễn ra mỗi lần dữ liệu thay đổi.

---

## 5. Dùng EXPLAIN để kiểm tra

MySQL:

```sql
EXPLAIN
SELECT *
FROM Employee
WHERE email = 'an@gmail.com';
```

Kết quả có thể hiển thị:

| type | key       |
| ---- | --------- |
| ref  | idx_email |

Điều này cho thấy:

```text
Query đang sử dụng Index idx_email
```

Nếu cột `key` là:

```text
NULL
```

thì Database đang thực hiện:

```text
Full Table Scan
```

và Index chưa được sử dụng.

---

## Quy trình tối ưu Index

```text
Viết truy vấn
      ↓
Phân tích bằng EXPLAIN
      ↓
Xác định điểm nghẽn
      ↓
Tạo Index phù hợp
      ↓
Kiểm tra lại EXPLAIN
      ↓
Đánh giá hiệu năng
```

> Quy tắc quan trọng nhất: **Đừng tạo Index vì nghĩ rằng nó sẽ nhanh hơn. Hãy tạo Index dựa trên các truy vấn thực tế và xác nhận bằng EXPLAIN/Execution Plan.**
