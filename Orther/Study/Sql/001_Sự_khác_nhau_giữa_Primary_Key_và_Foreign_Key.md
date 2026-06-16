### Sự khác nhau giữa Primary Key và Foreign Key

| Tiêu chí                | Primary Key                                                           | Foreign Key                                                                                                     |
| ----------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Khái niệm               | Một hoặc nhiều cột dùng để định danh duy nhất mỗi bản ghi trong bảng. | Một hoặc nhiều cột tham chiếu đến Primary Key (hoặc Unique Key) của bảng khác để tạo mối quan hệ giữa các bảng. |
| Giá trị trùng lặp       | Không được phép trùng lặp.                                            | Có thể trùng lặp.                                                                                               |
| Giá trị NULL            | Không được phép chứa giá trị `NULL`.                                  | Có thể chứa `NULL` (nếu không có ràng buộc `NOT NULL`).                                                         |
| Số lượng trong một bảng | Mỗi bảng chỉ có một Primary Key (có thể gồm nhiều cột).               | Một bảng có thể có nhiều Foreign Key.                                                                           |
| Mục đích                | Xác định duy nhất từng bản ghi trong bảng.                            | Liên kết dữ liệu giữa các bảng và đảm bảo tính toàn vẹn tham chiếu.                                             |

### Tổng kết

* **Primary Key** dùng để định danh duy nhất mỗi dòng dữ liệu trong một bảng và không được chứa giá trị trùng lặp hoặc `NULL`.
* **Foreign Key** dùng để tham chiếu đến dữ liệu ở bảng khác, tạo mối quan hệ giữa các bảng và đảm bảo tính nhất quán của dữ liệu.
* Một bảng chỉ có **một Primary Key**, nhưng có thể có **nhiều Foreign Key**.

### Ví dụ

Bảng `Department`:

| department_id (PK) | department_name |
| ------------------ | --------------- |
| 1                  | IT              |
| 2                  | HR              |

Bảng `Employee`:

| employee_id (PK) | employee_name | department_id (FK) |
| ---------------- | ------------- | ------------------ |
| 101              | An            | 1                  |
| 102              | Bình          | 1                  |
| 103              | Cường         | 2                  |

Trong đó:

* `employee_id` là **Primary Key** của bảng `Employee`.
* `department_id` là **Primary Key** của bảng `Department`.
* `department_id` trong bảng `Employee` là **Foreign Key**, tham chiếu đến `department_id` của bảng `Department`.

```sql
CREATE TABLE Department (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(50)
);

CREATE TABLE Employee (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50),
    department_id INT,
    FOREIGN KEY (department_id)
        REFERENCES Department(department_id)
);
```

Nhờ `Foreign Key`, mỗi nhân viên sẽ được liên kết với một phòng ban hợp lệ trong bảng `Department`.
