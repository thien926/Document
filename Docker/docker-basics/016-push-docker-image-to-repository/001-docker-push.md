Lệnh `docker push` được sử dụng để tải một **Docker image** từ máy cục bộ lên một **Docker registry**, chẳng hạn như Docker Hub hoặc một registry tự quản lý. Điều này cho phép bạn chia sẻ image của mình với người khác hoặc triển khai trên các môi trường khác nhau.

**Cú pháp:**
```bash
docker push [TÙY_CHỌN] TÊN_IMAGE[:TAG]
```

**Các bước thực hiện:**

1. **Đăng nhập vào registry:**
   Trước khi đẩy image, bạn cần đăng nhập vào registry mục tiêu.
   ```bash
   docker login
   ```
   Lệnh này sẽ yêu cầu bạn nhập tên người dùng và mật khẩu.

2. **Gắn thẻ (tag) cho image:**
   Để xác định đích đến cho image trên registry, bạn cần gắn thẻ cho image với định dạng phù hợp.
   ```bash
   docker tag TÊN_IMAGE_CỤC_BỘ TÊN_NGƯỜI_DÙNG/TÊN_REPOSITORY:TAG
   ```
   Ví dụ:
   ```bash
   docker tag my-app myusername/my-app:v1.0
   ```
   Trong ví dụ này, `my-app` là tên image cục bộ, `myusername/my-app` là tên repository trên Docker Hub, và `v1.0` là tag của image.

3. **Đẩy image lên registry:**
   Sau khi gắn thẻ, bạn có thể đẩy image lên registry bằng lệnh:
   ```bash
   docker push myusername/my-app:v1.0
   ```
   Lệnh này sẽ tải lên các lớp (layers) của image lên repository được chỉ định.

**Tùy chọn phổ biến:**

- `-a`, `--all-tags`: Đẩy tất cả các tag của image lên repository.
  ```bash
  docker push --all-tags myusername/my-app
  ```
  Lệnh này sẽ đẩy tất cả các tag liên quan đến `myusername/my-app` lên registry.

- `--disable-content-trust`: Bỏ qua việc ký nội dung của image khi đẩy.
  ```bash
  docker push --disable-content-trust myusername/my-app:v1.0
  ```
  Theo mặc định, Docker sử dụng `Content Trust` để đảm bảo tính toàn vẹn và xác thực của image. Tùy chọn này cho phép bạn bỏ qua kiểm tra đó.

**Lưu ý:**

- Khi đẩy image lên một registry tự quản lý (private registry), đảm bảo rằng registry đó được cấu hình đúng và có thể truy cập từ máy của bạn.

- Sử dụng tag cụ thể (ví dụ: `v1.0`) thay vì `latest` để tránh nhầm lẫn và đảm bảo bạn biết chính xác phiên bản nào đang được sử dụng.

- Đảm bảo bạn có quyền truy cập ghi (write access) vào repository trên registry mục tiêu.
