# Design Pattern là gì?

## Khái niệm

Design Pattern (Mẫu thiết kế) là các giải pháp mẫu đã được kiểm chứng để giải quyết những vấn đề phổ biến trong thiết kế phần mềm hướng đối tượng.

Design Pattern không phải là một đoạn mã cụ thể mà là một khuôn mẫu tư duy giúp xây dựng chương trình theo cách dễ hiểu, dễ bảo trì và dễ mở rộng.

---

## Mục đích

Design Pattern được tạo ra nhằm:

* Giải quyết các vấn đề thường gặp trong quá trình thiết kế phần mềm.
* Cung cấp những giải pháp đã được kiểm chứng thay vì phải tự xây dựng lại từ đầu.
* Chuẩn hóa cách thiết kế giữa các lập trình viên.
* Tăng khả năng mở rộng và bảo trì hệ thống.

---

# Các nhóm Design Pattern

Design Pattern được chia thành ba nhóm chính:

* Creational Patterns (Nhóm khởi tạo).
* Structural Patterns (Nhóm cấu trúc).
* Behavioral Patterns (Nhóm hành vi).

---

# Nhóm Khởi tạo (Creational Patterns)

## Khái niệm

Creational Patterns tập trung vào cơ chế tạo đối tượng, giúp việc khởi tạo đối tượng trở nên linh hoạt và tối ưu hơn.

## Các Pattern phổ biến

### Singleton

#### Khái niệm

Singleton đảm bảo một lớp chỉ có duy nhất một thể hiện (instance) và cung cấp một điểm truy cập toàn cục đến đối tượng đó.

#### Mục đích

* Tránh tạo nhiều đối tượng không cần thiết.
* Đảm bảo dữ liệu dùng chung được quản lý tập trung.

---

### Factory Method

#### Khái niệm

Factory Method cung cấp một phương thức để tạo đối tượng mà không cần chỉ định chính xác lớp cụ thể sẽ được tạo ra.

#### Mục đích

* Tách phần tạo đối tượng khỏi phần sử dụng đối tượng.
* Giúp hệ thống dễ mở rộng khi xuất hiện các loại đối tượng mới.

---

# Nhóm Cấu trúc (Structural Patterns)

## Khái niệm

Structural Patterns tập trung vào việc kết hợp các lớp và đối tượng để tạo thành những cấu trúc lớn hơn, linh hoạt hơn.

## Các Pattern phổ biến

### Adapter

#### Khái niệm

Adapter cho phép các interface không tương thích có thể làm việc với nhau.

#### Mục đích

* Tái sử dụng các thành phần đã tồn tại.
* Kết nối các hệ thống có giao diện khác nhau.

---

### Decorator

#### Khái niệm

Decorator cho phép bổ sung hành vi mới vào một đối tượng một cách linh hoạt mà không làm thay đổi cấu trúc của lớp hiện có.

#### Mục đích

* Mở rộng chức năng của đối tượng.
* Hạn chế việc sửa đổi mã nguồn hiện tại.

---

# Nhóm Hành vi (Behavioral Patterns)

## Khái niệm

Behavioral Patterns tập trung vào việc giao tiếp, phân công trách nhiệm và phối hợp hành vi giữa các đối tượng.

## Các Pattern phổ biến

### Observer

#### Khái niệm

Observer định nghĩa mối quan hệ "một - nhiều", trong đó khi trạng thái của một đối tượng thay đổi thì tất cả các đối tượng phụ thuộc sẽ được thông báo tự động.

#### Mục đích

* Đồng bộ dữ liệu giữa nhiều đối tượng.
* Giảm sự phụ thuộc trực tiếp giữa các thành phần.

---

### Strategy

#### Khái niệm

Strategy cho phép định nghĩa nhiều thuật toán khác nhau, đóng gói từng thuật toán thành các lớp riêng biệt và có thể thay thế cho nhau.

#### Mục đích

* Dễ dàng thay đổi thuật toán trong lúc chạy.
* Giảm số lượng câu lệnh điều kiện phức tạp.

---

# Lợi ích khi sử dụng Design Pattern

## Chuẩn hóa

