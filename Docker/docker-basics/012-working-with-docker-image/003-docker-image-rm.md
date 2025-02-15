Lệnh `docker rmi` là viết tắt của `docker image rm`, dùng để xóa một hoặc nhiều Docker images khỏi máy.  

---

### **Cú pháp:**  
```sh
docker rmi [OPTIONS] IMAGE [IMAGE...]
```

---

### **Các tùy chọn quan trọng:**  
| Tùy chọn | Mô tả |
|----------|-------|
| `-f`, `--force` | Bắt buộc xóa image ngay cả khi đang được sử dụng bởi container. |
| `--no-prune` | Không xóa các image trung gian chưa được sử dụng. |

---

### **Ví dụ sử dụng:**  

#### 1. **Xóa một image theo tên hoặc ID**  
```sh
docker rmi ubuntu:20.04
docker rmi 3a2e3c7c99ac
```

#### 2. **Xóa nhiều images cùng lúc**  
```sh
docker rmi nginx redis alpine
```

#### 3. **Xóa image đang bị sử dụng (bắt buộc xóa)**  
```sh
docker rmi -f my-image
```
(Nếu không có `-f`, Docker sẽ báo lỗi nếu image đang được container sử dụng.)

#### 4. **Xóa tất cả dangling images (images không có tag)**  
```sh
docker rmi $(docker images -q --filter "dangling=true")
```

#### 5. **Xóa tất cả images (CẨN THẬN ⚠️)**  
```sh
docker rmi -f $(docker images -q)
```

---

💡 **Lưu ý:**  
- Nếu có container đang dùng image, cần **xóa container trước** bằng `docker rm` hoặc dùng `-f`.  
- Lệnh `docker rmi` giống hệt `docker image rm`.  
