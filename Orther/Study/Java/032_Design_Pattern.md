# Tổng quan các Design Pattern

Design Pattern là những giải pháp thiết kế nhằm giải quyết các vấn đề thường gặp trong quá trình phát triển phần mềm.

Design Pattern không phải là một đoạn code cố định mà là một cách tiếp cận giải quyết vấn đề.

Có 3 nhóm chính:

| Nhóm       | Mục đích                      | Ví dụ                               |
| ---------- | ----------------------------- | ----------------------------------- |
| Creational | Khởi tạo đối tượng            | Singleton, Factory, Builder         |
| Structural | Tổ chức cấu trúc class/object | Adapter, Decorator, Proxy           |
| Behavioral | Tổ chức giao tiếp và hành vi  | Strategy, Observer, Template Method |

----
----

| Nhóm       | Design Pattern  | Mục đích                                                                          |
| ---------- | --------------- | --------------------------------------------------------------------------------- |
| Creational | Singleton       | Đảm bảo một instance duy nhất                                                     |
| Creational | Factory         | Tách biệt logic khởi tạo đối tượng                                                |
| Creational | Builder         | Xây dựng đối tượng phức tạp từng bước                                             |
| Structural | Adapter         | Chuyển đổi interface để các thành phần không tương thích có thể làm việc với nhau |
| Structural | Decorator       | Bổ sung chức năng cho đối tượng mà không cần sửa class gốc                        |
| Structural | Proxy           | Cung cấp đối tượng đại diện để kiểm soát truy cập đến đối tượng thật              |
| Behavioral | Strategy        | Cho phép thay đổi thuật toán mà không thay đổi code sử dụng                       |
| Behavioral | Observer        | Thông báo cho các đối tượng khi trạng thái của đối tượng khác thay đổi            |
| Behavioral | Template Method | Định nghĩa khung thuật toán và cho phép subclass triển khai một số bước cụ thể    |

# I. Creational Patterns – Nhóm khởi tạo đối tượng

Creational Patterns tập trung vào việc quản lý quá trình tạo đối tượng, giúp giảm sự phụ thuộc giữa Client và các class cụ thể.

## 1. Singleton Pattern

Mục đích: Đảm bảo một class chỉ có một instance duy nhất và cung cấp điểm truy cập toàn cục đến instance đó.

Ví dụ thực tế:

Hệ thống có một `ConfigurationManager` chịu trách nhiệm quản lý cấu hình ứng dụng.

Thay vì mỗi Service tự tạo một `ConfigurationManager`, toàn bộ ứng dụng sử dụng chung một instance.

```
public class ConfigurationManager {

    private static final ConfigurationManager INSTANCE =
            new ConfigurationManager();

    private ConfigurationManager() {
    }

    public static ConfigurationManager getInstance() {
        return INSTANCE;
    }

    public String getApplicationName() {
        return "My Application";
    }
}
```

Sử dụng:

```
ConfigurationManager config1 =
        ConfigurationManager.getInstance();

ConfigurationManager config2 =
        ConfigurationManager.getInstance();

System.out.println(config1 == config2); // true
```

Ưu điểm:

- Đảm bảo một instance duy nhất trong phạm vi triển khai.
- Dễ dàng chia sẻ tài nguyên.
- Kiểm soát việc khởi tạo đối tượng.

Nhược điểm:

- Tạo sự phụ thuộc toàn cục.
- Khó Unit Test nếu phụ thuộc trực tiếp vào Singleton.
- Cần chú ý Thread-safe khi triển khai Lazy Initialization.

Ứng dụng: Configuration Manager, Logger, Cache Manager.

## 2. Factory Pattern

Mục đích: Tách biệt logic khởi tạo đối tượng khỏi nơi sử dụng đối tượng.

Client không cần biết chi tiết cách tạo object mà chỉ cần yêu cầu Factory cung cấp object phù hợp.

Ví dụ thực tế:

Hệ thống thanh toán hỗ trợ nhiều phương thức như Credit Card, PayPal và Momo.

Thay vì để `PaymentService` tự khởi tạo từng loại thanh toán, chúng ta sử dụng `PaymentFactory`.

```
public interface Payment {
    void pay();
}
```

Các implementation:

```
public class MomoPayment implements Payment {
    public void pay() {
        System.out.println("Pay by Momo");
    }
}

public class PayPalPayment implements Payment {
    public void pay() {
        System.out.println("Pay by PayPal");
    }
}
```

