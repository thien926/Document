## 🔹 **Giải thích đường dẫn**  
```plaintext
\\wsl$\docker-desktop\mnt\docker-desktop-disk\data\docker\volumes
```
💡 Đây là đường dẫn **truy cập vào Docker Volumes trên WSL từ Windows Explorer hoặc Command Prompt**.

| Thành phần | Giải thích |
|------------|------------|
| `\\wsl$` | Truy cập vào **hệ thống tệp của WSL** từ Windows |
| `docker-desktop` | Hệ thống WSL do **Docker Desktop quản lý** |
| `mnt/docker-desktop-disk` | Mount point của ổ đĩa ảo Docker trong WSL |
| `data/docker/volumes` | Nơi Docker lưu **dữ liệu volumes** trên hệ thống |

---

## 🔹 **Sử dụng đường dẫn này để làm gì?**
1. **Truy cập Docker Volumes từ Windows Explorer**  
   📌 **Dán đường dẫn này vào thanh địa chỉ của Windows Explorer:**  
   ```
   \\wsl$\docker-desktop\mnt\docker-desktop-disk\data\docker\volumes
   ```
   Bạn sẽ thấy danh sách **tất cả volumes của Docker**.

2. **Mở volume bằng Command Prompt hoặc PowerShell**  
   📌 **Dùng `cd` để di chuyển đến thư mục volumes:**  
   ```powershell
   cd "\\wsl$\docker-desktop\mnt\docker-desktop-disk\data\docker\volumes"
   ```
   Sau đó, bạn có thể **duyệt dữ liệu của từng volume**.

3. **Sao lưu hoặc xóa dữ liệu volumes thủ công**  
   - Nếu bạn muốn sao lưu một volume cụ thể, bạn có thể **copy thư mục volume** từ đây.  
   - **⚠️ Không chỉnh sửa file trực tiếp từ Windows** (có thể gây lỗi quyền truy cập). Hãy thao tác qua WSL hoặc Docker CLI.

---

## 🔹 **Cách khác để xem danh sách volumes trong Docker**  
Nếu bạn chỉ muốn liệt kê các volume mà không cần truy cập trực tiếp, dùng lệnh:  
```sh
docker volume ls
```
📌 **Output mẫu:**  
```
DRIVER    VOLUME NAME
local     todo-db
local     postgres_data
```