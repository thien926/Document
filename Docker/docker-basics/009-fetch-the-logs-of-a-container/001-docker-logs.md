Lệnh `docker logs` dùng để xem log của một container đang chạy hoặc đã dừng. Dưới đây là hướng dẫn chi tiết và cụ thể về cách sử dụng nó.

---

## 🔹 Cú pháp lệnh
```sh
docker logs [OPTIONS] CONTAINER
```
- `CONTAINER`: Tên hoặc ID của container mà bạn muốn xem log.
- `[OPTIONS]`: Các tùy chọn để lọc hoặc hiển thị log theo nhu cầu.

---

## 🔹 Các tùy chọn phổ biến

| Tùy chọn | Mô tả |
|----------|-------|
| `--follow`, `-f` | Theo dõi log theo thời gian thực (giống `tail -f`) |
| `--since TIME` | Chỉ hiển thị log từ một thời điểm nhất định |
| `--until TIME` | Chỉ hiển thị log đến một thời điểm nhất định |
| `--timestamps`, `-t` | Hiển thị timestamp cho từng dòng log |
| `--tail N` | Chỉ hiển thị `N` dòng log cuối cùng (mặc định là tất cả) |

---

## 🔹 Ví dụ sử dụng

### 1️⃣ Xem toàn bộ log của container
```sh
docker logs my-container
```
📌 **Mô tả**: Hiển thị toàn bộ log của container có tên `my-container`.

---

### 2️⃣ Theo dõi log theo thời gian thực
```sh
docker logs -f my-container
```
📌 **Mô tả**: Giống `tail -f`, theo dõi log đang được ghi.

---

### 3️⃣ Xem log có timestamp
```sh
docker logs -t my-container
```
📌 **Mô tả**: Hiển thị timestamp của từng dòng log.

📌 **Ví dụ đầu ra**:
```
2025-02-13T12:00:01Z Starting server...
2025-02-13T12:00:05Z Server running on port 8080
```

---

### 4️⃣ Xem log từ một thời điểm nhất định
```sh
docker logs --since 10m my-container
```
📌 **Mô tả**: Chỉ hiển thị log trong 10 phút gần nhất.

🔹 Hỗ trợ các định dạng thời gian:
- `10m` (10 phút)
- `1h` (1 giờ)
- `2025-02-13T12:00:00` (thời gian cụ thể theo ISO 8601)

---

### 5️⃣ Xem log từ một thời điểm đến một thời điểm
```sh
docker logs --since "2025-02-13T12:00:00" --until "2025-02-13T12:30:00" my-container
```
📌 **Mô tả**: Chỉ hiển thị log trong khoảng thời gian từ `12:00` đến `12:30`.

---

### 6️⃣ Chỉ xem `N` dòng log cuối cùng
```sh
docker logs --tail 100 my-container
```
📌 **Mô tả**: Hiển thị 100 dòng log cuối cùng.

---

### 7️⃣ Kết hợp nhiều tùy chọn
```sh
docker logs -f --tail 50 -t my-container
```
📌 **Mô tả**:
- Hiển thị 50 dòng log cuối cùng
- Kèm timestamp
- Theo dõi log theo thời gian thực

---

## 🔹 Một số mẹo hữu ích

### 📌 Ghi log vào file
Nếu muốn lưu log vào file, bạn có thể dùng:
```sh
docker logs my-container > container.log
```
📌 **Mô tả**: Ghi log của container vào file `container.log`.

---

### 📌 Tích hợp với `grep` để lọc log
Ví dụ: Tìm tất cả lỗi trong log của container:
```sh
docker logs my-container | grep "ERROR"
```

---

### 📌 Xem log của tất cả container đang chạy
```sh
docker ps -q | xargs docker logs --tail 10
```
📌 **Mô tả**: Lấy log 10 dòng cuối của tất cả container đang chạy.

---

## 🔹 Tổng kết
- `docker logs CONTAINER` để xem log.
- `-f` để theo dõi log theo thời gian thực.
- `--since` và `--until` để lọc log theo thời gian.
- `--tail N` để lấy số dòng log cuối cùng.
- `-t` để hiển thị timestamp.