Factory:

```
public class PaymentFactory {

    public static Payment createPayment(String type) {

        switch (type.toUpperCase()) {
            case "MOMO":
                return new MomoPayment();

            case "PAYPAL":
                return new PayPalPayment();

            default:
                throw new IllegalArgumentException(
                        "Unsupported payment type"
                );
        }
    }
}
```

Sử dụng:

```
Payment payment = PaymentFactory.createPayment("MOMO");

payment.pay();
```

Ưu điểm:

- Tách biệt logic tạo object khỏi logic nghiệp vụ.
- Giảm sự phụ thuộc vào implementation cụ thể.
- Tập trung logic khởi tạo tại một nơi.

Nhược điểm:

- Factory có thể trở nên phức tạp khi có nhiều loại object.
- Simple Factory thường phải sửa Factory khi thêm loại object mới.

Ứng dụng: Payment Gateway, Notification Service, File Parser.

Lưu ý: Ví dụ trên là Simple Factory. Factory Method và Abstract Factory là hai Pattern chính thức khác trong nhóm Creational.

## 3. Builder Pattern

Mục đích: Xây dựng một đối tượng phức tạp thông qua từng bước, thay vì phải truyền quá nhiều tham số vào Constructor.

Ví dụ thực tế:

Hệ thống cần tạo một đối tượng `User` có nhiều thuộc tính như username, email, age, address và phone.

Nếu sử dụng Constructor thông thường:

```
User user = new User(
    "thien",
    "thien@gmail.com",
    26,
    "Ho Chi Minh",
    "0901234567"
);
```

Cách này có thể gây khó khăn khi class có nhiều thuộc tính hoặc nhiều tham số tùy chọn.

Sử dụng Builder:

```
public class User {

    private String username;
    private String email;
    private int age;
    private String address;

    private User(Builder builder) {
        this.username = builder.username;
        this.email = builder.email;
        this.age = builder.age;
        this.address = builder.address;
    }

    public static class Builder {

        private String username;
        private String email;
        private int age;
        private String address;

        public Builder username(String username) {
            this.username = username;
            return this;
        }

        public Builder email(String email) {
            this.email = email;
            return this;
        }

        public Builder age(int age) {
            this.age = age;
            return this;
        }

        public Builder address(String address) {
            this.address = address;
            return this;
        }

        public User build() {
            return new User(this);
        }
    }
}
```

Sử dụng:

```
User user = new User.Builder()
        .username("thien")
        .email("thien@gmail.com")
        .age(26)
        .address("Ho Chi Minh")
        .build();
```

Ưu điểm:

- Code khởi tạo dễ đọc.
- Không cần truyền quá nhiều tham số vào Constructor.
- Dễ quản lý các thuộc tính tùy chọn.
- Có thể kiểm tra dữ liệu trước khi tạo object.

Nhược điểm:

- Phải viết thêm Builder class.
- Có thể không cần thiết với object có ít thuộc tính.

Ứng dụng: DTO, Request Object, Configuration Object, Entity có nhiều thuộc tính tùy chọn.

# II. Structural Patterns – Nhóm cấu trúc

Structural Patterns tập trung vào cách tổ chức class và object để tạo thành cấu trúc linh hoạt, dễ mở rộng và bảo trì.

## 4. Adapter Pattern

Mục đích: Chuyển đổi interface của một class thành interface mà client mong muốn, giúp hai thành phần không tương thích có thể làm việc với nhau.

Ví dụ thực tế:

Hệ thống của chúng ta đang sử dụng một interface thanh toán nội bộ:

```
public interface Payment {
    void pay(double amount);
}
```

Nhưng thư viện thanh toán bên thứ ba lại cung cấp interface khác:

```
public class ThirdPartyPayment {

    public void makePayment(double money) {
        System.out.println("Paid: " + money);
    }
}
```

Hai interface không tương thích với nhau.

Chúng ta tạo Adapter để chuyển đổi:

```
public class PaymentAdapter implements Payment {

    private final ThirdPartyPayment thirdPartyPayment;

    public PaymentAdapter(ThirdPartyPayment thirdPartyPayment) {
        this.thirdPartyPayment = thirdPartyPayment;
    }

    @Override
    public void pay(double amount) {
        thirdPartyPayment.makePayment(amount);
    }
}
```

