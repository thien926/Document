## Phân biệt UNION và UNION ALL

Cả `UNION` và `UNION ALL` đều được sử dụng để gộp kết quả của nhiều câu lệnh `SELECT` thành một tập kết quả duy nhất.

| Tiêu chí          | UNION                                                   | UNION ALL                                            |
| ----------------- | ------------------------------------------------------- | ---------------------------------------------------- |
| Dữ liệu trùng lặp | Tự động loại bỏ các dòng trùng lặp.                     | Giữ lại tất cả các dòng, kể cả các dòng trùng lặp.   |
| Hiệu năng         | Chậm hơn do phải kiểm tra và loại bỏ dữ liệu trùng lặp. | Nhanh hơn vì không cần xử lý dữ liệu trùng lặp.      |
| Kết quả trả về    | Chỉ chứa các dòng duy nhất.                             | Chứa toàn bộ các dòng từ các câu lệnh `SELECT`.      |
| Sử dụng khi       | Cần loại bỏ dữ liệu trùng lặp.                          | Muốn giữ nguyên toàn bộ dữ liệu và tối ưu hiệu năng. |

## Tổng kết

* `UNION` và `UNION ALL` đều dùng để kết hợp kết quả của nhiều câu lệnh `SELECT`.
* **UNION** tự động loại bỏ các dòng trùng lặp nên thường chậm hơn.
* **UNION ALL** giữ lại tất cả các dòng, kể cả trùng lặp, nên có hiệu năng tốt hơn.
* Nếu không có yêu cầu loại bỏ dữ liệu trùng lặp, nên ưu tiên sử dụng **UNION ALL**.

> **Lưu ý:** Các câu lệnh `SELECT` tham gia `UNION` hoặc `UNION ALL` phải có cùng số lượng cột và các kiểu dữ liệu tương thích.

## Ví dụ

Giả sử có hai bảng:

### Bảng `Employee_2024`

| employee_name |
| ------------- |
| An            |
| Bình          |
| Cường         |

### Bảng `Employee_2025`

| employee_name |
| ------------- |
| Bình          |
| Dũng          |
| Hùng          |

---

### 1. UNION

```sql
SELECT employee_name
FROM Employee_2024

UNION

SELECT employee_name
FROM Employee_2025;
```

Kết quả:

| employee_name |
| ------------- |
| An            |
| Bình          |
| Cường         |
| Dũng          |
| Hùng          |

* Giá trị `"Bình"` xuất hiện ở cả hai bảng nhưng chỉ được trả về một lần.

---

### 2. UNION ALL

```sql
SELECT employee_name
FROM Employee_2024

UNION ALL

SELECT employee_name
FROM Employee_2025;
```

Kết quả:

| employee_name |
| ------------- |
| An            |
| Bình          |
| Cường         |
| Bình          |
| Dũng          |
| Hùng          |

* Giá trị `"Bình"` được giữ lại ở cả hai bảng.

## Minh họa

```text
Employee_2024:      Employee_2025:

An                  Bình
Bình                Dũng
Cường               Hùng
                     Bình
```

### UNION

```text
An
Bình
Cường
Dũng
Hùng
```

### UNION ALL

```text
An
Bình
Cường
Bình
Dũng
Hùng
```

### Khi nào sử dụng?

* **UNION**

  * Cần loại bỏ dữ liệu trùng lặp.
  * Ví dụ: Danh sách khách hàng duy nhất từ nhiều nguồn dữ liệu.

* **UNION ALL**

  * Muốn giữ lại toàn bộ dữ liệu.
  * Dữ liệu đã đảm bảo không trùng lặp hoặc không quan tâm đến sự trùng lặp.
  * Cần tối ưu hiệu năng.

> Trong thực tế, **`UNION ALL` thường được ưu tiên hơn** vì nhanh hơn. Chỉ nên sử dụng **`UNION`** khi thực sự cần loại bỏ các bản ghi trùng lặp.