Design Pattern cung cấp một ngôn ngữ chung cho các lập trình viên, giúp việc trao đổi và thảo luận về kiến trúc phần mềm trở nên dễ dàng hơn.

---

## Tái sử dụng

Các Design Pattern là những giải pháp đã được kiểm chứng qua thực tế, giúp giảm thời gian phát triển và hạn chế việc phải xây dựng lại những giải pháp đã tồn tại.

---

## Dễ bảo trì

Mã nguồn được tổ chức rõ ràng, trách nhiệm giữa các thành phần được phân chia hợp lý, từ đó giúp việc sửa lỗi và mở rộng hệ thống trở nên thuận lợi hơn.

---

# Ví dụ

## Singleton

```java
public class Logger {

    private static Logger instance;

    private Logger() {
    }

    public static Logger getInstance() {

        if (instance == null) {
            instance = new Logger();
        }

        return instance;
    }
}
```

---

## Factory Method

```java
interface Vehicle {
    void move();
}

class Car implements Vehicle {

    @Override
    public void move() {
        System.out.println("Car moving");
    }
}

class Bike implements Vehicle {

    @Override
    public void move() {
        System.out.println("Bike moving");
    }
}

class VehicleFactory {

    public static Vehicle createVehicle(String type) {

        if ("car".equals(type)) {
            return new Car();
        }

        if ("bike".equals(type)) {
            return new Bike();
        }

        return null;
    }
}
```

---

## Adapter

```java
interface Printer {
    void print();
}

class OldPrinter {

    public void oldPrint() {
        System.out.println("Printing...");
    }
}

class PrinterAdapter implements Printer {

    private OldPrinter oldPrinter;

    public PrinterAdapter(OldPrinter oldPrinter) {
        this.oldPrinter = oldPrinter;
    }

    @Override
    public void print() {
        oldPrinter.oldPrint();
    }
}
```

---

## Decorator

```java
interface Coffee {
    String getDescription();
}

class SimpleCoffee implements Coffee {

    @Override
    public String getDescription() {
        return "Coffee";
    }
}

class MilkDecorator implements Coffee {

    private Coffee coffee;

    public MilkDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + " + Milk";
    }
}
```

---

## Observer

```java
interface Observer {
    void update();
}

class User implements Observer {

    @Override
    public void update() {
        System.out.println("Received notification");
    }
}
```

---

## Strategy

```java
interface PaymentStrategy {
    void pay(int amount);
}

class CashPayment implements PaymentStrategy {

    @Override
    public void pay(int amount) {
        System.out.println("Pay by cash");
    }
}

class CreditCardPayment implements PaymentStrategy {

    @Override
    public void pay(int amount) {
        System.out.println("Pay by credit card");
    }
}
```

---

# Tổng kết

* Design Pattern là các giải pháp mẫu giúp giải quyết những vấn đề phổ biến trong thiết kế phần mềm hướng đối tượng.
* Design Pattern không phải là đoạn mã cụ thể mà là một khuôn mẫu tư duy đã được kiểm chứng.
* Design Pattern được chia thành ba nhóm:

| Nhóm                | Pattern tiêu biểu         |
| ------------------- | ------------------------- |
| Creational Patterns | Singleton, Factory Method |
| Structural Patterns | Adapter, Decorator        |
| Behavioral Patterns | Observer, Strategy        |

* Singleton đảm bảo chỉ tồn tại một instance.
* Factory Method tách việc tạo đối tượng khỏi phần sử dụng.
* Adapter giúp các interface không tương thích có thể làm việc cùng nhau.
* Decorator cho phép mở rộng chức năng của đối tượng một cách linh hoạt.
* Observer hỗ trợ cơ chế thông báo tự động giữa các đối tượng.
* Strategy cho phép thay đổi thuật toán mà không ảnh hưởng tới phần xử lý chính.
* Việc áp dụng Design Pattern giúp chuẩn hóa thiết kế, tăng khả năng tái sử dụng và giúp hệ thống dễ bảo trì, dễ mở rộng hơn.

Cấu trúc này thường phù hợp hơn để viết tài liệu về Java, Spring, Hibernate hoặc Design Pattern vì phần lý thuyết được trình bày liên tục, còn phần ví dụ được gom lại thành một chương riêng ở cuối.
