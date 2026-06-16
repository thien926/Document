# Tại sao Index giúp tăng tốc truy vấn?

Index giúp tăng tốc truy vấn vì thay vì phải đọc tuần tự toàn bộ bảng (**Full Table Scan**), cơ sở dữ liệu có thể tìm trực tiếp đến vị trí của dữ liệu cần tìm thông qua cấu trúc Index.

Thông thường, Index được cài đặt bằng **B-Tree**, cho phép tìm kiếm dữ liệu theo cơ chế phân nhánh, giúp giảm đáng kể số lượng bản ghi cần đọc.

## So sánh có và không có Index

| Không có Index                            | Có Index                                |
| ----------------------------------------- | --------------------------------------- |
| Phải quét toàn bộ bảng (Full Table Scan). | Tìm kiếm thông qua cấu trúc Index.      |
| Đọc từng dòng từ đầu đến cuối.            | Chỉ duyệt một số ít nút của cây B-Tree. |
| Độ phức tạp gần O(n).                     | Độ phức tạp gần O(log n).               |
| Chậm khi bảng có nhiều dữ liệu.           | Nhanh hơn đáng kể với dữ liệu lớn.      |

## Tổng kết

* **Không có Index**, cơ sở dữ liệu phải thực hiện **Full Table Scan**, đọc từng dòng cho đến khi tìm thấy dữ liệu cần thiết.
* **Có Index**, cơ sở dữ liệu sử dụng cấu trúc **B-Tree** để định vị dữ liệu nhanh hơn.
* Tìm kiếm bằng Index có độ phức tạp xấp xỉ **O(log n)**, trong khi quét toàn bộ bảng có độ phức tạp **O(n)**.
* Index đặc biệt hiệu quả khi bảng chứa lượng dữ liệu lớn.

---

# Ví dụ

Giả sử bảng `Employee` có 1 triệu bản ghi và cần tìm:

```sql
SELECT *
FROM Employee
WHERE email = 'binh@gmail.com';
```

## 1. Không có Index

Database sẽ thực hiện:

```text
Dòng 1
Dòng 2
Dòng 3
...
Dòng 500000
...
Dòng 1000000
```

Quá trình:

```text
Full Table Scan
↓
Đọc từng dòng
↓
So sánh email
↓
Tìm thấy kết quả
```

Độ phức tạp:

```text
O(n)
```

Nếu dữ liệu nằm ở cuối bảng thì gần như phải đọc toàn bộ 1 triệu dòng.

---

## 2. Có Index

Giả sử tạo Index:

```sql
CREATE INDEX idx_email
ON Employee(email);
```

Index dạng B-Tree:

```text
                 M
             /       \
           G           T
         /   \       /   \
        C     K     P     Z
             ...
```

Khi tìm:

```sql
SELECT *
FROM Employee
WHERE email = 'binh@gmail.com';
```

Database sẽ:

```text
Root
↓
Node trung gian
↓
Leaf Node
↓
Con trỏ đến dòng dữ liệu
```

Chỉ cần vài lần so sánh để tìm được vị trí của bản ghi.

Độ phức tạp:

```text
O(log n)
```

---

## Minh họa

### Không có Index

```text
Dữ liệu

Row 1
Row 2
Row 3
...
Row n

↓
Đọc từng dòng
↓
Tìm thấy dữ liệu
```

### Có Index

```text
            Root
             ↓
        Node trung gian
             ↓
          Leaf Node
             ↓
          Row dữ liệu
```

---

## Khi nào Index phát huy hiệu quả nhất?

Index đặc biệt hữu ích với các truy vấn:

```sql
SELECT * FROM Employee WHERE email = ?;
```

```sql
SELECT * FROM Employee
WHERE department_id = 1;
```

```sql
SELECT * FROM Employee
ORDER BY salary;
```

```sql
SELECT *
FROM Employee e
JOIN Department d
ON e.department_id = d.id;
```

vì chúng giúp tránh việc phải quét toàn bộ bảng.

> **Lưu ý:** Index không làm dữ liệu thay đổi nhanh hơn. Thực tế, `INSERT`, `UPDATE`, `DELETE` sẽ chậm hơn một chút vì cơ sở dữ liệu phải cập nhật thêm cấu trúc Index tương ứng.