Sử dụng:

```
ThirdPartyPayment thirdParty = new ThirdPartyPayment();

Payment payment = new PaymentAdapter(thirdParty);

payment.pay(1000);
```

Ưu điểm:

- Tái sử dụng được code hoặc thư viện hiện có.
- Không cần sửa đổi class bên thứ ba.
- Giảm sự phụ thuộc giữa các interface không tương thích.

Nhược điểm:

- Tăng số lượng class trong hệ thống.
- Có thể làm cấu trúc chương trình phức tạp hơn nếu lạm dụng.

Ứng dụng: Tích hợp API bên thứ ba, tích hợp Payment Gateway, chuyển đổi dữ liệu giữa các hệ thống.

## 5. Decorator Pattern

Mục đích: Bổ sung chức năng mới cho một đối tượng mà không cần sửa đổi class gốc hoặc tạo nhiều subclass.

Ví dụ thực tế:

Hệ thống gửi thông báo hỗ trợ Email Notification.

Sau đó cần mở rộng thêm chức năng:

- Gửi Email.
- Gửi Email kèm SMS.
- Gửi Email kèm Logging.

Thay vì tạo nhiều subclass khác nhau, chúng ta sử dụng Decorator.

Interface:

```
public interface Notification {
    void send(String message);
}
```

Implementation gốc:

```
public class EmailNotification implements Notification {

    @Override
    public void send(String message) {
        System.out.println("Send Email: " + message);
    }
}
```

Decorator:

```
public class NotificationDecorator implements Notification {

    protected final Notification notification;

    public NotificationDecorator(Notification notification) {
        this.notification = notification;
    }

    @Override
    public void send(String message) {
        notification.send(message);
    }
}
```

SMS Decorator:

```
public class SMSDecorator extends NotificationDecorator {

    public SMSDecorator(Notification notification) {
        super(notification);
    }

    @Override
    public void send(String message) {

        super.send(message);

        System.out.println("Send SMS: " + message);
    }
}
```

Sử dụng:

```
Notification notification =
        new SMSDecorator(new EmailNotification());

notification.send("Hello");
```

Kết quả:

```
Send Email: Hello
Send SMS: Hello
```

Ưu điểm:

- Mở rộng chức năng mà không sửa class gốc.
- Có thể kết hợp nhiều Decorator.
- Linh hoạt hơn việc tạo quá nhiều subclass.

Nhược điểm:

- Có thể tạo ra nhiều object lồng nhau.
- Khó theo dõi khi có nhiều lớp Decorator.

Ứng dụng: Java I/O Streams, Logging, Authentication, Notification.

## 6. Proxy Pattern

Mục đích: Cung cấp một đối tượng đại diện để kiểm soát việc truy cập đến đối tượng thực tế.

Proxy có thể thực hiện các thao tác trước hoặc sau khi gọi đối tượng thật.

Ví dụ thực tế:

Một hệ thống có `UserService` dùng để lấy thông tin người dùng.

Trước khi gọi service thật, chúng ta muốn kiểm tra quyền truy cập.

Interface:

```
public interface UserService {
    void getUser();
}
```

Real Subject:

```
public class RealUserService implements UserService {

    @Override
    public void getUser() {
        System.out.println("Get user information");
    }
}
```

Proxy:

```
public class UserServiceProxy implements UserService {

    private final UserService userService;

    public UserServiceProxy(UserService userService) {
        this.userService = userService;
    }

    @Override
    public void getUser() {

        System.out.println("Checking permission...");

        userService.getUser();

        System.out.println("Logging completed");
    }
}
```

Sử dụng:

```
UserService service =
        new UserServiceProxy(new RealUserService());

service.getUser();
```

Kết quả:

```
Checking permission...
Get user information
Logging completed
```

Ưu điểm:

- Kiểm soát việc truy cập đối tượng.
- Có thể bổ sung logging, caching, security.
- Không cần sửa đổi class thực tế.

Nhược điểm:

- Tăng độ phức tạp của hệ thống.
- Có thể tăng độ trễ do thêm một lớp trung gian.

Ứng dụng: Spring AOP, Spring Security, Transaction Management, Lazy Loading, Caching.

# III. Behavioral Patterns – Nhóm hành vi

Behavioral Patterns tập trung vào cách các đối tượng giao tiếp, phối hợp và phân chia trách nhiệm trong hệ thống.

