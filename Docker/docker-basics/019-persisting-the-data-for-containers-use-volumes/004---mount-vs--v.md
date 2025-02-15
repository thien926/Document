### **So sánh `--mount` vs `-v` trong Docker**  
Bạn có thể dùng cả **`-v`** và **`--mount`** để gắn volume vào container, nhưng chúng có một số khác biệt.  

#### **📌 Cùng một chức năng nhưng cú pháp khác:**  
Lệnh sử dụng `-v`:  
```sh
docker run -dp 127.0.0.1:3000:3000 -v todo-db:/etc/todos getting-started
```
Lệnh sử dụng `--mount`:  
```sh
docker run -dp 127.0.0.1:3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started
```

🔹 **Cả hai cách này đều gắn volume `todo-db` vào `/etc/todos`, nhưng khác nhau về cách sử dụng.**

---

## 🔹 **So sánh `-v` vs `--mount`**  
| **Đặc điểm**  | **`-v` (volume shorthand)** | **`--mount` (cú pháp rõ ràng hơn)** |
|--------------|-----------------|-----------------------|
| **Cách sử dụng** | `-v volume_name:/path/in/container` | `--mount type=volume,src=volume_name,target=/path/in/container` |
| **Dễ nhớ** | Ngắn gọn, dễ viết | Dài hơn nhưng rõ ràng |
| **Cấu hình chi tiết** | Không hỗ trợ các tùy chọn nâng cao như `readonly`, `propagation`, `bind options` | Hỗ trợ nhiều tùy chọn nâng cao |
| **Khả năng mở rộng** | Ít tùy chỉnh hơn | Hỗ trợ nhiều loại mount: `volume`, `bind`, `tmpfs` |
| **Quản lý bằng Docker Compose** | Thường được dùng (`volumes:`) | Ít phổ biến hơn trong Compose |

📌 **Về hiệu suất, cả hai cách đều giống nhau!** 🚀  

---

## 🔹 **Khi nào nên dùng gì?**  

| **Trường hợp** | **Dùng `-v`** | **Dùng `--mount`** |
|--------------|-------------|------------------|
| **Muốn gõ nhanh** | ✅ | ❌ |
| **Muốn đọc dễ dàng, rõ ràng** | ❌ | ✅ |
| **Cần tùy chỉnh nâng cao** (readonly, bind options...) | ❌ | ✅ |
| **Dùng trong Docker Compose** | ✅ | ❌ |

---

## 🔹 **Ví dụ về `readonly` chỉ dùng với `--mount`**  
Bạn không thể làm như này với `-v`:  
```sh
docker run -d --mount type=volume,src=todo-db,target=/etc/todos,readonly getting-started
```
Lệnh trên sẽ **gắn volume `todo-db` vào `/etc/todos` nhưng chỉ đọc (`readonly`)**, không cho phép ghi dữ liệu vào.

---

### **🚀 Kết luận**
- Dùng **`-v`** nếu bạn cần **cú pháp ngắn gọn, đơn giản**.  
- Dùng **`--mount`** nếu bạn cần **cấu hình chi tiết hơn (readonly, bind options, tmpfs...)**.  
- Về hiệu suất, **không có sự khác biệt** giữa hai cách.  