Để **persist data** (lưu trữ dữ liệu lâu dài) trong container (Docker), bạn có thể sử dụng **Volumes** hoặc **Bind Mounts**. Dưới đây là các cách thực hiện:

---

## 1️⃣ Sử dụng **Volumes** (Khuyến nghị)
Volumes là phương pháp được Docker quản lý, giúp dữ liệu bền vững hơn ngay cả khi container bị xóa.

**Cách sử dụng:**
- Tạo volume:  
  ```sh
  docker volume create my_volume
  ```
- Chạy container với volume:  
  ```sh
  docker run -d -v my_volume:/data --name my_container my_image
  ```
  📌 **Giải thích:**  
  - `-v my_volume:/data`: Gắn volume `my_volume` vào `/data` trong container.
  - Dữ liệu trong `/data` sẽ được lưu trong volume `my_volume`, ngay cả khi container bị xóa.

- Xem danh sách volumes:  
  ```sh
  docker volume ls
  ```
- Xóa volume:  
  ```sh
  docker volume rm my_volume
  ```

🔹 **Ưu điểm của Volumes:**  
✅ Dữ liệu an toàn, không bị mất khi container bị xóa.  
✅ Hiệu suất tốt hơn trên Docker-managed storage.  
✅ Dễ dàng backup & restore.  

---

## 2️⃣ Sử dụng **Bind Mounts**
Bind mounts cho phép container truy cập trực tiếp vào một thư mục trên máy host.

**Cách sử dụng:**
```sh
docker run -d -v /path/to/local/folder:/data --name my_container my_image
```
📌 **Giải thích:**  
- `/path/to/local/folder` là thư mục trên máy host.  
- `/data` là thư mục trong container.

🔹 **Ưu điểm của Bind Mounts:**  
✅ Linh hoạt, có thể chỉnh sửa dữ liệu trực tiếp từ host.  
✅ Hữu ích cho môi trường phát triển.

🔸 **Nhược điểm:**  
❌ Không được Docker quản lý, dễ gặp lỗi quyền truy cập.  
❌ Không dễ dàng di chuyển dữ liệu giữa các môi trường.

---

## 3️⃣ Sử dụng **Docker Compose** (Volumes)
Nếu bạn dùng `docker-compose.yml`, có thể định nghĩa volumes như sau:

```yaml
version: '3'
services:
  app:
    image: my_image
    volumes:
      - my_volume:/data
volumes:
  my_volume:
```
Chạy bằng:
```sh
docker-compose up -d
```

---

## 4️⃣ Dùng **Named Volumes trong Dockerfile** (Tự động tạo Volume)
Trong **Dockerfile**, bạn có thể dùng `VOLUME` để khai báo thư mục cần persist:

```dockerfile
VOLUME /data
```
Mỗi lần chạy container từ image này, Docker sẽ tự tạo một volume gắn vào `/data`.

---

## So sánh nhanh
| Phương pháp | Ưu điểm | Nhược điểm |
|------------|--------|------------|
| **Volumes** | An toàn, dễ backup, quản lý bởi Docker | Ít linh hoạt hơn Bind Mounts |
| **Bind Mounts** | Truy cập trực tiếp vào dữ liệu trên host | Có thể gặp lỗi quyền truy cập, khó di chuyển |
| **Dockerfile `VOLUME`** | Tự động tạo volume, ít cấu hình | Không kiểm soát được tên volume |

🚀 **Khuyến nghị:**  
- Dùng **Volumes** nếu bạn muốn persist data trong môi trường production.  
- Dùng **Bind Mounts** khi phát triển để dễ chỉnh sửa dữ liệu.  
- Dùng **Docker Compose** nếu có nhiều container.
