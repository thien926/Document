### Docker Registry là gì?  
**Docker Registry** là một hệ thống lưu trữ và phân phối các hình ảnh Docker (**Docker images**). Nó đóng vai trò như một kho chứa tập trung, giúp bạn lưu trữ, quản lý, chia sẻ và phân phối Docker images giữa các môi trường khác nhau.

---

## 📌 **Tổng quan về Docker Registry**
Docker Registry có thể là:
1. **Docker Hub (Public Registry)** – Registry mặc định của Docker, do Docker Inc. cung cấp.
2. **Amazon Elastic Container Registry (ECR), Google Container Registry (GCR), Azure Container Registry (ACR)** – Các dịch vụ cloud của AWS, Google Cloud, Azure cung cấp.
3. **Private Docker Registry** – Một registry riêng, có thể tự host bằng `docker registry` hoặc dùng giải pháp như **Harbor**, **Nexus**, **GitHub Container Registry**, v.v.

---

## 🔍 **Cấu trúc của Docker Registry**
Docker Registry lưu trữ các Docker images dưới dạng **repositories**.

📌 Ví dụ về một Docker image trong Registry:  
```
docker.io/library/nginx:latest
```
- `docker.io`: Registry (Docker Hub).
- `library/nginx`: Repository chứa image.
- `latest`: Tag của image.

---

## 🚀 **Các thành phần liên quan**
### 1. **Docker Registry**
- Hệ thống lưu trữ và quản lý các Docker images.
- Có thể tự host hoặc sử dụng các giải pháp có sẵn.

### 2. **Docker Repository**
- Là một tập hợp các phiên bản của một image.
- Ví dụ: `nginx` repository chứa nhiều phiên bản như `nginx:1.21`, `nginx:latest`, `nginx:alpine`.

### 3. **Docker Image**
- Là một tệp tin có thể thực thi, chứa ứng dụng và các dependencies.
- Được đẩy (push) lên registry hoặc kéo (pull) xuống khi cần.

---

## 🔧 **Cách sử dụng Docker Registry**
### 🏗 **1. Push image lên Docker Hub**
```sh
docker login  # Đăng nhập vào Docker Hub
docker tag my-app myusername/my-app:v1  # Gán tag cho image
docker push myusername/my-app:v1  # Push image lên registry
```

### 📥 **2. Pull image từ Docker Hub**
```sh
docker pull myusername/my-app:v1
```

### 🏗 **3. Tạo Private Docker Registry**
Tạo một **private registry** chạy cục bộ:
```sh
docker run -d -p 5000:5000 --name my-registry registry:2
```
Push image vào registry riêng:
```sh
docker tag my-app localhost:5000/my-app
docker push localhost:5000/my-app
```
Pull lại image:
```sh
docker pull localhost:5000/my-app
```

---

## 📊 **So sánh Public Registry & Private Registry**
| Tiêu chí           | Public Registry (Docker Hub) | Private Registry |
|-------------------|-----------------------------|-----------------|
| **Bảo mật**      | Public (trừ khi trả phí)    | Private (tự quản lý) |
| **Chi phí**      | Miễn phí (giới hạn)         | Tốn tài nguyên (server) |
| **Tốc độ**       | Có thể chậm khi tải cao     | Tùy thuộc vào hạ tầng |
| **Dễ sử dụng**   | Dễ dùng, có UI              | Cần cài đặt, cấu hình |

---

## ✅ **Khi nào nên dùng Docker Registry?**
- Dùng **Docker Hub** nếu cần chia sẻ nhanh chóng và không quan trọng về bảo mật.
- Dùng **Private Registry** nếu cần kiểm soát, bảo mật và có yêu cầu riêng (CI/CD pipeline, Kubernetes, v.v.).