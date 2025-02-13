Lệnh `docker start` được sử dụng để khởi động một hoặc nhiều container đã được tạo hoặc đã dừng trước đó. Lệnh này không tạo container mới mà chỉ khởi động lại các container hiện có. 

### Cú pháp

```sh
docker start [OPTIONS] CONTAINER [CONTAINER...]
```

- **CONTAINER**: ID hoặc tên của container cần khởi động.

### Các tùy chọn

- **`-a, --attach`**: Đính kèm đầu vào và đầu ra của container vào terminal hiện tại.
- **`-i, --interactive`**: Kết hợp với `--attach` để cho phép tương tác với container thông qua đầu vào chuẩn (STDIN).

### Ví dụ

1. **Khởi động một container:**

   ```sh
   docker start my_container
   ```

   Lệnh này khởi động container có tên `my_container`.

2. **Khởi động nhiều container cùng lúc:**

   ```sh
   docker start container1 container2 container3
   ```

   Lệnh này khởi động các container có tên `container1`, `container2` và `container3`.

3. **Khởi động container và đính kèm đầu ra vào terminal:**

   ```sh
   docker start -a my_container
   ```

   Lệnh này khởi động `my_container` và hiển thị đầu ra của nó trong terminal hiện tại.

4. **Khởi động container với chế độ tương tác:**

   ```sh
   docker start -a -i my_container
   ```

   Lệnh này khởi động `my_container`, đính kèm đầu ra và cho phép tương tác thông qua đầu vào chuẩn.

Lưu ý rằng nếu container được cấu hình để tự động khởi động lại (sử dụng tùy chọn `--restart` khi tạo container), Docker sẽ tự động khởi động lại container mà không cần sử dụng lệnh `docker start`. 