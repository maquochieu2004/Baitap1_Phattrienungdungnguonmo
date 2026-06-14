# Baitap1_Phattrienungdungnguonmo
## Môn: Phát triển ứng dụng với mã nguồn mở-TEE0421
## Lớp: 58KTPM

## Bài tập 01:

thời hạn : 23h59 ngày 13 tháng 4 năm 2026.
A. Đăng ký tên miền đặc biệt cho cá nhân:
Đăng ký miền kiệt (có thể dùng mắt bão , tên miền *.id.vn đang miễn phí cho mọi công dân việt nam <= 23 tuổi, *.io.vn : giá 30k vnđ/năm)
Đăng ký tài khoản cloudflare
Thêm tên miền đã đăng nhập trong cloudflare : Nhận 2 dòng không gian tên
Nhập 2 dòng namespace của cloudflare vào trong trang quản lý bản ghi DNS của tên miền đăng ký (vd trên mắt bão)
B. Cài đặt Ubuntu + Docker
Cài đặt hệ điều hành Ubuntu 24.04.4 LTS
Sử dụng một trong các công cụ để cài đặt: HyperV (có sẵn windows), VirutualBox (Miễn phí), VM_Ware (bản quyền)
Tải file iso về để cài đặt.
Cấu hình mạng trong Ubuntu (và công cụ giả lập) để cho phép truy cập SSH vào Ubuntu từ cmd của windows
Ví dụ:

để ssh tới ubuntu ở ip 192.168.100.123, user là admin thì mở CMD trên windows,
gõ lệnh: ssh admin@192.168.100.123
hệ thống sẽ yêu cầu mật khẩu nhập vào (chú ý : mật khẩu sẽ không hiển thị)
sau khi đăng nhập thành công sẽ thấy màn hình câu hỏi của ubuntu
Tìm hiểu cơ sở lệnh của Ubuntu
Lệnh cần tìm hiểu:

Liệt kê các tập tin trong thư mục: ls
Tạo thư mục: mkdir nameFolder
Chuyển thư mục làm việc: đường dẫn cd
Copy file: cp file_đường dẫn nguồn/file_đích
Thay đổi quyền thao tác file: sudo chmod xxx filename
Chỉnh sửa tệp: sudo nano tenfile
CTRL+o : lưu nội dung sau khi chỉnh sửa
CTRL+x : Chỉnh sửa tệp
Xem ip của máy ubuntu: ip -4 addr
Cài đặt docker cho Ubuntu
Kiểm tra phiên bản cài đặt của docker, kiểm tra phiên bản của docker editor
Cấu hình để chạy docker mà không cần tiền tố sudo
Tìm hiểu lệnh của docker và soạn thảo docker
Bảo đảm tường lửa trên Ubuntu đã cho phép các cổng 80, 1880, 9630 (Lệnh: sudo ufw allow ...)
C. Cấu hình docker soạn:
Tạo thư mục: ~/myapp
Move to in folder ~/myapp
Tạo thư mục: ./myweb
Create file ./myweb/index.html (với nội dung là thông tin cá nhân của em)
Tạo tập tin docker-compose.yml để nó có các dịch vụ sau:
Khai báo use nodered/node-red, port 1880, data in the folder ./nodered
Khai báo use nginx, port 80, config in file ./nginx/nginx.conf
Gắn thư mục ./myweb thành thư mục /myweb trong nginx
Mount file ./nginx/nginx.conf vào file /etc/nginx/nginx.conf trong nginx
Chỉnh sửa file ./nginx/nginx.conf để:
Cấu hình máy chủ web port 80
server_name là sub-domain (sub-domain tùy ý của em)
location / con trỏ tới root là thư mục /myweb
location /api use proxy_pass trỏ tới 1 (hoặc nhiều) nút http_in của nút
Chỉnh sửa tệp ./nodered/settings.js để noded bắt buộc đăng nhập
Chạy docker-compose lần đầu tiên để cấu hình tệp tự động Node-RED trong thư mục ./nodered, sau đó mới tiến hành chỉnh sửa settings.js và khởi động lại vùng chứa

