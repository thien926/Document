## Sự khác nhau giữa DELETE, TRUNCATE và DROP

| Tiêu chí                | DELETE                                         | TRUNCATE                         | DROP                                   |
| ----------------------- | ---------------------------------------------- | -------------------------------- | -------------------------------------- |
| Chức năng               | Xóa các bản ghi trong bảng.                    | Xóa toàn bộ dữ liệu trong bảng.  | Xóa hoàn toàn bảng khỏi cơ sở dữ liệu. |
| Điều kiện `WHERE`       | Có hỗ trợ.                                     | Không hỗ trợ.                    | Không hỗ trợ.                          |
| Dữ liệu                 | Xóa từng dòng được chỉ định hoặc toàn bộ bảng. | Xóa tất cả các dòng.             | Xóa cả dữ liệu và cấu trúc bảng.       |
| Cấu trúc bảng           | Được giữ lại.                                  | Được giữ lại.                    | Bị xóa.                                |
| Tốc độ                  | Chậm hơn.                                      | Nhanh hơn DELETE.                | Nhanh.                                 |
| Ghi log                 | Ghi log cho từng dòng bị xóa.                  | Ghi log tối thiểu.               | Ghi log thao tác xóa bảng.             |
| Identity/AUTO_INCREMENT | Không reset.                                   | Thường reset về giá trị ban đầu. | Không còn tồn tại vì bảng đã bị xóa.   |
| Có thể ROLLBACK         | Có (nếu đang ở trong transaction).             | Tùy hệ quản trị cơ sở dữ liệu.   | Tùy hệ quản trị cơ sở dữ liệu.         |

## Tổng kết

* **DELETE** dùng để xóa một hoặc nhiều bản ghi trong bảng, có thể sử dụng điều kiện `WHERE` để chỉ định dữ liệu cần xóa. Cấu trúc bảng vẫn được giữ nguyên và giá trị `IDENTITY/AUTO_INCREMENT` không bị reset.
* **TRUNCATE** dùng để xóa toàn bộ dữ liệu trong bảng, thực hiện nhanh hơn `DELETE` và thường reset lại bộ đếm `IDENTITY/AUTO_INCREMENT`.
* **DROP** dùng để xóa hoàn toàn bảng, bao gồm cả dữ liệu và cấu trúc bảng. Sau khi thực hiện, bảng sẽ không còn tồn tại trong cơ sở dữ liệu.

## Ví dụ

Giả sử bảng `Employee` có dữ liệu:

| employee_id | employee_name |
| ----------: | ------------- |
|           1 | An            |
|           2 | Bình          |
|           3 | Cường         |

### 1. DELETE

Xóa nhân viên có `employee_id = 2`:

```sql
DELETE FROM Employee
WHERE employee_id = 2;
```

Kết quả:

| employee_id | employee_name |
| ----------: | ------------- |
|           1 | An            |
|           3 | Cường         |

* Có thể xóa một phần dữ liệu bằng `WHERE`.
* Giá trị `IDENTITY/AUTO_INCREMENT` không bị reset.

---

### 2. TRUNCATE

Xóa toàn bộ dữ liệu trong bảng:

```sql
TRUNCATE TABLE Employee;
```

Kết quả:

* Bảng `Employee` vẫn tồn tại.
* Toàn bộ dữ liệu bị xóa.
* Bộ đếm `IDENTITY/AUTO_INCREMENT` thường được đưa về giá trị ban đầu.

---

### 3. DROP

Xóa hoàn toàn bảng:

```sql
DROP TABLE Employee;
```

Kết quả:

* Bảng `Employee` bị xóa khỏi cơ sở dữ liệu.
* Toàn bộ dữ liệu, cấu trúc bảng, chỉ mục (index) và các ràng buộc liên quan của bảng cũng bị xóa.

### Thứ tự mức độ ảnh hưởng

```text
DELETE < TRUNCATE < DROP
```

* **DELETE** → Xóa dữ liệu theo từng dòng.
* **TRUNCATE** → Xóa toàn bộ dữ liệu nhưng giữ lại bảng.
* **DROP** → Xóa cả bảng và dữ liệu.

> **Ghi chú:** Khả năng phục hồi (`ROLLBACK`) của `TRUNCATE` và `DROP` phụ thuộc vào từng hệ quản trị cơ sở dữ liệu (MySQL, SQL Server, Oracle, PostgreSQL, ...). Không phải lúc nào chúng cũng không thể phục hồi như nhiều tài liệu tóm tắt thường nêu.
