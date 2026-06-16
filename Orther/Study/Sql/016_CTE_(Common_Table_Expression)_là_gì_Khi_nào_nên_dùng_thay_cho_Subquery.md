# CTE (Common Table Expression) là gì?

CTE (Common Table Expression) là một tập kết quả tạm thời được đặt tên bằng từ khóa `WITH`, chỉ tồn tại trong phạm vi thực thi của một câu lệnh SQL.

Có thể xem CTE như một **bảng tạm thời ảo** hoặc một **subquery có tên**, giúp chia nhỏ các truy vấn phức tạp thành nhiều bước rõ ràng và dễ đọc hơn.

Cú pháp:

```sql
WITH TenCTE AS (
    SELECT ...
)
SELECT ...
FROM TenCTE;
```

---

# Khi nào nên dùng CTE thay cho Subquery?

## 1. Khi truy vấn phức tạp hoặc nhiều tầng

Nếu có nhiều subquery lồng nhau, câu lệnh SQL sẽ trở nên khó đọc và khó bảo trì. CTE giúp tách từng bước xử lý thành các phần riêng biệt, làm cho logic của chương trình rõ ràng hơn.

---

## 2. Khi cần sử dụng lại kết quả trung gian

Một CTE có thể được tham chiếu nhiều lần trong cùng một câu lệnh SQL, giúp tránh việc phải viết lặp lại cùng một subquery.

Điều này giúp:

* Giảm sự trùng lặp code.
* Dễ sửa đổi và bảo trì.
* Làm cho câu truy vấn ngắn gọn hơn.

---

## 3. Khi cần chia bài toán thành nhiều bước xử lý

Có thể khai báo nhiều CTE liên tiếp, trong đó CTE phía sau sử dụng kết quả của CTE phía trước.

Nhờ đó, truy vấn phức tạp được chia thành các bước nhỏ, dễ hiểu và dễ kiểm tra hơn.

---

## 4. Khi cần truy vấn đệ quy (Recursive Query)

CTE hỗ trợ đệ quy, cho phép xử lý các cấu trúc dữ liệu dạng cây hoặc phân cấp như:

* Cây thư mục.
* Danh mục sản phẩm nhiều cấp.
* Quan hệ quản lý nhân viên.
* Duyệt đồ thị.

Subquery thông thường không hỗ trợ kiểu xử lý này.

---

# Khi nào nên dùng Subquery?

## 1. Khi truy vấn đơn giản

Nếu chỉ cần một truy vấn con ngắn gọn, sử dụng Subquery sẽ đơn giản và trực quan hơn.

---

## 2. Khi sử dụng trong các mệnh đề

Subquery thường được dùng cùng:

* `WHERE`
* `IN`
* `EXISTS`
* `ANY`
* `ALL`

Trong các trường hợp này, Subquery thường ngắn gọn và tự nhiên hơn so với CTE.

---

# Hiệu năng của CTE và Subquery

CTE không nhất thiết nhanh hơn Subquery.

Trên các hệ quản trị cơ sở dữ liệu hiện đại như:

* SQL Server
* MySQL 8+
* PostgreSQL
* Oracle

CTE và Subquery thường được Optimizer chuyển thành execution plan tương tự nhau, nên hiệu năng gần như không khác biệt.

Mục đích chính của CTE là:

* Tăng khả năng đọc hiểu.
* Dễ bảo trì.
* Chia nhỏ logic của truy vấn.

Chứ không phải để cải thiện tốc độ thực thi.

---

# Tổng kết

* CTE là một tập kết quả tạm thời được khai báo bằng từ khóa `WITH`.
* CTE thích hợp cho các truy vấn phức tạp, nhiều bước hoặc cần tái sử dụng kết quả trung gian.
* CTE hỗ trợ truy vấn đệ quy, trong khi Subquery thông thường không hỗ trợ.
* Subquery phù hợp với các truy vấn đơn giản hoặc các điều kiện trong `WHERE`, `IN`, `EXISTS`.
* Về hiệu năng, CTE và Subquery thường tương đương nhau; CTE chủ yếu giúp code dễ đọc và dễ bảo trì hơn.

---

# Ví dụ

### Ví dụ 1: Sử dụng CTE

```sql
WITH DepartmentSalary AS (
    SELECT DepartmentId,
           AVG(Salary) AS AvgSalary
    FROM Employees
    GROUP BY DepartmentId
)
SELECT *
FROM DepartmentSalary
WHERE AvgSalary > 5000;
```

---

### Ví dụ 2: Sử dụng Subquery

```sql
SELECT *
FROM (
    SELECT DepartmentId,
           AVG(Salary) AS AvgSalary
    FROM Employees
    GROUP BY DepartmentId
) AS DepartmentSalary
WHERE AvgSalary > 5000;
```

---

### Ví dụ 3: Nhiều CTE liên tiếp

```sql
WITH EmployeeSalary AS (
    SELECT DepartmentId,
           AVG(Salary) AS AvgSalary
    FROM Employees
    GROUP BY DepartmentId
),
HighSalaryDepartment AS (
    SELECT *
    FROM EmployeeSalary
    WHERE AvgSalary > 5000
)
SELECT *
FROM HighSalaryDepartment;
```

---

### Ví dụ 4: CTE đệ quy

```sql
WITH RECURSIVE CategoryTree AS (
    SELECT Id, ParentId, Name
    FROM Categories
    WHERE ParentId IS NULL

    UNION ALL

    SELECT c.Id, c.ParentId, c.Name
    FROM Categories c
    JOIN CategoryTree ct
        ON c.ParentId = ct.Id
)
SELECT *
FROM CategoryTree;
```

Ví dụ trên dùng để duyệt cấu trúc cây của bảng `Categories`, chẳng hạn như danh mục sản phẩm nhiều cấp hoặc cây thư mục.
