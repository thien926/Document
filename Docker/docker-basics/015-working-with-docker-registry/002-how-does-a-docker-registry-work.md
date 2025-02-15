## 🚀 **Cách Docker Registry Hoạt Động**  
Docker Registry hoạt động theo mô hình **Client - Server**, trong đó **Docker CLI** (Client) sẽ giao tiếp với **Docker Registry** (Server) để quản lý các Docker images.

---

## 🏗 **Kiến trúc tổng quan**  
Docker Registry có 3 thành phần chính:
1. **Docker Client (Docker CLI / Docker Daemon)**
   - Thực hiện lệnh `pull`, `push`, `login`, `logout` để làm việc với Docker Registry.
2. **Docker Registry**
   - Lưu trữ Docker images và cung cấp API để client tương tác.
   - Có thể là **Docker Hub**, **AWS ECR**, **GCR**, **ACR** hoặc private registry.
3. **Docker Index (Optional)**
   - Là hệ thống quản lý thông tin metadata về images, user authentication.
   - Docker Hub có Docker Index riêng.

📌 **Luồng hoạt động của Docker Registry:**
1. **Push Image:** Docker Client gửi image lên Registry.
2. **Pull Image:** Docker Client tải image từ Registry về.
3. **Authentication:** Nếu Registry yêu cầu đăng nhập, Client cần xác thực.
4. **Image Layering & Caching:** Registry tối ưu việc lưu trữ bằng cách sử dụng các lớp (layers).

---

## 🔄 **Quy trình hoạt động chi tiết**
### 1️⃣ **Push Docker Image lên Registry**
**Luồng hoạt động khi `docker push` một image:**
1. Docker Client kiểm tra Registry (`docker.io`, `localhost:5000`, AWS ECR, v.v.).
2. Nếu Registry yêu cầu xác thực (`private registry`), Docker Client gửi **access token**.
3. Image được chia thành nhiều **layers** (base layer, app layer, v.v.).
4. Docker Client kiểm tra nếu layer nào đã có trong Registry thì bỏ qua.
5. Các layers mới được **upload** lên Registry.
6. Registry cập nhật **metadata** của image.

📌 **Ví dụ:**
```sh
docker login
docker tag my-app myrepo/my-app:v1
docker push myrepo/my-app:v1
```

---

### 2️⃣ **Pull Docker Image từ Registry**
**Luồng hoạt động khi `docker pull` một image:**
1. Docker Client gửi yêu cầu `GET` đến Registry để lấy metadata của image.
2. Registry trả về danh sách các **layers** của image.
3. Docker Client kiểm tra cache để tránh tải lại các layers có sẵn.
4. Các layers còn thiếu sẽ được tải về.
5. Docker Daemon lắp ráp các layers thành một container image.

📌 **Ví dụ:**
```sh
docker pull myrepo/my-app:v1
```

---

### 3️⃣ **Cấu trúc lưu trữ của Docker Registry**
Docker Registry lưu trữ images dưới dạng các **content-addressable layers**, nghĩa là:
- Mỗi image gồm nhiều **layers** (các lớp).
- Mỗi layer được lưu bằng **SHA256 hash** để tránh trùng lặp.
- Nếu nhiều images có chung layer, Registry chỉ lưu **một bản duy nhất**.

📌 **Ví dụ về cấu trúc lưu trữ**:
```
/var/lib/registry/docker/registry/v2/
├── blobs/sha256/
│   ├── 0a/          # Layer 1
│   ├── f5/          # Layer 2
│   ├── 6c/          # Layer 3
│   └── manifest/    # Metadata của image
```

---

## 🔐 **Bảo mật trong Docker Registry**
Docker Registry hỗ trợ:
- **Basic Authentication**: Dùng username/password (`docker login`).
- **Token-based Authentication**: Các cloud registry (ECR, GCR, ACR) sử dụng token.
- **TLS Encryption (HTTPS)**: Bảo vệ dữ liệu truyền tải.
- **Image Signing & Verification**: Kiểm tra tính toàn vẹn của image.

📌 **Ví dụ chạy Private Registry có bảo mật bằng TLS:**
```sh
docker run -d -p 5000:5000 \
  -v /certs:/certs \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  --name secure-registry registry:2
```

---

## 🛠 **Tích hợp Docker Registry với CI/CD**
Docker Registry thường được tích hợp với:
- **GitHub Actions / GitLab CI** → Build & push images tự động.
- **Kubernetes (K8s)** → Lưu trữ images cho deployments.
- **Harbor, Nexus** → Quản lý images trong doanh nghiệp.

📌 **Ví dụ: CI/CD pipeline push image lên Docker Hub**
```yaml
jobs:
  build:
    steps:
      - run: docker login -u $DOCKER_USER -p $DOCKER_PASSWORD
      - run: docker build -t myrepo/my-app:v1 .
      - run: docker push myrepo/my-app:v1
```

---

## 🚀 **Tóm tắt**
| Thành phần  | Chức năng |
|------------|----------|
| **Docker Client** | Gửi yêu cầu `pull` / `push` images |
| **Docker Registry** | Lưu trữ, quản lý Docker images |
| **Docker Index** | (Tuỳ chọn) Quản lý metadata & xác thực user |
| **Image Layers** | Các lớp của image, tối ưu dung lượng |

Docker Registry giúp quản lý Docker images hiệu quả, hỗ trợ triển khai nhanh và tích hợp với CI/CD.