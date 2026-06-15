# SOLID

## SOLID là gì?

SOLID là tập hợp **5 nguyên lý thiết kế cơ bản trong lập trình hướng đối tượng (OOP)**.

Mục tiêu của SOLID là giúp phần mềm:

* Dễ đọc.
* Dễ bảo trì.
* Linh hoạt khi thay đổi.
* Dễ mở rộng khi có yêu cầu mới.
* Hạn chế ảnh hưởng đến các phần đã hoạt động ổn định.

SOLID gồm 5 nguyên lý:

1. Single Responsibility Principle (SRP)
2. Open/Closed Principle (OCP)
3. Liskov Substitution Principle (LSP)
4. Interface Segregation Principle (ISP)
5. Dependency Inversion Principle (DIP)

---

# S – Single Responsibility Principle (Nguyên tắc đơn nhiệm)

## Khái niệm

Mỗi lớp (Class) chỉ nên đảm nhận **một trách nhiệm duy nhất**.

Nói cách khác:

> Một class chỉ nên có một lý do để thay đổi.

Nếu một lớp làm quá nhiều việc, lớp đó sẽ:

* Cồng kềnh.
* Khó hiểu.
* Khó bảo trì.
* Khi sửa một chức năng có thể làm hỏng chức năng khác.

---

## Ví dụ vi phạm SRP

```java
public class EmployeeService {

    public void saveEmployee(Employee employee) {
        // lưu xuống database
    }

    public void sendEmail(Employee employee) {
        // gửi email
    }

    public void generateReport() {
        // xuất báo cáo
    }
}
```

Class trên đang thực hiện 3 nhiệm vụ:

* Quản lý dữ liệu nhân viên.
* Gửi email.
* Xuất báo cáo.

Điều này vi phạm SRP.

---

## Áp dụng SRP

### EmployeeService

```java
public class EmployeeService {

    public void saveEmployee(Employee employee) {
        // lưu database
    }
}
```

### EmailService

```java
public class EmailService {

    public void sendEmail(Employee employee) {
        // gửi email
    }
}
```

### ReportService

```java
public class ReportService {

    public void generateReport() {
        // xuất báo cáo
    }
}
```

Mỗi class chỉ chịu trách nhiệm cho một chức năng duy nhất.

---

## Lợi ích

* Dễ bảo trì.
* Dễ mở rộng.
* Giảm ảnh hưởng giữa các chức năng.
* Code rõ ràng hơn.

---

# O – Open/Closed Principle (Nguyên tắc đóng/mở)

## Khái niệm

Phần mềm:

* **Mở để mở rộng (Open for Extension).**
* **Đóng để sửa đổi (Closed for Modification).**

Nghĩa là:

Có thể thêm chức năng mới nhưng không nên sửa code cũ đã hoạt động ổn định.

---

## Ví dụ vi phạm OCP

```java
public class PaymentService {

    public void pay(String type) {

        if (type.equals("CASH")) {
            System.out.println("Thanh toán tiền mặt");
        }
        else if (type.equals("VISA")) {
            System.out.println("Thanh toán Visa");
        }
    }

}
```

Nếu xuất hiện thêm:

* Paypal
* Momo
* VNPay

thì phải sửa lại class này liên tục.

---

## Áp dụng OCP

### Interface

```java
public interface PaymentMethod {

    void pay();
}
```

### CashPayment

```java
public class CashPayment implements PaymentMethod {

    @Override
    public void pay() {
        System.out.println("Thanh toán tiền mặt");
    }
}
```

### VisaPayment

```java
public class VisaPayment implements PaymentMethod {

    @Override
    public void pay() {
        System.out.println("Thanh toán Visa");
    }
}
```

### PaymentService

```java
public class PaymentService {

    public void pay(PaymentMethod paymentMethod) {
        paymentMethod.pay();
    }

}
```

Sử dụng:

```java
PaymentService service = new PaymentService();

service.pay(new CashPayment());
service.pay(new VisaPayment());
```

Nếu cần thêm Paypal:

```java
public class PaypalPayment implements PaymentMethod {

    @Override
    public void pay() {
        System.out.println("Thanh toán Paypal");
    }
}
```

Không cần sửa code cũ.

---

## Lợi ích

* Dễ mở rộng chức năng.
* Hạn chế lỗi phát sinh.
* Không ảnh hưởng đến hệ thống đang chạy ổn định.

---

# L – Liskov Substitution Principle (Nguyên tắc thay thế Liskov)

## Khái niệm

Các đối tượng của lớp con phải có khả năng thay thế hoàn toàn đối tượng của lớp cha mà không làm chương trình bị lỗi.

Nói cách khác:

> Nơi nào dùng được lớp cha thì cũng phải dùng được lớp con.

---

## Ví dụ vi phạm LSP

### Bird

```java
public class Bird {

    public void fly() {

    }

}
```

### Penguin

```java
public class Penguin extends Bird {

    @Override
    public void fly() {
        throw new UnsupportedOperationException();
    }

}
```

Chim cánh cụt không biết bay.

Nếu chương trình gọi:

```java
Bird bird = new Penguin();

bird.fly();
```

Sẽ gây lỗi.

Như vậy Penguin không thể thay thế hoàn toàn Bird.

---

## Thiết kế đúng

### Bird

```java
public class Bird {

}
```

### FlyingBird

```java
public class FlyingBird extends Bird {

    public void fly() {

    }

}
```

### Eagle

```java
public class Eagle extends FlyingBird {

}
```

### Penguin

```java
public class Penguin extends Bird {

}
```

Khi đó:

