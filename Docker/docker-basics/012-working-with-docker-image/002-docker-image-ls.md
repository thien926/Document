Lệnh `docker images` là một alias của `docker image ls`, dùng để liệt kê các Docker images có sẵn trên máy.  

---

### **Cú pháp:**  
```sh
docker images [OPTIONS]
```

---

### **Các tùy chọn quan trọng:**  

| Tùy chọn | Mô tả |
|----------|-------|
| `-a`, `--all` | Hiển thị tất cả images, bao gồm cả intermediate images. |
| `-q`, `--quiet` | Chỉ hiển thị Image ID. |
| `--filter` | Lọc danh sách images theo điều kiện cụ thể. |
| `--format` | Hiển thị thông tin theo định dạng tùy chỉnh. |
| `--no-trunc` | Không cắt ngắn thông tin đầu ra. |

---

### **Ví dụ sử dụng:**  

#### 1. **Liệt kê tất cả images có sẵn**  
```sh
docker images
```

#### 2. **Liệt kê tất cả images, bao gồm cả intermediate images**  
```sh
docker images -a
```

#### 3. **Chỉ hiển thị Image ID**  
```sh
docker images -q
```

#### 4. **Lọc theo repository name**  
```sh
docker images --filter "reference=ubuntu"
```

#### 5. **Lọc chỉ hiển thị dangling images**  
```sh
docker images --filter "dangling=true"
```

#### 6. **Hiển thị danh sách images theo định dạng tùy chỉnh**  
```sh
docker images --format "{{.Repository}}:{{.Tag}} ({{.Size}})"
```
Ví dụ kết quả:
```
nginx:latest (133MB)
ubuntu:20.04 (72MB)
```

---

💡 **Lưu ý:**  
- Lệnh `docker images` tương đương với `docker image ls`.  
- Nếu bạn muốn quản lý images, có thể kết hợp với `docker rmi` để xóa.  