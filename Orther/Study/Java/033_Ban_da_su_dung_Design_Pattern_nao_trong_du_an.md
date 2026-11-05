Với kinh nghiệm Java Spring Boot, ASP.NET Core, .NET 8 và các dự án Backend mà bạn đã tham gia, mình khuyên bạn nên trả lời theo hướng nêu 3–4 Design Pattern thực sự hiểu và có thể giải thích cách áp dụng, thay vì liệt kê quá nhiều Pattern.

Đối với vị trí Backend Developer (Java/Spring Boot), bạn có thể chuẩn bị câu trả lời như sau.

# 1. Câu trả lời mẫu khi phỏng vấn

Khuyến nghị sử dụng

Câu hỏi: Bạn đã sử dụng Design Pattern nào trong dự án?

Trả lời tiếng Việt:

Trong các dự án trước đây, tôi đã sử dụng hoặc làm việc với một số Design Pattern như Singleton, Factory, Strategy và Proxy.

Cụ thể:

1. Singleton Pattern: Trong Spring Boot, các Bean như Service và Repository mặc định sử dụng Singleton Scope. Spring Container quản lý instance và inject vào các thành phần cần sử dụng.
2. Factory Pattern: Tôi sử dụng Factory để tập trung logic khởi tạo và lựa chọn implementation phù hợp. Ví dụ, trong hệ thống hỗ trợ nhiều phương thức thanh toán hoặc xử lý nhiều loại dữ liệu đầu vào.
3. Strategy Pattern: Pattern này phù hợp với những nghiệp vụ có nhiều thuật toán xử lý khác nhau. Ví dụ, tính phí vận chuyển hoặc áp dụng các loại chính sách giảm giá. Tôi có thể tách từng thuật toán thành một implementation riêng và lựa chọn theo nghiệp vụ.
4. Proxy Pattern: Trong Spring Boot, tôi có tìm hiểu và làm việc với cơ chế Proxy thông qua Spring AOP, chẳng hạn như `@Transactional`. Spring sử dụng Proxy để bổ sung các hành vi như quản lý transaction mà không cần đưa toàn bộ logic đó vào business code.

Tôi thường lựa chọn Design Pattern dựa trên vấn đề thực tế cần giải quyết, tránh áp dụng Pattern khi chưa thực sự cần thiết.

# 2. Nếu nhà tuyển dụng hỏi sâu từng Pattern

Bạn nên chuẩn bị các câu hỏi tiếp theo vì người phỏng vấn thường không dừng lại ở việc nghe bạn liệt kê tên Pattern.

| Pattern   | Câu hỏi có thể được hỏi                                |
| --------- | ------------------------------------------------------ |
| Singleton | Spring Singleton Bean có Thread-safe không?            |
| Factory   | Factory khác gì việc sử dụng `if-else` thông thường?   |
| Strategy  | Strategy khác Factory như thế nào?                     |
| Proxy     | Spring sử dụng Proxy như thế nào với `@Transactional`? |

Đặc biệt, hãy nắm chắc 4 nội dung sau.

### 2.1. Singleton – Spring Bean

Interviewer: Tại sao Spring Bean mặc định là Singleton?

Trả lời:

Vì Spring muốn tái sử dụng cùng một instance trong phạm vi ApplicationContext, tránh việc tạo nhiều object không cần thiết và giúp quản lý vòng đời Bean tập trung.

Tuy nhiên, Singleton không đồng nghĩa với Thread-safe. Vì vậy, tôi thường thiết kế Service theo hướng Stateless, không lưu dữ liệu thay đổi theo từng request trong instance variable.

### 2.2. Factory – Lựa chọn implementation

Interviewer: Khi nào bạn sử dụng Factory?

Trả lời:

Khi hệ thống có nhiều implementation cùng thực hiện một interface và cần lựa chọn implementation dựa trên điều kiện nghiệp vụ.

Ví dụ, hệ thống hỗ trợ nhiều phương thức thanh toán như Momo, PayPal và Credit Card. Tôi có thể sử dụng Factory để lựa chọn Payment implementation tương ứng, thay vì đặt nhiều điều kiện `if-else` trong Service.

### 2.3. Strategy – Thay đổi thuật toán

Interviewer: Factory và Strategy khác nhau như thế nào?

Trả lời:

Factory tập trung vào việc tạo hoặc lựa chọn đối tượng phù hợp.

Strategy tập trung vào việc đóng gói các thuật toán khác nhau thành những implementation riêng biệt và cho phép thay đổi thuật toán sử dụng.

