# Tại sao Java được gọi là ngôn ngữ đa nền tảng?

Java được gọi là **ngôn ngữ đa nền tảng (platform-independent)** vì chương trình Java **không được biên dịch trực tiếp thành mã máy của một hệ điều hành cụ thể**, mà được biên dịch thành **bytecode**. Bytecode này sau đó được thực thi bởi **JVM (Java Virtual Machine)** trên từng nền tảng.

## Cơ chế hoạt động

Quá trình thực thi của Java diễn ra như sau:

```
Mã nguồn Java (.java)
        ↓
Trình biên dịch javac
        ↓
Bytecode (.class)
        ↓
JVM của từng hệ điều hành
        ↓
Mã máy tương ứng
        ↓
Chương trình được thực thi
```

Điều này có nghĩa là:

* Một lần biên dịch (`Compile Once`).
* Có thể chạy trên nhiều hệ điều hành khác nhau (`Run Anywhere`).

Đây chính là triết lý nổi tiếng của Java:

> **Write Once, Run Anywhere (WORA)**
> Viết một lần, chạy ở mọi nơi.

---

## Ví dụ

Giả sử bạn viết chương trình:

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello Java");
    }
}
```

Biên dịch:

```bash
javac HelloWorld.java
```

Sinh ra:

```
HelloWorld.class
```

File `HelloWorld.class` chứa **bytecode**.

Nếu sao chép file này sang:

* Windows (có JVM)
* Linux (có JVM)
* macOS (có JVM)

thì đều có thể chạy bằng:

```bash
java HelloWorld
```

và nhận được kết quả:

```
Hello Java
```

mà **không cần biên dịch lại chương trình**.

---

## Vai trò của JVM

JVM đóng vai trò như một lớp trung gian giữa chương trình Java và hệ điều hành:

```
                Chương trình Java
                        ↓
                   Bytecode (.class)
                        ↓
               +-----------------+
               |       JVM       |
               +-----------------+
                  ↓          ↓
             Windows      Linux
                  ↓          ↓
               Mã máy      Mã máy
```

Mỗi hệ điều hành sẽ có một JVM riêng:

* JVM cho Windows
* JVM cho Linux
* JVM cho macOS

Nhờ đó cùng một bytecode có thể được thực thi trên nhiều nền tảng khác nhau.

---

## So sánh với C/C++

### C/C++

```
Source Code
     ↓
Compiler
     ↓
Executable (.exe)
     ↓
Windows
```

File `.exe` tạo ra cho Windows thường không chạy được trên Linux hoặc macOS và phải biên dịch lại.

### Java

```
Source Code
     ↓
javac
     ↓
Bytecode (.class)
     ↓
JVM
     ↓
Windows / Linux / macOS
```

Chỉ cần JVM phù hợp là chương trình có thể chạy trên nhiều hệ điều hành.

---

## Kết luận

Java được gọi là **ngôn ngữ đa nền tảng** vì:

1. Mã nguồn Java được biên dịch thành **bytecode**, không phụ thuộc vào hệ điều hành.
2. **JVM** trên từng nền tảng sẽ thực thi bytecode và chuyển đổi thành mã máy tương ứng.

Nhờ vậy cùng một chương trình Java có thể chạy trên Windows, Linux, macOS nếu có JVM tương ứng.

Vì vậy, Java nổi tiếng với khẩu hiệu:

> **Write Once, Run Anywhere (WORA) – Viết một lần, chạy ở mọi nơi.**
