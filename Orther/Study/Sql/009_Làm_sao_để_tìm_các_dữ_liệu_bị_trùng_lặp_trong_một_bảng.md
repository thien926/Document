## Làm sao để tìm các dữ liệu bị trùng lặp trong một bảng?

Để tìm các dữ liệu bị trùng lặp, ta thường sử dụng `GROUP BY` kết hợp với `HAVING COUNT(*) > 1`.

* `GROUP BY` dùng để gom các bản ghi có cùng giá trị.
* `COUNT(*)` đếm số lần xuất hiện của mỗi giá trị.
* `HAVING COUNT(*) > 1` chỉ giữ lại những nhóm xuất hiện nhiều hơn một lần.

## Tổng kết

* Sử dụng `GROUP BY` để nhóm dữ liệu theo cột cần kiểm tra.
* Sử dụng `COUNT(*)` để đếm số lần xuất hiện của mỗi giá trị.
* Sử dụng `HAVING COUNT(*) > 1` để lọc ra các giá trị bị trùng lặp.
* Có thể kiểm tra trùng lặp trên một hoặc nhiều cột.

## Ví dụ

Giả sử bảng `Users`:

| user_id | username | email                                   |
| ------: | -------- | --------------------------------------- |
|       1 | An       | [an@gmail.com](mailto:an@gmail.com)     |
|       2 | Bình     | [binh@gmail.com](mailto:binh@gmail.com) |
|       3 | Cường    | [an@gmail.com](mailto:an@gmail.com)     |
|       4 | Dũng     | [dung@gmail.com](mailto:dung@gmail.com) |
|       5 | Hùng     | [binh@gmail.com](mailto:binh@gmail.com) |

### 1. Tìm email bị trùng lặp

```sql
SELECT email, COUNT(*) AS total
FROM Users
GROUP BY email
HAVING COUNT(*) > 1;
```

Kết quả:

| email                                   | total |
| --------------------------------------- | ----: |
| [an@gmail.com](mailto:an@gmail.com)     |     2 |
| [binh@gmail.com](mailto:binh@gmail.com) |     2 |

---

### 2. Tìm username bị trùng lặp

```sql
SELECT username, COUNT(*) AS total
FROM Users
GROUP BY username
HAVING COUNT(*) > 1;
```

Nếu không có username nào trùng, kết quả sẽ rỗng.

---

### 3. Tìm dữ liệu trùng lặp theo nhiều cột

Ví dụ kiểm tra các cặp `(username, email)` bị trùng:

```sql
SELECT username, email, COUNT(*) AS total
FROM Users
GROUP BY username, email
HAVING COUNT(*) > 1;
```

---

### 4. Lấy toàn bộ các bản ghi bị trùng lặp

Nếu muốn xem đầy đủ thông tin của các dòng có email bị trùng:

```sql
SELECT *
FROM Users
WHERE email IN (
    SELECT email
    FROM Users
    GROUP BY email
    HAVING COUNT(*) > 1
);
```

Kết quả:

| user_id | username | email                                   |
| ------: | -------- | --------------------------------------- |
|       1 | An       | [an@gmail.com](mailto:an@gmail.com)     |
|       2 | Bình     | [binh@gmail.com](mailto:binh@gmail.com) |
|       3 | Cường    | [an@gmail.com](mailto:an@gmail.com)     |
|       5 | Hùng     | [binh@gmail.com](mailto:binh@gmail.com) |

## Quy trình hoạt động

```text
Dữ liệu gốc
      ↓
GROUP BY email
      ↓
COUNT(*)
      ↓
HAVING COUNT(*) > 1
      ↓
Các giá trị bị trùng lặp
```

> **Lưu ý:** Nếu một cột được khai báo `PRIMARY KEY` hoặc `UNIQUE`, cơ sở dữ liệu sẽ tự động ngăn chặn việc xuất hiện dữ liệu trùng lặp trên cột đó.
