Lệnh `docker run` được sử dụng để tạo và chạy các container từ các image Docker. Dưới đây là hướng dẫn chi tiết về cách sử dụng lệnh này:

### Cú pháp chung

```sh
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

- **`IMAGE`**: Tên của image Docker mà bạn muốn chạy.
- **`COMMAND`**: Lệnh sẽ được thực thi trong container.
- **`[ARG...]`**: Các tham số bổ sung cho lệnh.

### Các tùy chọn phổ biến

- **`--name`**: Đặt tên cho container.
- **`-d`**: Chạy container ở chế độ nền (detached mode).
- **`-p`**: Map cổng từ host đến container.
- **`-v`**: Gắn kết (mount) volume từ host vào container.
- **`-e`**: Thiết lập biến môi trường cho container.
- **`--rm`**: Tự động xóa container sau khi nó dừng.
- **`--network`**: Chỉ định mạng cho container.
- **`--restart`**: Chính sách khởi động lại container (ví dụ: `no`, `on-failure`, `always`, `unless-stopped`).

### Ví dụ cụ thể

1. **Chạy container ở chế độ nền với tên cụ thể và map cổng:**

   ```sh
   docker run --name my-nginx -d -p 8080:80 nginx
   ```

   - **Giải thích**:
     - `--name my-nginx`: Đặt tên container là `my-nginx`.
     - `-d`: Chạy container ở chế độ nền.
     - `-p 8080:80`: Map cổng 8080 của host đến cổng 80 của container.
     - `nginx`: Sử dụng image `nginx`.

2. **Chạy container với biến môi trường và volume:**

   ```sh
   docker run --name my-app -e APP_ENV=production -v /host/data:/app/data my-image
   ```

   - **Giải thích**:
     - `-e APP_ENV=production`: Thiết lập biến môi trường `APP_ENV` với giá trị `production`.
     - `-v /host/data:/app/data`: Gắn kết thư mục `/host/data` trên host vào `/app/data` trong container.
     - `my-image`: Sử dụng image `my-image`.

3. **Chạy container và tự động xóa sau khi dừng:**

   ```sh
   docker run --rm my-image
   ```

   - **Giải thích**:
     - `--rm`: Tự động xóa container sau khi nó dừng.
     - `my-image`: Sử dụng image `my-image`.

4. **Chạy container với chính sách khởi động lại:**

   ```sh
   docker run --name my-app --restart unless-stopped my-image
   ```

   - **Giải thích**:
     - `--restart unless-stopped`: Container sẽ tự động khởi động lại trừ khi nó bị dừng thủ công.
     - `my-image`: Sử dụng image `my-image`.

5. **Chạy container với quyền truy cập thiết bị và đặc quyền:**

   ```sh
   docker run --name privileged-container --privileged --device=/dev/sdc my-image
   ```

   - **Giải thích**:
     - `--privileged`: Cung cấp quyền đặc biệt cho container.
     - `--device=/dev/sdc`: Cung cấp quyền truy cập thiết bị `/dev/sdc` cho container.
     - `my-image`: Sử dụng image `my-image`.

6. **Chạy container với giới hạn tài nguyên:**

   ```sh
   docker run --name limited-container --memory=512m --cpus=1 my-image
   ```

   - **Giải thích**:
     - `--memory=512m`: Giới hạn bộ nhớ của container ở mức 512 MB.
     - `--cpus=1`: Giới hạn container sử dụng tối đa 1 CPU.
     - `my-image`: Sử dụng image `my-image`.

7. **Chạy container với quyền truy cập vào GPU:**

   ```sh
   docker run --name gpu-container --gpus all my-image
   ```

   - **Giải thích**:
     - `--gpus all`: Cung cấp quyền truy cập vào tất cả các GPU cho container.
     - `my-image`: Sử dụng image `my-image`.

8. **Chạy container với giới hạn băng thông mạng:**

   ```sh
   docker run --name bandwidth-limited-container --network=bridge --tc-htb rate=1mbit my-image
   ```

   - **Giải thích**:
     - `--network=bridge`: Sử dụng mạng bridge cho container.
     - `--tc-htb rate=1mbit`: Giới hạn băng thông mạng của container ở mức 1 Mbit/s.
     - `my-image`: Sử dụng image `my-image`.

9. **Chạy container với thời gian chờ khi dừng:**

   ```sh
   docker run --name timeout-container --stop-timeout=30 my-image
   ```

   - **Giải thích**:
     - `--stop-timeout=30`: Thiết lập thời gian chờ 30 giây trước khi buộc dừng container.
     - `my-image`:  