D. (Tiền thưởng - không bắt buộc)
tạo thư mục ./myapi
tạo tập tin ./myapi/app.py sử dụng Python + Flask để làm điều gì đó thú vị
tạo tệp ./myapi/requirements.txt chứa các thư viện mà app.py sử dụng (ví dụ như app.py thì require.txt chỉ cần có nội dung: jar )
tạo file ./myapi/Dockerfile để khai báo sử dụng Python 3.9 slim
 # Sử dụng phiên bản Python nhẹ (alpine) để giảm dung lượng image
 FROM python:3.9-slim

 # Thiết lập thư mục làm việc bên trong container
 WORKDIR /app

 # Sao chép file requirements vào và cài đặt thư viện
 COPY requirements.txt .
 RUN pip install --no-cache-dir -r requirements.txt

 # Sao chép toàn bộ mã nguồn vào container
 COPY . .

 # Thông báo container sẽ chạy ở cổng 9630
 EXPOSE 9630

 # Lệnh khởi chạy ứng dụng
 CMD ["python", "app.py"]
Chỉnh sửa docker-compose để sử dụng myapp (xem phần tham khảo ở bên dưới)
Chỉnh sửa nginx/nginx.conf để /api trỏ tới dịch vụ myapp port 9630
E. Triển khai ứng dụng (kiểm tra trình độ)
Move to in folder ~/myapp
Gõ lệnh để docker soạn chạy: sẽ chạy tất cả các khai báo dịch vụ trong tệp docker-compose.yml
Lợi ích: Chỉ cần docker-compose up -d là toàn bộ hệ thống (Web + Node-RED + Tunnel) tự chạy,

Kiểm tra các container đang chạy trong docker, nếu có cái nào được khởi động lại cần tìm lỗi và chỉnh sửa lại docker-compose.yml
Kiểm tra kiểm tra các dịch vụ đang chạy độc lập thông tin qua ip và cổng của nó: ví dụ mở trình duyệt ip_ubuntu:1880 để kiểm tra nút đã chạy chưa
Sử dụng nodered: kéo nodered http_in , http_response, function : để tạo api get đơn giản (dùng cho /api proxy_pass của nginx)
Edit file ./myweb/index.html : thêm code html+js để sử dụng api đã khai báo proxy_pass (thực tế là sử dụng http_in được gật đầu hoặc sử dụng dịch vụ myapi)
F. Gỡ lỗi:
if có lỗi tiến triển trong quá trình phát triển khai docker soạn thảo -d
Kiểm tra tốc độ nhanh: docker soạn ps giúp biết container nào đang chạy xem nhật ký, ví dụ: docker logs mynginx docker logs myapi

Thêm healthcheck cho myapi trong file docker-compose.yml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:9630"]
giới hạn tài nguyên cho 1 service: (trang job 1 service use too many ram)
deploy:
  resources:
    limits:
      memory: 512M
use command: docker soạn stats để định lượng ram sử dụng cho mỗi dịch vụ
G. Triển khai ứng dụng cho End-user
Trong Cloudflare: Tạo đường hầm (đường hầm), chọn loại phát triển khai cho docker
Convert docker run command ... sang docker doing dạng soạn thảo
Khai báo kết quả chuyển đổi thành tệp docker-compose.yml
Chạy lại docker compose
Ứng dụng công khai bằng cách thêm 1 con trỏ bộ định tuyến tới vùng chứa đang chạy trong docker, dữ liệu sẽ đi qua đường hầm, dạng url phụ
Kiểm tra tên miền phụ url đã hoạt động công khai cho mọi người dùng cuối
Cấu trúc thư mục:
myapp/
├── docker-compose.yml
├── nginx/
│   └── nginx.conf
├── myweb/
│   └── index.html
└── nodered/ (sẽ tự sinh dữ liệu)
│   └── (có nhiều file tự sinh)
│   └── settings.js (file này cần edit để bắt nodered login)
Sơ đồ góc nhìn của nhà phát triển:

Sơ đồ theo góc nhìn ngược lại:

