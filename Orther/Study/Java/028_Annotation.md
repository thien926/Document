## Bảng Tra Cứu Các Annotation Trong Spring Boot

| Nhóm | Annotation | Giải thích chi tiết | Ví dụ |
|---|---|---|---|
| Quản lý Bean và DI | @Component | Đánh dấu một class là bean chung để Spring quản lý trong IoC Container. Đây là annotation gốc, các annotation chuyên biệt khác thường kế thừa ý nghĩa của nó. | @Component class EmailValidator {} |
| | @Service | Dùng cho lớp xử lý nghiệp vụ ở service layer. Về kỹ thuật gần giống @Component, nhưng giúp code có ý nghĩa kiến trúc rõ ràng hơn. | @Service class UserService {} |
| | @Repository | Dùng cho lớp truy cập dữ liệu ở data access layer. Ngoài việc là bean, nó còn giúp Spring chuyển các lỗi persistence sang dạng exception thống nhất của Spring. | @Repository class UserRepository {} |
| | @Controller | Dùng cho Spring MVC controller. Thường trả về view name hoặc model để renderer giao diện web. | @Controller class PageController {} |
| | @RestController | Kết hợp giữa @Controller và @ResponseBody. Dùng cho API REST, thường trả về JSON/XML thay vì view. | @RestController class UserApiController {} |
| | @Bean | Định nghĩa một bean bằng phương thức trong class cấu hình. Hữu ích khi bạn không thể hoặc không muốn dùng @Component trên class nguồn. | @Bean ObjectMaper objectMaper() { ... } |
| | @Primary | Chỉ định bean mặc định khi có nhiều bean cùng kiểu và Spring không biết chọn bean nào. | @Primary @Bean DataSource primaryDataSource() { ... } |
| | @Lazy | Bean chỉ được khởi tạo khi thực sự được dụng lần đầu, thay vì tạo ngay khi ứng dụng start. | @Lazy @Service class ReportService {} |
| | @Qualifier | Chọn đúng bean khi có nhiều bean cùng kiểu. Thường dùng cùng với injection bằng constructor hoặc field. | public UserServiceImpl(@Qualifier("fastUserService") UserService s) {} |
| Cấu hình Spring | @SpringBootApplication | Annotation tổng hợp của @Configuration, @ComponentScan, và @EnableAutoConfiguration. Thường đặt ở class main của ứng dụng. | @SpringBootApplication class App {} |
| | @Configuration | Đánh dấu class chứa các phương thức tạo bean hoặc cấu hình Spring. | @Configuration class AppConfig {} |
| | @EnableAutoConfiguration | Cho Spring Boot tự cấu hình dựa trên dependency có trong project và các setting hiện có. | Thường đã có sẵn bên trong @SpringBootApplication |
| | @ComponentScan | Chỉ định package để Spring quét và tìm các class có @Component, @Service, @Repository, @Controller. | @ComponentScan("com.example") |
| | @PropertySource | Nạp file cấu hình ngoài như .properties vào Environment. Thường dùng với file properties; YAML không phải cách mặc định của annotation này. | @PropertySource("classpath:app.properties") |
| | @EnableAsync | Bật khả năng chạy bất đồng bộ cho các method có @Async. | @EnableAsync class AsyncConfig {} |
| | @EnableScheduling | Bật khả năng chạy tác vụ theo lịch học cho các method có @Scheduled. | @EnableScheduling class ScheduleConfig {} |
| | @Import | Import thêm các class cấu hình khác vào context hiện tại. | @Import({DbConfig.class, SecurityConfig.class}) |
| Xử lý HTTP Request | @RequestMapping | Mapping tổng quát cho URL và HTTP method. Có thể đặt ở class để tạo prefix chung, hoặc ở method để map chi tiết. | @RequestMapping("/users") |
| | @GetMapping | Shortcut của @RequestMapping(method = RequestMethod.GET). Dùng cho request HTTP GET. | @GetMapping("/users/{id}") |
| | @PostMapping | Shortcut cho request HTTP POST. | @PostMapping("/users") |
| | @PutMapping | Shortcut cho request HTTP PUT. | @PutMapping("/users/{id}") |
| | @DeleteMapping | Shortcut cho request HTTP DELETE. | @DeleteMapping("/users/{id}") |
| | @PatchMapping | Shortcut cho request HTTP PATCH, thường dùng khi cập nhật một phần dữ liệu. | @PatchMapping("/users/{id}") |
| | @RequestParam | Lấy dữ liệu từ query string hoặc form data. Dùng cho tham số kiểu ?page=1&size=20. | @RequestParam(defaultValue="0") int page |
| | @PathVariable | Lấy dữ liệu từ một phần của URL path. | @PathVariable Long id |
| | @RequestBody | Map dữ liệu từ body của request sang đối tượng Java bằng message converter, thường là JSON. | create(@RequestBody UserCreateRequest req) |
| | @ResponseBody | Ghi trực tiếp giá trị trả về vào HTTP response body, không đi qua view resolver. | @ResponseBody UserDto get() {} |
| | @CrossOrigin | Cho phép request từ domain khác theo chính sách CORS. | @CrossOrigin(origins = "https://ui.example.com") |
| Xử lý bảo mật | @Secured | Kiểm tra role được phép truy cập method hoặc class. Cách này đơn giản, thường dùng role trực tiếp. | @Secured("ROLE_ADMIN") |
| | @PreAuthorize | Kiểm tra quyền trước khi method chạy. Hỗ trợ SpEL nên rất linh hoạt, có thể kiểm tra role, tham số, hoặc authentication hiện tại. | @PreAuthorize("hasRole('ADMIN')") |
| | @PostAuthorize | Kiểm tra quyền sau khi method chạy, thường dùng khi muốn kiểm tra cả dữ liệu trả về. | @PostAuthorize("returnObject.owner == authentication.name") |
| | @EnableGlobalMethodSecurity | Bật method security cho @Secured, @PreAuthorize, @PostAuthorize. Đây là cách cũ; nếu dùng Spring Security 6+ thì nên xem @EnableMethodSecurity. | @EnableMethodSecurity(prePostEnabled = true, securedEnabled = true) |
| JPA và Hibernate | @Entity | Đánh dấu một class là entity, tức là lớp được ánh xạ với bảng trong database. | @Entity class User {} |
| | @Table | Chỉ định tên bảng, unique constraint, index, hoặc các thiết lập bảng khác. | @Table(name = "users") |
| | @Id | Chỉ định field là khóa chính của entity. | @Id private Long id; |
| | @GeneratedValue | Chỉ định cách sinh giá trị cho khóa chính. | @GeneratedValue(strategy = GenerationType.IDENTITY) |
| | @Column | Ánh xạ field với cột trong bảng và cấu hình như nullable, length, unique. | @Column(name = "full_name", nullable = false, length = 100) |
| | @ManyToOne | Nhiều bản ghi của entity này liên kết với một bản ghi ở entity khác. Thường là phía giữ khóa ngoại. | @ManyToOne @JoinColumn(name = "department_id") |
| | @OneToMany | Một bản ghi có thể có nhiều bản ghi con. Thường là phía ngược của quan hệ và dùng mappedBy. | @OneToMany(mappedBy = "department") |
| | @OneToOne | Quan hệ một-một giữa hai entity. Thường dùng với khóa ngoại duy nhất. | @OneToOne @JoinColumn(name = "profile_id") |
| | @ManyToMany | Quan hệ nhiều-nhiều, thường cần bảng trung gian. | @ManyToMany @JoinTable(name = "user_roles") |
| | @JoinColumn | Chỉ định tên cột khóa ngoại trong bảng hiện tại. | @JoinColumn(name = "customer_id") |
| | @MappedSuperclass | Class cha chứa các field dùng chung cho nhiều entity con. Class này không tạo ra bảng riêng trong database. | @MappedSuperclass abstract class BaseEntity {} |
| | @Embeddable | Đánh dấu một object value có thể nhúng vào nhiều entity khác, thường đi cùng @Embedded. | @Embeddable class Address {} |
| Xử lý lỗi | @ControllerAdvice | Xử lý lỗi và logic dùng chung cho toàn bộ controller. Có thể xử lý exception, ràng buộc dữ liệu, hoặc thêm model chung. Nếu API trả JSON thì thường dùng @RestControllerAdvice. | @ControllerAdvice class GlobalExceptionHandler {} |
| | @ExceptionHandler | Bắt và xử lý một loại exception cụ thể. Có thể đặt trong controller hoặc trong class advice. | @ExceptionHandler(UserNotFoundException.class) |
| Hỗ trợ test | @SpringBootTest | Nạp full Spring context để test tích hợp gần với môi trường thật nhất. Chậm hơn slice test nhưng bao phủ rộng hơn. | @SpringBootTest |
| | @WebMvcTest | Chỉ nạp web layer, thường dùng để test controller với MockMvc. Phù hợp khi muốn test mapping, validation, response. | @WebMvcTest(UserController.class) |
| | @DataJpaTest | Chỉ nạp phần JPA/repository layer, thường dùng để test query, mapping entity, repository. | @DataJpaTest |
| | @MockBean | Thay một bean thật trong Spring context bằng mock của Mockito để cô lập dependency khi test. | @MockBean UserService userService; |
| | @SpyBean | Bọc một bean thật bằng spy của Mockito để vẫn dùng logic thật nhưng có thể override một phần hành vi. | @SpyBean UserService userService; |

------------------------------
Nếu bạn cần bổ sung thêm một số annotation phổ biến khác (như @Value, @Autowired, @Scheduled chi tiết...) hoặc muốn chuyển bảng này sang định dạng khác, hãy cho tôi biết nhé!

