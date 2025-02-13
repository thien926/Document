Lệnh `docker exec` cho phép bạn chạy một lệnh bên trong container Docker đang chạy.  

---

## 🔹 Cú pháp lệnh  
```sh
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
```
- `CONTAINER`: Tên hoặc ID của container.  
- `COMMAND [ARG...]`: Lệnh cần thực thi bên trong container.  
- `[OPTIONS]`: Các tùy chọn để thay đổi cách thực thi lệnh.  

---

## 🔹 Các tùy chọn phổ biến  

| Tùy chọn | Mô tả |
|----------|-------|
| `-i`, `--interactive` | Giữ kết nối đầu vào để tương tác với container. |
| `-t`, `--tty` | Cấp phát một pseudo-TTY (giống terminal). |
| `-d`, `--detach` | Chạy lệnh trong chế độ nền (không cần chờ). |
| `-u`, `--user` | Chạy lệnh với user cụ thể trong container. |
| `--privileged` | Chạy với quyền nâng cao. |
| `--workdir`, `-w` | Chạy lệnh trong thư mục làm việc cụ thể. |

---

## 🔹 Ví dụ sử dụng  

### 1️⃣ Chạy lệnh trong container  
```sh
docker exec my-container echo "Hello from Docker"
```
📌 **Mô tả**: Chạy lệnh `echo "Hello from Docker"` bên trong `my-container`.  

---

### 2️⃣ Mở terminal (bash) trong container  
```sh
docker exec -it my-container bash
```
📌 **Mô tả**:  
- `-i`: Giữ kết nối để tương tác.  
- `-t`: Mở terminal ảo.  
- `bash`: Chạy shell `bash` trong container.  

🔹 **Nếu container không có `bash`, thử dùng `sh`:**  
```sh
docker exec -it my-container sh
```

---

### 3️⃣ Chạy lệnh với quyền root  
```sh
docker exec -u root -it my-container bash
```
📌 **Mô tả**: Chạy `bash` với quyền `root`.  

---

### 4️⃣ Chạy lệnh trong thư mục cụ thể  
```sh
docker exec -w /app -it my-container ls -l
```
📌 **Mô tả**: Chạy `ls -l` trong thư mục `/app` bên trong container.  

---

### 5️⃣ Chạy tiến trình trong chế độ nền  
```sh
docker exec -d my-container touch /tmp/hello.txt
```
📌 **Mô tả**: Tạo file `/tmp/hello.txt` bên trong container mà không hiển thị đầu ra.  

---

### 6️⃣ Kiểm tra thông tin hệ thống của container  
```sh
docker exec my-container uname -a
```
📌 **Mô tả**: Xem thông tin hệ điều hành trong container.  

```sh
docker exec my-container env
```
📌 **Mô tả**: Hiển thị biến môi trường trong container.  

---

## 🔹 Một số lưu ý khi dùng `docker exec`  
✔ Chỉ có thể chạy lệnh trên container **đang chạy**.  
✔ Nếu container không có shell (`bash` hoặc `sh`), bạn cần cài đặt trước.  
✔ Dùng `-u` nếu cần chạy với user cụ thể.  
