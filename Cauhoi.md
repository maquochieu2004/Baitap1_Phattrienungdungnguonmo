# Câu hỏi về bài làm
1.Tại sao phải dùng Nginx làm Reverse Proxy mà không trỏ thẳng Tunnel vào Node-RED?
- Ẩn service phía sau → tăng bảo mật
- Có thể quản lý nhiều service (Node-RED, API, web…) qua 1 domain
- Hỗ trợ SSL, load balancing, routing (/, /api)
- Giảm tải và kiểm soát request tốt hơn
2. Sự khác biệt giữa việc Mount file và Mount thư mục trong Docker là gì?
- Mount file:
 + Chỉ map 1 file cụ thể
- Mount thư mục:
+ Map cả folder
+ Dùng để lưu dữ liệu, code (vd: ./myweb, ./nodered)
3. Nếu thay đổi file index.html ở máy Ubuntu, nội dung trên web có thay đổi ngay không? Tại sao?
- Trong hầu hết trường hợp: Có, thay đổi sẽ hiển thị ngay.
+ Lý do:
- Thư mục ./myweb được mount trực tiếp vào container Nginx.
- Nginx đọc file từ hệ thống file tại thời điểm request.
+ Tuy nhiên:
- Trình duyệt có thể cache → cần reload (Ctrl + F5) để thấy thay đổi.
4. docker-compose.yml khai báo các services có phần restart: always hoặc restart: unless-stopped : chúng để làm gì?
- Đây là cơ chế tự động khởi động lại container:
- restart: always:
- Container luôn được khởi động lại nếu bị dừng hoặc lỗi.
- Ngay cả khi Docker daemon restart, container vẫn chạy lại.
- restart: unless-stopped:
- Container tự restart trừ khi người dùng chủ động dừng (docker stop). → Giúp đảm bảo hệ thống luôn hoạt động ổn định.
5. Cách khai báo để tất cả các services đều dùng chung 1 network? lợi ích của việc khai báo này là gì? Sửa đổi file docker-compose để tất cả các service đều dùng chung 1 network.
" networks:
  mynetwork:

services:
  nodered:
    networks:
      - mynetwork

  nginx:
    networks:
      - mynetwork "
6. Tìm cách đưa Cloudflare Token vào trong file .env rồi sau đó thêm .env vào file .gitignore trước khi push code lên github. Tại sao nói đây là điều quan trọng về bảo mật mã nguồn?
File .env dùng để lưu thông tin nhạy cảm như token, password.
- Thêm .env vào .gitignore để không bị đẩy lên GitHub. Ý nghĩa bảo mật:
- Tránh lộ thông tin quan trọng ra public repository.
- Ngăn chặn người khác sử dụng token để truy cập hệ thống của bạn.
- Đây là best practice trong phát triển phần mềm.
7. Tại sao chúng ta nên thêm hậu tố :ro khi mount file cấu hình Nginx?
- :ro (read-only) giúp container chỉ có quyền đọc file.
- Ngăn container sửa đổi file cấu hình từ bên trong.
- Giảm rủi ro bị thay đổi cấu hình ngoài ý muốn hoặc do tấn công.
8. Khi dùng Cloudflare Tunnel: có cần thiết phải mở cổng cho các service nữa không?
- Không cần mở port ra Internet.
+ Vì:
- Tunnel tạo kết nối từ server → Cloudflare (outbound).
- Người dùng truy cập qua Cloudflare, không truy cập trực tiếp server.
- Tuy nhiên:
Các service nội bộ (Docker) vẫn cần port để giao tiếp trong hệ thống.
