# 1. RESTful API là gì?

RESTful API (Representational State Transfer API) là một kiểu thiết kế API dựa trên kiến trúc REST, cho phép các hệ thống giao tiếp và trao đổi dữ liệu với nhau thông qua giao thức HTTP.

Nói một cách đơn giản:

RESTful API giúp Frontend và Backend giao tiếp với nhau thông qua các HTTP Request và HTTP Response.

Mô hình hoạt động của RESTful API

Client (Frontend) <-> Server (Backend) <-> Database

# 2. Các HTTP Method trong RESTful API

RESTful API sử dụng các HTTP Method để xác định hành động muốn thực hiện trên một resource (tài nguyên).

Ví dụ resource là `users` (người dùng).

| HTTP Method | Chức năng                 | Ví dụ              |
| ----------- | ------------------------- | ------------------ |
| GET         | Lấy dữ liệu               | Lấy danh sách user |
| POST        | Tạo mới dữ liệu           | Tạo user           |
| PUT         | Cập nhật toàn bộ dữ liệu  | Cập nhật user      |
| PATCH       | Cập nhật một phần dữ liệu | Cập nhật email     |
| DELETE      | Xóa dữ liệu               | Xóa user           |

# 3. Các nguyên tắc của RESTful API

REST định nghĩa 6 ràng buộc kiến trúc (Architectural Constraints). Một API được xem là RESTful khi tuân thủ các ràng buộc này.

## 3.1. Client - Server

Client và Server phải được phân tách trách nhiệm.

- Client: Chịu trách nhiệm giao diện và trải nghiệm người dùng.
- Server: Chịu trách nhiệm xử lý nghiệp vụ, dữ liệu và bảo mật.

Lợi ích: Frontend và Backend có thể phát triển độc lập.

## 3.2. Stateless

Server không lưu trạng thái phiên làm việc của Client giữa các request.

Mỗi request phải chứa đầy đủ thông tin cần thiết để Server xử lý.

Lợi ích:

- Dễ dàng mở rộng hệ thống.
- Có thể phân phối request đến nhiều Server.
- Không cần phụ thuộc vào session được lưu trên một Server cụ thể.

Lưu ý: Stateless không có nghĩa là Server không lưu dữ liệu. Server vẫn lưu dữ liệu nghiệp vụ trong Database.

Ví dụ:

```
GET /api/users/1 HTTP/1.1
Authorization: Bearer <access_token>
```

Request tiếp theo cũng phải gửi thông tin xác thực cần thiết.

## 3.3. Cacheable

Response phải xác định được dữ liệu có thể được cache hay không.

Lợi ích: Giảm số lượng request đến Server và cải thiện hiệu năng.

Ví dụ:

```
Cache-Control: max-age=3600
```

Có nghĩa là response có thể được cache trong 3600 giây theo chính sách này.

## 3.4. Uniform Interface

Các thành phần giao tiếp phải tuân theo một giao diện thống nhất.

Một số nguyên tắc quan trọng:

- Resource được định danh bằng URI.
- Sử dụng HTTP Method để thao tác với resource.
- Dữ liệu được truyền thông qua các representation như JSON.
- Message phải chứa đủ thông tin để Client hiểu và xử lý.

Ví dụ:

```
GET /api/users/1
```

Không nên thiết kế endpoint theo kiểu:

```
GET /api/getUserById?id=1
```

Cách thứ hai không phải lúc nào cũng sai về mặt kỹ thuật, nhưng cách đầu tiên thể hiện rõ tư duy thiết kế resource-oriented của REST hơn.

## 3.5. Layered System

Nghĩa là có thể có các lớp trung gian.

Ví dụ:

```
Client
   |
API Gateway
   |
Load Balancer
   |
Backend Service
   |
Database
```

Lợi ích: Dễ dàng bổ sung API Gateway, Load Balancer, Proxy và các tầng bảo mật.

## 3.6. Code on Demand (Optional)

Server có thể gửi mã thực thi xuống Client, ví dụ JavaScript, để Client thực thi.

Đây là ràng buộc tùy chọn, không bắt buộc đối với RESTful API.

# 4. HTTP Status Code trong RESTful API

Backend sử dụng HTTP Status Code để thông báo kết quả xử lý request cho Client.

| Status Code               | Ý nghĩa                            | Trường hợp sử dụng             |
| ------------------------- | ---------------------------------- | ------------------------------ |
| 200 OK                    | Thành công                         | Lấy dữ liệu thành công         |
| 201 Created               | Đã tạo thành công                  | Tạo user thành công            |
| 204 No Content            | Thành công, không có response body | Xóa user thành công            |
| 400 Bad Request           | Request không hợp lệ               | Thiếu hoặc sai dữ liệu đầu vào |
| 401 Unauthorized          | Chưa xác thực                      | Token không hợp lệ             |
| 403 Forbidden             | Không có quyền truy cập            | User không đủ quyền            |
| 404 Not Found             | Không tìm thấy                     | User không tồn tại             |
| 409 Conflict              | Xung đột dữ liệu                   | Email đã tồn tại               |
| 500 Internal Server Error | Lỗi phía Server                    | Exception không được xử lý     |

# 6. Ưu điểm và nhược điểm của RESTful API

| Ưu điểm                        | Nhược điểm                                         |
| ------------------------------ | -------------------------------------------------- |
| Dễ triển khai và sử dụng       | Có thể trả về nhiều dữ liệu không cần thiết        |
| Frontend và Backend độc lập    | Nhiều request khi cần lấy nhiều loại dữ liệu       |
| Dễ mở rộng hệ thống            | Cần thiết kế API và versioning hợp lý              |
| Hỗ trợ caching                 | Stateless có thể làm tăng dữ liệu phải gửi lặp lại |
| Tương thích với nhiều nền tảng | Không tối ưu cho mọi kiểu giao tiếp thời gian thực |

# 7. Những câu hỏi thường gặp khi phỏng vấn

Câu 1: REST và RESTful API khác nhau như thế nào?

- REST: Là một architectural style (phong cách kiến trúc) được Roy Fielding giới thiệu.
- RESTful API: Là API được thiết kế và triển khai dựa trên các nguyên tắc, ràng buộc của REST.

Câu 2: RESTful API có bắt buộc sử dụng JSON không?

Không.

REST không bắt buộc sử dụng JSON. API có thể trao đổi dữ liệu bằng JSON, XML hoặc các định dạng representation khác.

JSON được sử dụng phổ biến vì đơn giản, nhẹ và dễ xử lý.

Câu 3: Tại sao RESTful API sử dụng Stateless?

Vì Stateless giúp Server không phải duy trì session riêng cho từng Client.

Nhờ đó, các request có thể được xử lý bởi nhiều Server khác nhau, giúp hệ thống dễ dàng mở rộng theo chiều ngang (Horizontal Scaling).

Câu 4: PUT và PATCH khác nhau như thế nào?

- PUT: Thay thế toàn bộ representation của resource.
- PATCH: Thực hiện cập nhật một phần resource.

Câu 5: GET và POST khác nhau như thế nào?

- GET: Lấy dữ liệu, không được thiết kế để thay đổi trạng thái nghiệp vụ của Server.
- POST: Gửi dữ liệu để Server xử lý, thường dùng để tạo resource hoặc thực hiện một nghiệp vụ.

Ngoài ra, GET là safe và idempotent; POST không mặc định có hai tính chất này.
