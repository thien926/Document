### Docker Image là gì?  
Docker Image là một **mẫu (template) bất biến** chứa tất cả những gì cần thiết để chạy một ứng dụng, bao gồm:  
- **Mã nguồn** của ứng dụng  
- **Thư viện, dependencies**  
- **Cấu hình môi trường**  
- **Công cụ hệ thống**  

Docker Image hoạt động như một bản "snapshot" của một container. Khi bạn chạy một container, nó sẽ tạo ra một bản sao từ image và chạy nó trong môi trường cô lập.  

---

### 🔥 Điểm quan trọng về Docker Image:
1. **Bất biến (Immutable)**  
   - Khi đã tạo Docker Image, bạn không thể thay đổi nó trực tiếp. Nếu cần thay đổi, bạn phải tạo một image mới.  

2. **Layered (Tầng lớp)**  
   - Mỗi Docker Image được xây dựng từ nhiều **lớp (layers)**.  
   - Khi bạn thay đổi một file trong image, Docker chỉ tạo một layer mới thay vì sao chép toàn bộ image. Điều này giúp **tối ưu dung lượng và tốc độ**.  

3. **Lưu trữ trên Docker Hub hoặc Private Registry**  
   - Bạn có thể lưu trữ Docker Image trên Docker Hub hoặc các registry riêng như AWS ECR, GitHub Container Registry, hoặc GitLab Container Registry.  

---

### 🛠 Ví dụ: Docker Image trong thực tế
Giả sử bạn có một ứng dụng **Node.js** và muốn đóng gói nó thành Docker Image.  

#### **1️⃣ Viết Dockerfile**
```dockerfile
# Sử dụng image Node.js chính thức từ Docker Hub
FROM node:18

# Thiết lập thư mục làm việc
WORKDIR /app

# Copy mã nguồn vào container
COPY . .

# Cài đặt dependencies
RUN npm install

# Mở cổng 3000
EXPOSE 3000

# Chạy ứng dụng
CMD ["node", "server.js"]
```

#### **2️⃣ Xây dựng Docker Image**
Chạy lệnh:
```sh
docker build -t my-node-app .
```
→ Docker sẽ tạo một image có tên **my-node-app**.

#### **3️⃣ Chạy container từ image**
```sh
docker run -p 3000:3000 my-node-app
```
→ Chạy container và ứng dụng có thể truy cập tại `http://localhost:3000`.

---

### 📌 Tổng kết:
- Docker Image là một **bản mẫu (template) bất biến** giúp tạo container.  
- Nó bao gồm **mã nguồn, thư viện, config**, và các công cụ hệ thống.  
- Được xây dựng từ nhiều **layer** để tối ưu dung lượng.  
- Có thể lưu trữ trên Docker Hub hoặc registry riêng.  
- Sử dụng Dockerfile để tạo image theo yêu cầu.
