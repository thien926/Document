Lệnh `docker rm` được sử dụng để xóa một hoặc nhiều container trong Docker.  

---

## 🔹 Cú pháp lệnh  
```sh
docker rm [OPTIONS] CONTAINER [CONTAINER...]
```
- `CONTAINER`: Tên hoặc ID của container cần xóa.  
- `[OPTIONS]`: Các tùy chọn để thay đổi cách xóa container.  

---

## 🔹 Các tùy chọn phổ biến  

| Tùy chọn | Mô tả |
|----------|-------|
| `-f`, `--force` | Bắt buộc dừng và xóa container đang chạy. |
| `-v`, `--volumes` | Xóa các volume liên kết với container. |
| `-l`, `--link` | Xóa một liên kết (link) thay vì container. |

---

## 🔹 Ví dụ sử dụng  

### 1️⃣ Xóa một container  
```sh
docker rm my-container
```
📌 **Mô tả**: Xóa container có tên `my-container` (chỉ áp dụng nếu container đã dừng).  

📌 **Kiểm tra lại bằng lệnh**:  
```sh
docker ps -a
```
Container sẽ không còn xuất hiện trong danh sách.  

---

### 2️⃣ Xóa nhiều container cùng lúc  
```sh
docker rm container1 container2 container3
```
📌 **Mô tả**: Xóa các container `container1`, `container2` và `container3`.  

---

### 3️⃣ Xóa container đang chạy (bắt buộc dừng trước)  
```sh
docker rm -f my-container
```
📌 **Mô tả**: Nếu container `my-container` đang chạy, lệnh này sẽ buộc dừng (`docker stop`) và xóa nó ngay lập tức.  

---

### 4️⃣ Xóa container kèm theo volume liên kết  
```sh
docker rm -v my-container
```
📌 **Mô tả**: Xóa container `my-container` và tất cả các volume liên kết với nó.  

---

### 5️⃣ Xóa tất cả container đã dừng  
```sh
docker rm $(docker ps -aq)
```
📌 **Mô tả**:  
- `docker ps -aq`: Lấy danh sách tất cả container.  
- `docker rm`: Xóa tất cả các container đó.  

🔹 **Nếu có container đang chạy, cần dùng `-f`:**  
```sh
docker rm -f $(docker ps -aq)
```

---

### 6️⃣ Xóa toàn bộ container trừ container đang chạy  
```sh
docker ps -aq --filter "status=exited" | xargs docker rm
```
📌 **Mô tả**: Chỉ xóa các container đã dừng (`exited`), giữ lại container đang chạy.  

---

## 🔹 Một số lưu ý khi dùng `docker rm`  
✔ Không thể xóa container đang chạy nếu không dùng `-f`.  
✔ Volume ẩn (`anonymous volume`) không bị xóa trừ khi dùng `-v`.  
✔ Nếu container có liên kết (`link`), cần xóa link trước khi xóa container.  