## 7. Strategy Pattern

Mục đích: Cho phép định nghĩa nhiều thuật toán khác nhau, đóng gói từng thuật toán thành một class riêng và có thể thay đổi thuật toán tại runtime.

Ví dụ thực tế:

Hệ thống hỗ trợ nhiều phương thức vận chuyển:

- Standard Shipping.
- Express Shipping.
- Same Day Shipping.

Nếu sử dụng nhiều `if-else`, code có thể trở nên phức tạp khi thêm phương thức vận chuyển mới.

Strategy Pattern giúp tách từng thuật toán ra riêng biệt.

Interface:

```
public interface ShippingStrategy {
    double calculateFee(double weight);
}
```

Standard Shipping:

```
public class StandardShipping implements ShippingStrategy {

    @Override
    public double calculateFee(double weight) {
        return weight * 10;
    }
}
```

Express Shipping:

```
public class ExpressShipping implements ShippingStrategy {

    @Override
    public double calculateFee(double weight) {
        return weight * 20;
    }
}
```

Context:

```
public class ShippingService {

    private ShippingStrategy strategy;

    public ShippingService(ShippingStrategy strategy) {
        this.strategy = strategy;
    }

    public void setStrategy(ShippingStrategy strategy) {
        this.strategy = strategy;
    }

    public double calculateFee(double weight) {
        return strategy.calculateFee(weight);
    }
}
```

Sử dụng:

```
ShippingService service =
        new ShippingService(new StandardShipping());

System.out.println(service.calculateFee(5));

service.setStrategy(new ExpressShipping());

System.out.println(service.calculateFee(5));
```

Kết quả:

```
50.0
100.0
```

Ưu điểm:

- Dễ dàng thay đổi thuật toán.
- Hạn chế sử dụng nhiều `if-else`.
- Dễ mở rộng và Unit Test.

Nhược điểm:

- Tăng số lượng class.
- Client hoặc Context cần lựa chọn Strategy phù hợp.

Ứng dụng: Payment Method, Shipping Fee, Discount Calculation, Authentication.

## 8. Observer Pattern

Mục đích: Xây dựng mối quan hệ một-nhiều giữa các đối tượng. Khi một đối tượng thay đổi trạng thái, các đối tượng đăng ký theo dõi sẽ nhận được thông báo.

Ví dụ thực tế:

Khi đơn hàng được thanh toán thành công, hệ thống cần:

- Gửi Email cho khách hàng.
- Gửi thông báo đến ứng dụng.
- Cập nhật trạng thái đơn hàng.

Thay vì `OrderService` phụ thuộc trực tiếp vào tất cả các thành phần, chúng ta sử dụng Observer.

Interface:

```
public interface OrderObserver {
    void onOrderPaid();
}
```

Các Observer:

```
public class EmailObserver implements OrderObserver {

    @Override
    public void onOrderPaid() {
        System.out.println("Send payment confirmation email");
    }
}
```

```
public class NotificationObserver implements OrderObserver {

    @Override
    public void onOrderPaid() {
        System.out.println("Send push notification");
    }
}
```

Subject:

```
import java.util.ArrayList;
import java.util.List;

public class Order {

    private final List<OrderObserver> observers = new ArrayList<>();

    public void addObserver(OrderObserver observer) {
        observers.add(observer);
    }

    public void pay() {

        System.out.println("Order paid successfully");

        notifyObservers();
    }

    private void notifyObservers() {
        for (OrderObserver observer : observers) {
            observer.onOrderPaid();
        }
    }
}
```

Sử dụng:

```
Order order = new Order();

order.addObserver(new EmailObserver());
order.addObserver(new NotificationObserver());

order.pay();
```

Kết quả:

```
Order paid successfully
Send payment confirmation email
Send push notification
```

Ưu điểm:

- Giảm sự phụ thuộc giữa Subject và Observer cụ thể.
- Dễ dàng thêm hoặc loại bỏ Observer.
- Phù hợp với cơ chế thông báo sự kiện.

Nhược điểm:

- Khó kiểm soát thứ tự thực thi nếu có nhiều Observer.
- Có thể phát sinh vấn đề hiệu năng nếu có quá nhiều Observer.
- Cần quản lý việc đăng ký và hủy đăng ký để tránh giữ reference không cần thiết.

Ứng dụng: Event Listener, Notification System, GUI Event, Spring Application Event.

