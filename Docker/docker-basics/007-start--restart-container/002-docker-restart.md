Lệnh `docker restart` được sử dụng để khởi động lại một hoặc nhiều container đang chạy hoặc đã dừng. Khi thực thi lệnh này, Docker sẽ dừng container (nếu đang chạy) và sau đó khởi động lại nó. citeturn0search0

### Cú pháp

```sh
docker restart [OPTIONS] CONTAINER [CONTAINER...]
```

- **CONTAINER**: ID hoặc tên của container cần khởi động lại.

### Các tùy chọn

- **`-t, --time`**: Thiết lập thời gian chờ (tính bằng giây) trước khi buộc dừng container. Nếu container không dừng sau khoảng thời gian này, Docker sẽ gửi tín hiệu `SIGKILL` để buộc dừng. Mặc định là 10 giây.

- **`-s, --signal`**: Xác định tín hiệu sẽ gửi đến container để yêu cầu dừng. Mặc định là `SIGTERM`. Bạn có thể chỉ định tín hiệu khác như `SIGINT` hoặc `SIGQUIT`.

### Ví dụ

1. **Khởi động lại một container với tên cụ thể:**

   ```sh
   docker restart my_container
   ```

   Lệnh này sẽ dừng và sau đó khởi động lại container có tên `my_container`.

2. **Khởi động lại một container với thời gian chờ tùy chỉnh:**

   ```sh
   docker restart -t 20 my_container
   ```

   Lệnh này gửi tín hiệu `SIGTERM` đến `my_container` và chờ 20 giây trước khi gửi tín hiệu `SIGKILL` nếu container chưa dừng.

3. **Khởi động lại nhiều container cùng lúc:**

   ```sh
   docker restart container1 container2 container3
   ```

   Lệnh này sẽ dừng và khởi động lại các container có tên `container1`, `container2` và `container3`.

4. **Khởi động lại tất cả các container đang chạy:**

   ```sh
   docker restart $(docker ps -q)
   ```

   Lệnh này lấy danh sách ID của tất cả các container đang chạy và khởi động lại chúng.

Lưu ý rằng việc khởi động lại container có thể hữu ích khi bạn muốn áp dụng các thay đổi cấu hình hoặc khắc phục các sự cố tạm thời mà không cần phải tạo lại container. 