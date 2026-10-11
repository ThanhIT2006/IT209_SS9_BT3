# Bài 3: Viết Dockerfile đóng gói trang Web HTML tĩnh

## 1. Mục tiêu

- Tạo một trang web HTML tĩnh với nội dung được yêu cầu.
- Sử dụng Dockerfile để đóng gói trang web vào Docker Image dựa trên Nginx Alpine.
- Thực hành xây dựng Docker Image và khởi chạy Docker Container.
- Sử dụng Port Mapping để truy cập trang web thông qua cổng 8081.
- Kiểm tra kết quả triển khai bằng lệnh `curl`.

## 2. Cấu trúc thư mục

Các tệp của bài tập được lưu tại đường dẫn:

`D:\BTVN\IT209\SS9\BT3`

Cấu trúc thư mục:

```text
BT3/
├── Dockerfile
├── index.html
└── README.md
```

Trong đó:

- `index.html`: Chứa nội dung HTML của trang web.
- `Dockerfile`: Chứa các chỉ thị để xây dựng Docker Image.
- `README.md`: Mô tả mục tiêu, quy trình thực hiện và kết quả kiểm tra.

## 3. Tạo trang web HTML tĩnh

Tạo tệp `index.html` với nội dung:

```html
<h1>Hello Docker Session 09!</h1>
```

Đây là nội dung trang web cần được Nginx phục vụ sau khi khởi chạy container.

## 4. Xây dựng Dockerfile

Tệp `Dockerfile` gồm hai câu lệnh:

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

Giải thích:

- `FROM nginx:alpine`: Sử dụng image Nginx Alpine làm image cơ sở. Image này đã có sẵn Nginx để phục vụ nội dung web.
- `COPY index.html /usr/share/nginx/html/index.html`: Sao chép tệp HTML từ thư mục build vào thư mục mặc định chứa trang web của Nginx, thay thế nội dung trang mặc định bằng trang HTML của bài tập.

Dockerfile không cần thêm lệnh cài đặt Nginx hoặc khởi động dịch vụ vì image cơ sở đã cấu hình sẵn.

## 5. Xây dựng Docker Image

Tại thư mục chứa `Dockerfile` và `index.html`, thực hiện lệnh:

```bash
docker build -t my-html-app:v1 .
```

Giải thích:

- `docker build`: Xây dựng Docker Image từ Dockerfile.
- `-t my-html-app:v1`: Đặt tên image là `my-html-app` và gắn tag phiên bản `v1`.
- `.`: Chỉ định thư mục hiện tại làm build context.

Sau khi build thành công, kiểm tra image bằng lệnh:

```bash
docker images my-html-app
```

Kết quả cần hiển thị image `my-html-app` với tag `v1`.

## 6. Khởi chạy Docker Container

Sử dụng lệnh:

```bash
docker run -d -p 8081:80 --name html-app my-html-app:v1
```

Ý nghĩa các tham số:

- `-d`: Chạy container ở chế độ nền.
- `-p 8081:80`: Ánh xạ cổng 8081 trên máy chủ đến cổng 80 trong container.
- `--name html-app`: Đặt tên container là `html-app`.
- `my-html-app:v1`: Sử dụng image vừa xây dựng để tạo container.

Kiểm tra trạng thái container:

```bash
docker ps
```

Container `html-app` cần ở trạng thái đang chạy. Cột `PORTS` phải thể hiện ánh xạ tương ứng với cổng 8081 trên máy chủ và cổng 80 của container.

## 7. Kiểm tra hoạt động của trang web

Thực hiện lệnh:

```bash
curl http://localhost:8081
```

Kết quả mong đợi:

```html
<h1>Hello Docker Session 09!</h1>
```

Nếu kết quả trả về đúng nội dung trên, có thể xác nhận rằng:

- Docker Image đã được xây dựng thành công.
- Container Nginx đã khởi chạy.
- Port Mapping giữa cổng 8081 và cổng 80 hoạt động.
- Nginx trả về đúng nội dung HTML đã được đóng gói trong image.