Hai Pattern có thể kết hợp với nhau. Ví dụ, Factory lựa chọn Strategy phù hợp dựa trên loại nghiệp vụ.

### 2.4. Proxy – Spring AOP

Interviewer: `@Transactional` có liên quan đến Design Pattern nào?

Trả lời:

`@Transactional` trong Spring thường hoạt động thông qua Proxy Pattern.

Khi một method được đánh dấu `@Transactional`, Spring tạo Proxy để intercept lời gọi method, thực hiện các thao tác quản lý transaction như bắt đầu transaction, commit hoặc rollback.

Nhờ đó, chúng ta không cần tự viết logic quản lý transaction trong từng Service.

# 3. Một ví dụ thực tế kết hợp Factory và Strategy trong Spring Boot

Đây là ví dụ khá tốt để trình bày nếu interviewer muốn biết bạn hiểu cách áp dụng Pattern vào Backend như thế nào.

Giả sử hệ thống có nhiều chính sách tính phí vận chuyển.

Bước 1: Định nghĩa Strategy

```
public interface ShippingStrategy {

    BigDecimal calculateFee(BigDecimal weight);
}
```

Bước 2: Các implementation

```
@Component("STANDARD")
public class StandardShipping implements ShippingStrategy {

    @Override
    public BigDecimal calculateFee(BigDecimal weight) {
        return weight.multiply(BigDecimal.TEN);
    }
}
```

```
@Component("EXPRESS")
public class ExpressShipping implements ShippingStrategy {

    @Override
    public BigDecimal calculateFee(BigDecimal weight) {
        return weight.multiply(BigDecimal.valueOf(20));
    }
}
```

Bước 3: Factory lựa chọn Strategy

```
@Component
public class ShippingFactory {

    private final Map<String, ShippingStrategy> strategies;

    public ShippingFactory(
            Map<String, ShippingStrategy> strategies) {
        this.strategies = strategies;
    }

    public ShippingStrategy getStrategy(String type) {

        ShippingStrategy strategy =
                strategies.get(type.toUpperCase());

        if (strategy == null) {
            throw new IllegalArgumentException(
                    "Unsupported shipping type"
            );
        }

        return strategy;
    }
}
```

Bước 4: Sử dụng trong Service

```
@Service
public class ShippingService {

    private final ShippingFactory shippingFactory;

    public ShippingService(ShippingFactory shippingFactory) {
        this.shippingFactory = shippingFactory;
    }

    public BigDecimal calculateFee(
            String type,
            BigDecimal weight) {

        ShippingStrategy strategy =
                shippingFactory.getStrategy(type);

        return strategy.calculateFee(weight);
    }
}
```

Phân tích:

- Strategy: Đóng gói từng thuật toán tính phí vận chuyển.
- Factory: Lựa chọn thuật toán phù hợp.
- Dependency Injection: Spring inject các implementation vào Factory.
- OCP: Có thể bổ sung phương thức vận chuyển mới mà không cần sửa logic lựa chọn trong Factory.

Đây là một ví dụ thể hiện khả năng vận dụng nhiều kiến thức Java/Spring Boot trong cùng một bài toán Backend.

# 4. Lưu ý quan trọng khi trả lời phỏng vấn

Mình khuyên bạn không nên khẳng định đã trực tiếp triển khai tất cả Pattern trên trong dự án nếu thực tế chỉ mới tìm hiểu hoặc sử dụng cơ chế có sẵn của Spring.

Hãy phân biệt:

| Pattern   | Cách diễn đạt trung thực                                     |
| --------- | ------------------------------------------------------------ |
| Singleton | Đã sử dụng thông qua Spring Singleton Bean                   |
| Factory   | Đã triển khai nếu thực sự có Factory trong code dự án        |
| Strategy  | Đã triển khai nếu thực sự có nhiều implementation/thuật toán |
| Proxy     | Đã làm việc với cơ chế Proxy của Spring AOP                  |

Một câu hỏi mình muốn làm rõ để xây dựng câu trả lời sát kinh nghiệm thực tế của bạn hơn:

Trong các dự án Java Spring Boot bạn từng làm (Carshare Japan, RMS Enterprise, hoặc các dự án khác), bạn có trực tiếp triển khai Factory/Strategy hay chủ yếu sử dụng các cơ chế có sẵn của Spring như Singleton Bean, Dependency Injection, `@Transactional`?

Từ đó có thể xây dựng một câu trả lời vừa chuyên nghiệp, vừa có ví dụ thực tế để bạn tự tin trả lời khi interviewer hỏi sâu.