G. Câu hỏi về bài làm?
Tại sao phải dùng Nginx làm Reverse Proxy mà không trỏ Tunnel vào Node-RED?
Sự khác biệt giữa Mount file và Mount folder trong Docker là gì?
Nếu thay đổi file index.html trên máy Ubuntu, nội dung trên web có thể thay đổi ngay không? Tại sao?
docker-compose.yml khai báo các dịch vụ có phần khởi động lại: luôn luôn hoặc khởi động lại: trừ khi dừng : chúng để làm gì?
Cách khai báo để tất cả các dịch vụ đều dùng chung 1 mạng? lợi ích của việc khai báo này là gì? Chỉnh sửa file docker-compose để tất cả các dịch vụ dùng chung 1 mạng.
Tìm cách Cloudflare Token đưa vào trong tệp .env rồi sau đó thêm .env vào tệp .gitignore trước khi đẩy mã lên github. Tại sao nói đây là điều quan trọng về nguồn mã bảo mật?
Tại sao chúng ta nên thêm hậu tố :ro khi mount file cấu hình Nginx?
Khi sử dụng Cloudflare Tunnel: có cần thiết phải mở cổng cho các dịch vụ nữa không?
Hướng dẫn làm bài:
sv tự làm trên laptop cá nhân, tự động nâng cấp các phần mềm hoặc hệ điều hành lên phiên bản phù hợp, trang được cấu hình đủ tải (RAM từ 8GB, ổ cứng SSD hoặc NVME)
quá trình thực hiện: chụp màn hình, dán hình ảnh + nhập văn bản chú thích cho hình ảnh vào readme.md của 1 repo trên github cá nhân, để truy cập công khai
Mỗi phần ABCDEFG tạo 1 tệp tương ứng là A.md , B.md .... file README.md chứa liên kết tới các tệp A.md, B.md, ... để dễ quản lý
Mỗi tập tin cho mỗi phần chứa nội dung đã làm: hình ảnh + văn bản thuyết minh (lặp nhiều lần) cho phần đó.
làm xong tất cả: dán link repo vào file tổng hợp excel online (làm sau cũng được, vì github ko fake date được)
link gửi bài: https://docs.google.com/s Spreadsheets/d/1zftQMj748nRpS-_br4_jdHZocNVvo848zqxCGcTy4uU/edit?gid=0#gid=0

Tham khảo tập tin trên lớp
./docker-compose.yml :

	 services:
	  myapi:
	    build:
	      context: ./myapi
	      dockerfile: Dockerfile
	    container_name: myapi
	    ports:
	      - "9630:9630"
	    restart: always

	  mynodered:
	    image: nodered/node-red
	    container_name: mynodered
	    restart: unless-stopped
	    ports:
	      - "1880:1880"
	    volumes:
	      # đường dẫn thư mục trên máy của bạn
	      - ./nodered:/data

	  mycloudflared:
	    image: cloudflare/cloudflared:latest
	    container_name: mycloudflared
	    restart: unless-stopped
	    command: tunnel --no-autoupdate run --token <your_token>

	  mynginx:
	    image: nginx
	    container_name: mynginx
	    restart: always
	    ports:
	      - "80:80"
	      - "443:443"
	    volumes:
	      # Ánh xạ thư mục chứa file bài thơ
	      - ./myweb:/myweb:ro
	      # Ánh xạ file cấu hình nginx
	      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
./nginx/nginx.conf :

events {}
http {
	server {
	    listen 80;
	    server_name thotinh.tdh.io.vn;

	    location / {
	        root /myweb;
	        index index.html index.htm;
	        try_files $uri $uri/ =404;
	    }

	    location /api {
	        # 'myapi' là tên container trong docker-compose
	        proxy_pass http://myapi:9630/tinh-vat;
	        proxy_set_header Host $host;
	        proxy_set_header X-Real-IP $remote_addr;
	        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	        proxy_set_header X-Forwarded-Proto $scheme;
	    }
	}
}
./myapp/app.py :

	from flask import Flask, request, jsonify

	app = Flask(__name__)

	@app.route('/tinh-vat', methods=['GET'])
	def tinh_vat():
	    # Lấy giá trị từ tham số "tien" trên URL
	    tien_input = request.args.get('tien')
	    
	    # Kiểm tra xem người dùng có nhập tiền hay không
	    if tien_input is None:
	        return jsonify({"error": "Vui lòng cung cấp tham số 'tien'"}), 400
	    
	    try:
	        # Chuyển đổi sang kiểu số thực và tính toán
	        so_tien = float(tien_input)
	        ket_qua = so_tien * 1.1
	        
	        return jsonify({
	            "so_tien_goc": so_tien,
	            "thue_vat": "10%",
	            "tong_cong": ket_qua
	        })
	    except ValueError:
	        # Trả về lỗi nếu đầu vào không phải là số
	        return jsonify({"error": "Giá trị 'tien' phải là một con số hợp lệ"}), 400

	if __name__ == '__main__':
	    # Chạy ứng dụng tại cổng 9630
	    app.run(host='0.0.0.0', port=9630)