* Eagle có thể bay.
* Penguin không có phương thức fly().
* Không xảy ra lỗi khi thay thế.

---

## Lợi ích

* Kế thừa đúng bản chất.
* Tránh lỗi Runtime.
* Giúp hệ thống ổn định hơn.

---

# I – Interface Segregation Principle (Nguyên tắc phân tách giao diện)

## Khái niệm

Không nên tạo một Interface quá lớn.

Nên chia thành nhiều Interface nhỏ với mục đích cụ thể.

Các lớp chỉ cần triển khai những phương thức mà chúng thực sự sử dụng.

---

## Ví dụ vi phạm ISP

```java
public interface Worker {

    void work();

    void eat();

    void sleep();
}
```

Robot:

```java
public class Robot implements Worker {

    @Override
    public void work() {

    }

    @Override
    public void eat() {
        throw new UnsupportedOperationException();
    }

    @Override
    public void sleep() {
        throw new UnsupportedOperationException();
    }
}
```

Robot không ăn và không ngủ nhưng vẫn bị ép phải cài đặt.

---

## Áp dụng ISP

### Workable

```java
public interface Workable {

    void work();
}
```

### Eatable

```java
public interface Eatable {

    void eat();
}
```

### Sleepable

```java
public interface Sleepable {

    void sleep();
}
```

### Human

```java
public class Human
        implements Workable, Eatable, Sleepable {

    @Override
    public void work() {

    }

    @Override
    public void eat() {

    }

    @Override
    public void sleep() {

    }
}
```

### Robot

```java
public class Robot implements Workable {

    @Override
    public void work() {

    }
}
```

Robot chỉ cần triển khai:

```java
work()
```

mà không cần cài đặt các phương thức dư thừa.

---

## Lợi ích

* Interface gọn gàng.
* Giảm code thừa.
* Tăng khả năng tái sử dụng.
* Dễ bảo trì.

---

# D – Dependency Inversion Principle (Nguyên tắc đảo ngược phụ thuộc)

## Khái niệm

* Module cấp cao không nên phụ thuộc trực tiếp vào module cấp thấp.
* Cả hai nên phụ thuộc vào abstraction (Interface).
* Chi tiết triển khai phải phụ thuộc vào abstraction, không phải ngược lại.

---

## Ví dụ vi phạm DIP

```java
public class MySQLDatabase {

    public void save() {
        System.out.println("Save MySQL");
    }
}
```

```java
public class UserService {

    private MySQLDatabase database =
            new MySQLDatabase();

    public void saveUser() {
        database.save();
    }

}
```

UserService phụ thuộc trực tiếp vào MySQLDatabase.

Nếu chuyển sang PostgreSQL thì phải sửa code.

---

## Áp dụng DIP

### Interface

```java
public interface Database {

    void save();
}
```

### MySQLDatabase

```java
public class MySQLDatabase implements Database {

    @Override
    public void save() {
        System.out.println("Save MySQL");
    }
}
```

### PostgreSQLDatabase

```java
public class PostgreSQLDatabase implements Database {

    @Override
    public void save() {
        System.out.println("Save PostgreSQL");
    }
}
```

### UserService

```java
public class UserService {

    private Database database;

    public UserService(Database database) {
        this.database = database;
    }

    public void saveUser() {
        database.save();
    }

}
```

Sử dụng:

```java
Database db = new MySQLDatabase();

UserService service =
        new UserService(db);

service.saveUser();
```

Hoặc:

```java
Database db = new PostgreSQLDatabase();

UserService service =
        new UserService(db);

service.saveUser();
```

Không cần sửa `UserService`.

---

## Lợi ích

* Giảm sự phụ thuộc giữa các module.
* Dễ thay thế implementation.
* Dễ mở rộng.
* Hỗ trợ Dependency Injection trong Spring.

---

# Tổng kết

SOLID là tập hợp **5 nguyên lý thiết kế cơ bản trong lập trình hướng đối tượng (OOP)**.

Mục tiêu của SOLID là giúp phần mềm:

* Dễ đọc.
* Dễ bảo trì.
* Linh hoạt khi thay đổi.
* Dễ mở rộng khi có yêu cầu mới.
* Hạn chế ảnh hưởng đến các phần đã hoạt động ổn định.


| Nguyên lý                               | Ý nghĩa                                                                                                      |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **S – Single Responsibility Principle (Nguyên tắc đơn nhiệm)** | Một class chỉ nên có một trách nhiệm duy nhất.                                                               |
| **O – Open/Closed Principle (Nguyên tắc đóng/mở)**           | Có thể mở rộng chức năng mới nhưng hạn chế sửa code cũ đang ổn định.                                         |
| **L – Liskov Substitution Principle (Nguyên tắc thay thế Liskov)**   | Đối tượng lớp con phải thay thế được lớp cha mà không gây lỗi.                                               |
| **I – Interface Segregation Principle (Nguyên tắc phân tách giao diện)** | Chia Interface lớn thành nhiều Interface nhỏ, tránh ép các lớp triển khai những phương thức không cần thiết. |
| **D – Dependency Inversion Principle (Nguyên tắc đảo ngược phụ thuộc)**  | Các module nên phụ thuộc vào Interface (abstraction) thay vì phụ thuộc trực tiếp vào implementation cụ thể.  |

SOLID là nền tảng quan trọng trong lập trình hướng đối tượng, giúp xây dựng các hệ thống **dễ đọc, dễ bảo trì, linh hoạt và dễ mở rộng**, đặc biệt được áp dụng rất nhiều trong Java, Spring Framework và Spring Boot.
