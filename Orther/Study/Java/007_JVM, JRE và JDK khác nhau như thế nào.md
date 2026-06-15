# JVM, JRE và JDK khác nhau như thế nào?

Trong hệ sinh thái Java, ba thành phần quan trọng nhất là **JVM**, **JRE** và **JDK**. Chúng có mối quan hệ kế thừa với nhau:

* **JVM** là thành phần dùng để thực thi chương trình Java.
* **JRE** chứa JVM và các thư viện cần thiết để chạy ứng dụng.
* **JDK** chứa JRE và các công cụ phục vụ việc phát triển phần mềm Java.

Có thể hình dung:

```
JDK
 └── JRE
      └── JVM
```

---

# 1. JVM (Java Virtual Machine)

## Khái niệm

JVM (Java Virtual Machine) là máy ảo Java, chịu trách nhiệm thực thi các tập tin bytecode (`.class`).

Khi mã nguồn Java được biên dịch, nó không chạy trực tiếp mà được chuyển thành bytecode. JVM sẽ đọc bytecode này và thực thi trên hệ điều hành tương ứng.

---

## Quá trình thực thi

### Bước 1: Viết mã nguồn

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```

File:

```
HelloWorld.java
```

---

### Bước 2: Biên dịch

Sử dụng:

```bash
javac HelloWorld.java
```

Kết quả:

```
HelloWorld.class
```

Đây là file bytecode.

---

### Bước 3: JVM thực thi

Chạy:

```bash
java HelloWorld
```

JVM sẽ đọc:

```
HelloWorld.class
```

và xuất ra:

```
Hello World
```

---

## Vai trò của JVM

* Thực thi bytecode.
* Cho phép chương trình Java chạy trên nhiều hệ điều hành khác nhau.
* Là thành phần cốt lõi giúp Java đạt được nguyên tắc:

> Write Once, Run Anywhere.

---

# 2. JRE (Java Runtime Environment)

## Khái niệm

JRE (Java Runtime Environment) là môi trường chạy ứng dụng Java.

JRE bao gồm:

* JVM.
* Các thư viện chuẩn cần thiết để chương trình Java hoạt động.

Có thể biểu diễn:

```
JRE
 ├── JVM
 └── Java Libraries
```

---

## Chức năng

JRE cung cấp đầy đủ những gì cần thiết để chạy một chương trình Java đã được biên dịch.

Ví dụ:

Một người dùng chỉ cần chạy chương trình Java mà không cần lập trình.

Chỉ cần cài:

```
JRE
```

Sau đó chạy:

```bash
java HelloWorld
```

mà không cần công cụ biên dịch.

---

## Thành phần của JRE

### JVM

Dùng để thực thi bytecode.

### Các thư viện Java

Ví dụ:

```java
System.out.println();
```

```java
String
```

```java
ArrayList
```

```java
HashMap
```

Những lớp này nằm trong thư viện chuẩn được cung cấp bởi JRE.

---

# 3. JDK (Java Development Kit)

## Khái niệm

JDK (Java Development Kit) là bộ công cụ dùng để phát triển ứng dụng Java.

JDK bao gồm:

* JRE.
* Các công cụ hỗ trợ lập trình.

Biểu diễn:

```
JDK
 ├── JRE
 │    ├── JVM
 │    └── Libraries
 │
 ├── javac
 ├── jar
 ├── javadoc
 └── debugger
```

---

## Các công cụ quan trọng trong JDK

### javac

Dùng để biên dịch mã nguồn Java thành bytecode.

Ví dụ:

```bash
javac HelloWorld.java
```

Sinh ra:

```
HelloWorld.class
```

---

### jar

Đóng gói nhiều file class thành một file JAR.

Ví dụ:

```bash
jar cf app.jar *.class
```

---

### javadoc

Sinh tài liệu từ mã nguồn Java.

Ví dụ:

```bash
javadoc HelloWorld.java
```

---

### debugger

Hỗ trợ gỡ lỗi chương trình.

Cho phép:

* Đặt breakpoint.
* Theo dõi giá trị biến.
* Thực thi từng bước.

---

## Khi nào cần JDK?

Khi:

* Viết chương trình Java.
* Biên dịch mã nguồn.
* Build project.
* Debug ứng dụng.

Ví dụ:

Một lập trình viên Java cần cài:

```
JDK
```

vì trong JDK đã chứa:

* JRE.
* JVM.
* Các công cụ phát triển.

---

# Mối quan hệ giữa JVM, JRE và JDK

```
JDK
│
├── JRE
│     │
│     ├── JVM
│     └── Java Libraries
│
├── javac
├── jar
├── javadoc
└── debugger
```

Hoặc có thể hiểu đơn giản:

| Thành phần | Chức năng                                       |
| ---------- | ----------------------------------------------- |
| JVM        | Máy thực thi bytecode                           |
| JRE        | Môi trường chạy ứng dụng Java                   |
| JDK        | Bộ công cụ để phát triển và build ứng dụng Java |

---

# Ví dụ thực tế

Giả sử có file:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Java");
    }
}
```

### Bước 1: Dùng JDK để biên dịch

```bash
javac Main.java
```

Sinh ra:

```text
Main.class
```

---

### Bước 2: Dùng JRE để chạy

```bash
java Main
```

---

### Bước 3: JVM thực thi bytecode

JVM đọc:

```text
Main.class
```

và in ra:

```text
Java
```

---

# Tổng kết

* **JVM (Java Virtual Machine)** là máy ảo Java, chịu trách nhiệm thực thi các file bytecode (`.class`).
* **JRE (Java Runtime Environment)** bao gồm **JVM** và các thư viện cần thiết để chạy ứng dụng Java.
* **JDK (Java Development Kit)** bao gồm **JRE** và các công cụ phát triển như `javac`, `jar`, `javadoc`, `debugger`.
* Mối quan hệ giữa chúng:

```text
JDK
 └── JRE
      └── JVM
```

* Có thể ghi nhớ đơn giản:

> **JVM là máy chạy, JRE là môi trường chạy, JDK là bộ công cụ để lập trình và build ứng dụng Java.**
