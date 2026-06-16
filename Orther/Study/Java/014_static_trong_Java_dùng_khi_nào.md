# static trong Java dùng khi nào?

## 1. Khái niệm

`static` là từ khóa dùng để khai báo các thành phần **thuộc về class thay vì thuộc về từng object**.

Điều này có nghĩa là các thành phần `static` được tạo ra một lần khi class được nạp vào bộ nhớ và được chia sẻ bởi tất cả các đối tượng của class đó.

---

## 2. Biến `static`

### Chức năng

Biến `static` là biến dùng chung cho tất cả các instance của class.

### Đặc điểm

* Chỉ tồn tại một bản sao duy nhất trong bộ nhớ.
* Mọi object đều sử dụng chung giá trị của biến này.
* Thay đổi giá trị từ một object sẽ ảnh hưởng đến tất cả các object khác.

### Khi nào nên sử dụng?

Biến `static` phù hợp khi:

* Dữ liệu cần được chia sẻ giữa tất cả các object.
* Giá trị là thuộc tính chung của class thay vì của từng đối tượng.
* Khai báo hằng số hoặc bộ đếm số lượng object.

---

## 3. Method `static`

### Chức năng

Method `static` có thể được gọi trực tiếp thông qua tên class mà không cần tạo object.

### Đặc điểm

* Không cần khởi tạo đối tượng để gọi.
* Có thể truy cập trực tiếp các biến và method `static` khác.
* Không thể sử dụng từ khóa `this`.
* Không thể truy cập trực tiếp các thành phần non-static.

### Khi nào nên sử dụng?

Method `static` phù hợp với:

* Các utility method.
* Các hàm xử lý độc lập với trạng thái của object.
* Các phương thức hỗ trợ (helper method).

---

## 4. Các trường hợp sử dụng phổ biến

### Hằng số

Các hằng số thường được khai báo bằng `static final`.

#### Lợi ích

* Chỉ có một bản sao trong bộ nhớ.
* Có thể truy cập trực tiếp thông qua tên class.
* Giá trị không bị thay đổi.

---

### Utility Method

Các hàm tiện ích không phụ thuộc vào trạng thái của object thường được khai báo là `static`.

#### Lợi ích

* Không cần tạo object.
* Dễ dàng tái sử dụng.
* Tiết kiệm bộ nhớ và chi phí khởi tạo đối tượng.

---

### Dữ liệu dùng chung

Khi tất cả các object cần chia sẻ cùng một giá trị, biến `static` là lựa chọn phù hợp.

Ví dụ:

* Bộ đếm số lượng object.
* Cấu hình dùng chung.
* Thông tin phiên bản chương trình.

---

## 5. So sánh thành phần static và non-static

| Tiêu chí                     | Static                        | Non-static                    |
| ---------------------------- | ----------------------------- | ----------------------------- |
| Thuộc về                     | Class                         | Object                        |
| Số lượng bản sao             | Một                           | Mỗi object một bản sao        |
| Cần tạo object để sử dụng    | Không                         | Có                            |
| Được chia sẻ giữa các object | Có                            | Không                         |
| Có thể dùng `this`           | Không                         | Có                            |
| Phù hợp với                  | Dữ liệu chung, utility method | Dữ liệu riêng của từng object |

---

## Ví dụ

### Ví dụ về biến `static`

```java
class Student {

    static int count = 0;

    Student() {
        count++;
    }
}
```

```java
Student s1 = new Student();
Student s2 = new Student();
Student s3 = new Student();

System.out.println(Student.count);
```

Kết quả:

```text
3
```

Biến `count` được chia sẻ giữa tất cả các object và dùng để đếm số lượng đối tượng được tạo ra.

---

### Ví dụ về method `static`

```java
class MathUtil {

    static int square(int n) {
        return n * n;
    }
}
```

Gọi method:

```java
int result = MathUtil.square(5);

System.out.println(result);
```

Kết quả:

```text
25
```

Method `square()` được gọi trực tiếp thông qua tên class mà không cần tạo object.

---

### Ví dụ về hằng số `static final`

```java
class Config {

    static final double PI = 3.1415926535;
}
```

Sử dụng:

```java
System.out.println(Config.PI);
```

Kết quả:

```text
3.1415926535
```

---

# Tổng kết

* `static` dùng cho các thành phần thuộc về class thay vì thuộc về từng object.
* Biến `static` được chia sẻ giữa tất cả các instance và chỉ tồn tại một bản sao trong bộ nhớ.
* Method `static` có thể được gọi trực tiếp thông qua tên class mà không cần tạo object.
* Thành phần `static` không thể sử dụng từ khóa `this` và không thể truy cập trực tiếp các thành phần non-static.
* `static` thường được sử dụng cho:

  * Hằng số (`static final`).
  * Utility method.
  * Dữ liệu thực sự dùng chung giữa các object.
* Chỉ nên sử dụng `static` khi dữ liệu hoặc hành vi thuộc về class, không phụ thuộc vào trạng thái của từng đối tượng.
