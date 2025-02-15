## 🗄 **Persistent Container Data là gì?**
**Persistent Container Data** là dữ liệu được lưu trữ **bên ngoài vòng đời của container**, giúp dữ liệu không bị mất khi container dừng, khởi động lại hoặc bị xóa.

Mặc định, dữ liệu bên trong container chỉ tồn tại trong quá trình container chạy. Nếu container bị xóa (`docker rm`), mọi dữ liệu lưu trong đó cũng sẽ biến mất. Để giải quyết vấn đề này, ta sử dụng **Persistent Storage** thông qua **Volumes hoặc Bind Mounts**.

---

## ❓ **Tại sao cần Persistent Container Data?**
Nếu container của bạn chỉ chạy một ứng dụng không lưu dữ liệu (stateless app), thì không cần thiết. Nhưng nếu container có dữ liệu quan trọng, cần lưu trữ lâu dài, thì **Persistent Storage là bắt buộc**.

### 🔥 **Tình huống cần Persistent Storage:**
1. **Lưu dữ liệu của Database (MySQL, PostgreSQL, MongoDB...)**
   - Nếu database container bị xóa mà không có Persistent Storage, toàn bộ dữ liệu sẽ biến mất.
   - Dùng **Volumes** để lưu dữ liệu trên ổ đĩa của host.

2. **Lưu file upload từ ứng dụng Web**
   - Một website có chức năng upload ảnh, nếu không lưu trữ ngoài container, ảnh sẽ mất khi container restart.

3. **Chia sẻ dữ liệu giữa nhiều container**
   - Nếu nhiều container cần truy cập cùng một bộ dữ liệu (ví dụ: API và worker xử lý dữ liệu chung).

4. **Backup và restore dữ liệu**
   - Lưu dữ liệu trên host giúp dễ dàng backup, tránh mất mát khi update container.

---

## 🔧 **Cách triển khai Persistent Storage**
Có 2 cách phổ biến để làm điều này trong Docker:

### 🏗 **1. Docker Volumes (Khuyến nghị)**
Docker Volumes lưu trữ dữ liệu **bên ngoài container** trong `/var/lib/docker/volumes/`. Điều này giúp dữ liệu không bị mất khi container bị xóa.

👉 **Tạo volume và gắn vào container:**
```sh
docker volume create my_data
docker run -d -v my_data:/app/data my_image
```
**Ưu điểm:**
- Quản lý tốt, dễ dàng backup.
- Hiệu suất cao hơn Bind Mounts.
- Hoạt động tốt với Docker Swarm & Kubernetes.

---

### 🏗 **2. Bind Mounts (Linh hoạt nhưng rủi ro hơn)**
Bind Mounts sử dụng thư mục cụ thể trên máy host, cung cấp nhiều quyền kiểm soát hơn.

👉 **Gắn thư mục host vào container:**
```sh
docker run -d -v /home/user/data:/app/data my_image
```
**Ưu điểm:**
- Linh hoạt, có thể gắn bất kỳ thư mục nào từ máy host.
- Dễ dàng chỉnh sửa file trên host.

**Nhược điểm:**
- Phụ thuộc vào cấu trúc thư mục host, dễ gặp lỗi phân quyền.
- Hiệu suất kém hơn Docker Volumes.

---

## 🎯 **Khi nào dùng Volume và khi nào dùng Bind Mount?**
| **Tiêu chí**       | **Docker Volume** (Khuyến nghị) | **Bind Mount** |
|------------------|-------------------------|-----------------|
| Quản lý dễ dàng | ✅ Có lệnh `docker volume` | ❌ Phải tự quản lý |
| Hiệu suất       | ✅ Tốt hơn trên Docker | ❌ Chậm hơn |
| Bảo mật         | ✅ Docker kiểm soát quyền | ❌ Phụ thuộc vào host |
| Backup         | ✅ Dễ backup | ❌ Khó backup hơn |
| Dùng trên nhiều môi trường | ✅ Không phụ thuộc vào host | ❌ Phụ thuộc vào đường dẫn host |

---

## ✅ **Tóm tắt**
- **Persistent Container Data** giúp lưu dữ liệu **không bị mất** khi container bị xóa hoặc restart.
- **Dữ liệu quan trọng (database, file upload, logs,...) cần được lưu ngoài container.**
- **Docker Volume** là cách lưu trữ tốt nhất, trong khi **Bind Mounts** phù hợp khi cần truy cập trực tiếp từ máy host.
