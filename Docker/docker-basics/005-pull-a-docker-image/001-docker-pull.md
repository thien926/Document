Để tải một Docker image về máy của bạn, bạn có thể sử dụng lệnh `docker pull`. Cú pháp chung của lệnh này như sau:

```bash
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
```

- **NAME**: Tên của image bạn muốn tải. Nếu bạn không chỉ định tên miền, Docker sẽ mặc định sử dụng Docker Hub.
- **TAG**: Thẻ của image, thường được sử dụng để chỉ định phiên bản cụ thể. Nếu không chỉ định, Docker sẽ mặc định sử dụng thẻ `latest`.
- **DIGEST**: Hàm băm của image, được sử dụng để chỉ định một phiên bản cụ thể dựa trên nội dung của nó.

**Ví dụ 1**: Tải image `nginx` với thẻ `latest` (mặc định):

```bash
docker pull nginx
```

**Ví dụ 2**: Tải image `nginx` với thẻ `1.21.6`:

```bash
docker pull nginx:1.21.6
```

**Ví dụ 3**: Tải image `nginx` bằng digest:

```bash
docker pull nginx@sha256:5a3ba9c0f3f1b2b1a5b1a5b1a5b1a5b1a5b1a5b1a5b1a5b1a5b1a5b1a5b1a5b1
```

**Tùy chọn phổ biến**:

- `--all-tags` hoặc `-a`: Tải tất cả các thẻ của image.

  ```bash
  docker pull -a nginx
  ```

- `--quiet` hoặc `-q`: Giảm thiểu đầu ra của lệnh, chỉ hiển thị ID của image sau khi tải xong.

  ```bash
  docker pull -q nginx
  ```

**Lưu ý**: Trước khi tải image, hãy đảm bảo rằng bạn đã cài đặt Docker và Docker daemon đang chạy trên hệ thống của bạn. Nếu bạn gặp lỗi quyền truy cập, hãy kiểm tra xem người dùng của bạn có thuộc nhóm `docker` hay không. Bạn có thể tham khảo các bước sau cài đặt Docker trên Linux để biết thêm chi tiết.