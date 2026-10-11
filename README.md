# Bài 5: Khởi chạy dịch vụ đơn giản với Docker Compose

## 1. Mục tiêu

- Làm quen với Docker Compose CLI v2.
- Biết cách khai báo và quản lý dịch vụ bằng file `docker-compose.yml`.
- Khởi chạy một dịch vụ web Nginx bằng Docker Compose.
- Kiểm tra trạng thái container và truy cập dịch vụ thông qua cổng đã ánh xạ.
- Biết cách dừng và xóa container bằng lệnh `docker compose down`.

## 2. Công nghệ sử dụng

- Docker Desktop
- Docker Compose CLI v2
- Nginx Alpine
- Windows PowerShell hoặc Terminal

## 3. Cấu trúc thư mục

```text
homework/
└── session_09/
    └── ex5/
        ├── docker-compose.yml
        └── README.md
```

## 4. Nội dung file docker-compose.yml

```yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "8082:80"
```

### Giải thích cấu hình

- `services`: Khai báo danh sách dịch vụ được quản lý bởi Docker Compose.
- `web`: Tên dịch vụ web.
- `image: nginx:alpine`: Sử dụng Docker Image Nginx Alpine để chạy web server.
- `ports`: Khai báo ánh xạ cổng giữa máy tính và container.
- `"8082:80"`: Chuyển tiếp yêu cầu từ cổng 8082 trên máy tính đến cổng 80 bên trong container.

**Lưu ý:** File YAML sử dụng dấu cách để thụt lề, không sử dụng phím Tab.

## 5. Các bước thực hiện

### Bước 1: Kiểm tra Docker

Mở Terminal và chạy:

```bash
docker --version
docker compose version
```

Hai lệnh trên dùng để kiểm tra Docker và Docker Compose CLI v2 đã sẵn sàng sử dụng hay chưa.

### Bước 2: Khởi chạy dịch vụ

Di chuyển Terminal đến thư mục chứa file `docker-compose.yml`, sau đó chạy:

```bash
docker compose up -d
```

Giải thích:

- `docker compose up`: Tạo và khởi chạy các dịch vụ được khai báo trong file Compose.
- `-d`: Chạy container ở chế độ nền.

Docker sẽ tải image `nginx:alpine` nếu image chưa có trên máy, sau đó tạo và khởi chạy container cho dịch vụ `web`.

### Bước 3: Kiểm tra trạng thái dịch vụ

Chạy lệnh:

```bash
docker compose ps
```

Lệnh này hiển thị trạng thái các container thuộc dự án Compose hiện tại.

Kết quả mong đợi:

- Dịch vụ có tên `web`.
- Container đang chạy.
- Cổng 8082 trên máy tính được ánh xạ đến cổng 80 trong container.

Ví dụ phần thông tin cổng:

```text
0.0.0.0:8082->80/tcp
```

Tên container và các thông tin hiển thị khác có thể khác tùy môi trường thực tế.

### Bước 4: Kiểm tra trang web Nginx

Trên Windows PowerShell, chạy:

```powershell
curl.exe http://localhost:8082
```

Lệnh gửi yêu cầu HTTP đến dịch vụ Nginx đang chạy trên máy tính.

Kết quả mong đợi là nội dung HTML của trang chào mừng mặc định Nginx, thường có tiêu đề:

```html
<h1>Welcome to nginx!</h1>
```

Có thể kiểm tra bằng trình duyệt tại địa chỉ:

http://localhost:8082

Nếu trang chào mừng Nginx xuất hiện, dịch vụ web đã hoạt động thành công.

### Bước 5: Dừng dịch vụ

Sau khi kiểm tra, chạy:

```bash
docker compose down
```

Lệnh này dừng và xóa container cùng mạng do Docker Compose tạo cho dự án. Image `nginx:alpine` vẫn được giữ lại để sử dụng cho lần chạy sau.

## 6. Kết quả mong đợi

Sau khi hoàn thành bài tập:

- File `docker-compose.yml` được tạo đúng cấu trúc.
- Lệnh `docker compose up -d` khởi chạy dịch vụ thành công.
- Lệnh `docker compose ps` hiển thị dịch vụ `web` đang chạy.
- Lệnh `curl.exe http://localhost:8082` trả về HTML của trang chào mừng Nginx.
- Lệnh `docker compose down` dừng và xóa container của dự án.

## 7. Một số lỗi thường gặp

### Lỗi cổng 8082 đã được sử dụng

Kiểm tra các container đang chạy:

```bash
docker ps
```

Nếu container khác đang chiếm cổng 8082, hãy dừng container đó nếu không còn cần thiết hoặc chọn một cổng host khác.

### Docker Compose không khởi chạy được

Kiểm tra Docker Desktop đã chạy và Docker Engine đã sẵn sàng chưa:

```bash
docker compose version
docker info
```

### Không truy cập được trang web

Kiểm tra trạng thái dịch vụ và log:

```bash
docker compose ps
docker compose logs web
```

Đảm bảo container đang chạy và cổng ánh xạ là `8082:80`.

## 8. Dọn dẹp tài nguyên

Sau khi hoàn thành bài tập, chạy:

```bash
docker compose down
```

Không cần xóa image Nginx nếu muốn tái sử dụng trong các lần thực hành tiếp theo.

## 9. Kết luận

Qua bài tập này, em đã thực hành sử dụng Docker Compose CLI v2 để khai báo, khởi chạy, kiểm tra và dừng một dịch vụ web Nginx. Việc sử dụng file `docker-compose.yml` giúp quản lý cấu hình dịch vụ tập trung, thuận tiện hơn so với việc nhập từng lệnh `docker run` thủ công.