## 9. Template Method Pattern

Mục đích: Định nghĩa bộ khung của một thuật toán trong lớp cha, đồng thời cho phép lớp con triển khai hoặc ghi đè một số bước cụ thể mà không thay đổi cấu trúc tổng thể của thuật toán.

Ví dụ thực tế:

Hệ thống xử lý file hỗ trợ:

- CSV.
- Excel.

Quy trình xử lý chung:

1. Đọc file.
2. Validate dữ liệu.
3. Xử lý dữ liệu.
4. Lưu dữ liệu.

Các bước đọc file và xử lý dữ liệu có thể khác nhau tùy định dạng.

Abstract class:

```
public abstract class FileProcessor {

    // Template Method
    public final void process() {

        readFile();

        validate();

        processData();

        saveData();
    }

    protected abstract void readFile();

    protected void validate() {
        System.out.println("Validate data");
    }

    protected abstract void processData();

    protected void saveData() {
        System.out.println("Save data to database");
    }
}
```

CSV Processor:

```
public class CSVProcessor extends FileProcessor {

    @Override
    protected void readFile() {
        System.out.println("Read CSV file");
    }

    @Override
    protected void processData() {
        System.out.println("Process CSV data");
    }
}
```

Excel Processor:

```
public class ExcelProcessor extends FileProcessor {

    @Override
    protected void readFile() {
        System.out.println("Read Excel file");
    }

    @Override
    protected void processData() {
        System.out.println("Process Excel data");
    }
}
```

Sử dụng:

```
FileProcessor processor = new CSVProcessor();

processor.process();
```

Kết quả:

```
Read CSV file
Validate data
Process CSV data
Save data to database
```

Ưu điểm:

- Tái sử dụng quy trình xử lý chung.
- Đảm bảo các subclass tuân theo cùng một trình tự.
- Giảm code trùng lặp.

Nhược điểm:

- Phụ thuộc vào kế thừa.
- Khó thay đổi thứ tự xử lý vì Template Method thường được cố định ở lớp cha.
- Có thể khó mở rộng nếu quy trình có quá nhiều biến thể.

Ứng dụng: File Processing, Data Import/Export, Authentication Workflow, Spring Framework.

# IV. Bảng so sánh tổng hợp

| Pattern         | Nhóm       | Vấn đề giải quyết                       |
| --------------- | ---------- | --------------------------------------- |
| Singleton       | Creational | Quản lý một instance dùng chung         |
| Factory         | Creational | Tách biệt logic khởi tạo object         |
| Builder         | Creational | Khởi tạo object có nhiều thuộc tính     |
| Adapter         | Structural | Kết nối các interface không tương thích |
| Decorator       | Structural | Bổ sung chức năng cho object            |
| Proxy           | Structural | Kiểm soát truy cập đến object           |
| Strategy        | Behavioral | Thay đổi thuật toán linh hoạt           |
| Observer        | Behavioral | Thông báo khi trạng thái thay đổi       |
| Template Method | Behavioral | Chuẩn hóa quy trình xử lý               |

# V. Tổng kết – Cách phân biệt nhanh khi phỏng vấn

Để dễ nhớ, hãy tập trung vào vấn đề mà từng Pattern giải quyết:

Creational – Tạo đối tượng

- Singleton: Chỉ cần một instance.
- Factory: Không muốn Client tự tạo object.
- Builder: Object có nhiều thuộc tính, cần khởi tạo từng bước.

Structural – Tổ chức cấu trúc

- Adapter: Hai interface không tương thích cần làm việc với nhau.
- Decorator: Muốn bổ sung chức năng mà không sửa class gốc.
- Proxy: Muốn kiểm soát việc truy cập object thật.

Behavioral – Tổ chức hành vi

- Strategy: Có nhiều thuật toán và muốn thay đổi linh hoạt.
- Observer: Một đối tượng thay đổi, nhiều đối tượng cần nhận thông báo.
- Template Method: Nhiều class có chung quy trình nhưng khác một số bước.

Khuyến nghị khi phỏng vấn Backend Java/Spring Boot: Nên ưu tiên hiểu sâu Singleton, Factory, Builder, Strategy, Proxy và Observer. Đây là những Pattern có liên hệ trực tiếp với cách tổ chức Service, Dependency Injection, Spring AOP, Transaction và xử lý nghiệp vụ trong các ứng dụng Spring Boot.