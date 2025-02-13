Lệnh `docker rename` được sử dụng để đổi tên một container Docker đang tồn tại.  

---

## 🔹 Cú pháp lệnh  
```sh
docker rename CONTAINER NEW_NAME
```
- `CONTAINER`: Tên hoặc ID của container cần đổi tên.  
- `NEW_NAME`: Tên mới cho container.  

---

## 🔹 Ví dụ sử dụng  

### 1️⃣ Đổi tên container  
Giả sử bạn có một container có tên `old-container` và muốn đổi tên nó thành `new-container`:  
```sh
docker rename old-container new-container
```
📌 **Mô tả**: Container `old-container` sẽ được đổi thành `new-container`.  

📌 **Kiểm tra lại bằng lệnh**:  
```sh
docker ps -a
```
Bạn sẽ thấy container đã có tên mới.  

---

### 2️⃣ Đổi tên container bằng ID  
Bạn có thể đổi tên container bằng ID của nó:  
```sh
docker rename 123abc456def my-renamed-container
```
📌 **Mô tả**: Đổi tên container có ID `123abc456def` thành `my-renamed-container`.  

📌 **Kiểm tra lại bằng lệnh**:  
```sh
docker ps -a --filter "name=my-renamed-container"
```

---

## 🔹 Một số lưu ý khi dùng `docker rename`  
✔ Container có thể đang chạy hoặc đã dừng khi đổi tên.  
✔ Tên mới không được trùng với bất kỳ container nào khác.  
✔ Việc đổi tên không ảnh hưởng đến dữ liệu hoặc trạng thái của container.  