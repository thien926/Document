Lệnh `docker stop` được sử dụng để dừng một hoặc nhiều container đang chạy. Khi thực thi lệnh này, Docker gửi tín hiệu `SIGTERM` đến tiến trình chính bên trong container để yêu cầu nó dừng. Nếu sau một khoảng thời gian chờ (mặc định là 10 giây), container vẫn chưa dừng, Docker sẽ gửi tín hiệu `SIGKILL` để buộc dừng container. citeturn0search0

### Cú pháp

```sh
docker stop [OPTIONS] CONTAINER [CONTAINER...]
```

- **CONTAINER**: ID hoặc tên của container cần dừng.

### Các tùy chọn

- **`-t, --time`**: Đặt thời gian chờ (tính bằng giây) trước khi buộc dừng container. Nếu container không dừng trong khoảng thời gian này sau khi nhận tín hiệu `SIGTERM`, Docker sẽ gửi tín hiệu `SIGKILL`. Mặc định là 10 giây.

- **`-s, --signal`**: Xác định tín hiệu sẽ gửi đến container để yêu cầu dừng. Mặc định là `SIGTERM`. Bạn có thể chỉ định tín hiệu khác như `SIGINT` hoặc `SIGQUIT`.

### Ví dụ

1. **Dừng một container với tên cụ thể:**

   ```sh
   docker stop my_container
   ```

   Lệnh này gửi tín hiệu `SIGTERM` đến container có tên `my_container`. Nếu container không dừng sau 10 giây, Docker sẽ gửi tín hiệu `SIGKILL` để buộc dừng.

2. **Dừng một container với thời gian chờ tùy chỉnh:**

   ```sh
   docker stop -t 20 my_container
   ```

   Lệnh này gửi tín hiệu `SIGTERM` đến `my_container` và chờ 20 giây trước khi gửi tín hiệu `SIGKILL` nếu container chưa dừng.

3. **Dừng một container bằng tín hiệu khác:**

   ```sh
   docker stop -s SIGINT my_container
   ```

   Lệnh này gửi tín hiệu `SIGINT` đến `my_container` thay vì `SIGTERM`.

4. **Dừng nhiều container cùng lúc:**

   ```sh
   docker stop container1 container2 container3
   ```

   Lệnh này gửi tín hiệu dừng đến các container có tên `container1`, `container2` và `container3`.

5. **Dừng tất cả các container đang chạy:**

   ```sh
   docker stop $(docker ps -q)
   ```

   Lệnh này lấy danh sách ID của tất cả các container đang chạy và dừng chúng.

Lưu ý rằng việc sử dụng tín hiệu phù hợp và thời gian chờ hợp lý giúp đảm bảo các tiến trình bên trong container có thể hoàn tất công việc hiện tại và giải phóng tài nguyên một cách an toàn trước khi dừng. 