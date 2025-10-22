# Lương Văn Học - K225480106025
# K58KTP Môn phát triển ứng dụng trên nền web
# Nội dung bài tập 2:
1. Sử dụng github để ghi lại quá trình làm, tạo repo mới, để truy cập public, edit file `readme.md`:
   chụp ảnh màn hình (CTRL+Prtsc) lúc đang làm, paste vào file `readme.md`, thêm mô tả cho ảnh.
2. NỘI DUNG BÀI TẬP:
2.1. Cài đặt Apache web server:
- Vô hiệu hoá IIS: nếu iis đang chạy thì mở cmd quyền admin để chạy lệnh: iisreset /stop
- Download apache server, giải nén ra ổ D, cấu hình các file:
  + D:\Apache24\conf\httpd.conf
  + D:Apache24\conf\extra\httpd-vhosts.conf
  để tạo website với domain: fullname.com
  code web sẽ đặt tại thư mục: `D:\Apache24\fullname` (fullname ko dấu, liền nhau)
- sử dụng file `c:\WINDOWS\SYSTEM32\Drivers\etc\hosts` để fake ip 127.0.0.1 cho domain này
  ví dụ sv tên là: `Đỗ Duy Cốp` thì tạo website với domain là fullname ko dấu, liền nhau: `doduycop.com`
- thao tác dòng lệnh trên file `D:\Apache24\bin\httpd.exe` với các tham số `-k install` và `-k start` để cài đặt và khởi động web server apache.
2.2. Cài đặt nodejs và nodered => Dùng làm backend:
- Cài đặt nodejs:
  + download file `https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi`  (đây ko phải bản mới nhất, nhưng ổn định)
  + cài đặt vào thư mục `D:\nodejs`
- Cài đặt nodered:
  + chạy cmd, vào thư mục `D:\nodejs`, chạy lệnh `npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"`
  + download file: https://nssm.cc/release/nssm-2.24.zip
    giải nén được file nssm.exe
    copy nssm.exe vào thư mục `D:\nodejs\nodered\`
  + tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):
@echo off
REM fix path
set PATH=D:\nodejs;%PATH%
REM Run Node-RED
node "D:\nodejs\nodered\node_modules\node-red\red.js" -u "D:\nodejs\nodered\work" %*
  + mở cmd, chuyển đến thư mục: `D:\nodejs\nodered`
  + cài đặt service `a1-nodered` bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"
  + chạy service `a1-nodered` bằng lệnh: `nssm start a1-nodered`
2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name
2.4. Cài đặt thư viện trên nodered:
- truy cập giao diện nodered bằng url: http://localhost:1880
- cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus
- Sửa file `D:\nodejs\nodered\work\settings.js` : 
  tìm đến chỗ adminAuth, bỏ comment # ở đầu dòng (8 dòng), thay chuỗi mã hoá mật khẩu bằng chuỗi mới
    adminAuth: {
        type: "credentials",
        users: [{
            username: "admin",
            password: "chuỗi_mã_hoá_mật_khẩu",
            permissions: "*"
        }]
    },   
   với mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php
- chạy lại nodered bằng cách: mở cmd, vào thư mục `D:\nodejs\nodered` và chạy lệnh `nssm restart a1-nodered`
  khi đó nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880
2.5. tạo api back-end bằng nodered:
- tại flow1 trên nodered, sử dụng node `http in` và `http response` để tạo api
- thêm node `MSSQL` để truy vấn tới cơ sở dữ liệu
- logic flow sẽ gồm 4 node theo thứ tự sau (thứ tự nối dây): 
  1. http in  : dùng GET cho đơn giản, URL đặt tuỳ ý, ví dụ: /timkiem
  2. function : để tiền xử lý dữ liệu gửi đến
  3. MSSQL: để truy vấn dữ liệu tới CSDL, nhận tham số từ node tiền xử lý
  4. http response: để phản hồi dữ liệu về client: Status Code=200, Header add : Content-Type = application/json
  có thể thêm node `debug` để quan sát giá trị trung gian.
- test api thông qua trình duyệt, ví dụ: http://localhost:1880/timkiem?q=thị
2.6. Tạo giao diện front-end:
- html form gồm các file : index.html, fullname.js, fullname.css
  cả 3 file này đặt trong thư mục: `D:\Apache24\fullname`
  nhớ thay fullname là tên của bạn, viết liền, ko dấu, chữ thường, vd tên là Đỗ Duy Cốp thì fullname là `doduycop`
  khi đó 3 file sẽ là: index.html, doduycop.js và doduycop.css
- index.html và fullname.css: trang trí tuỳ ý, có dấu ấn cá nhân, có form nhập được thông tin.
- fullname.js: lấy dữ liệu trên form, gửi đến api nodered đã làm ở bước 2.5, nhận về json, dùng json trả về để tạo giao diện phù hợp với kết quả truy vấn của bạn.
2.7. Nhận xét bài làm của mình:
- đã hiểu quá trình cài đặt các phần mềm và các thư viện như nào?
- đã hiểu cách sử dụng nodered để tạo api back-end như nào?
- đã hiểu cách frond-end tương tác với back-end ra sao?
==============================
TIÊU CHÍ CHẤM ĐIỂM:
1. y/c bắt buộc về thời gian: ko quá Deadline, quá: 0 điểm (ko có ngoại lệ)
2. cài đặt được apache và nodejs và nodered: 1đ
3. cài đặt được các thư viện của nodered: 1đ
4. nhập dữ liệu demo vào sql-server: 1đ
5. tạo được back-end api trên nodered, test qua url thành công: 1đ
6. tạo được front-end html css js, gọi được api, hiển thị kq: 1đ
7. trình bày độ hiểu về toàn bộ quá trình (mục 2.7): 5đ
==============================
GHI CHÚ:
1. yêu cầu trên cài đặt trên ổ D, nếu máy ko có ổ D có thể linh hoạt chuyển sang ổ khác, path khác.
2. có thể thực hiện trực tiếp trên máy tính windows, hoặc máy ảo
3. vì csdl là tuỳ ý: sv cần mô tả rõ db chứa dữ liệu gì, nhập nhiều dữ liệu test có nghĩa, json trả về sẽ có dạng như nào cũng cần mô tả rõ.
4. có thể xây dựng nhiều API cùng cơ chế, khác tính năng: như tìm kiếm, thêm, sửa, xoá dữ liệu trong DB.
5. bài làm phải có dấu ấn cá nhân, nghiêm cấm mọi hình thức sao chép, gian lận (sẽ cấm thi nếu bị phát hiện gian lận).
6. bài tập thực làm sẽ tốn nhiều thời gian, sv cần chứng minh quá trình làm: save file `readme.md` mỗi khoảng 15-30 phút làm : lịch sử sửa đổi sẽ thấy quá trình làm này!
7. nhắc nhẹ: github ko fake datetime được.
8. sv được sử dụng AI để tham khảo.
==============================
# -----BÀI LÀM-----
## 1. Cài đặt Apache web server

### 1.1. Vô hiệu hoá IIS

- Nếu iis đang chạy thì mở cmd quyền admin để chạy lệnh: iisreset /stop

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0945e80f-f14d-4eb3-96fa-892575f2d710" />

### 1.2. Download apache server, giải nén ra ổ D, cấu hình các file:

1. sau khi tải và giải nén file ra ổ D.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4280b14d-ad58-431e-a847-3d68411db7dd" />

2. Đảm bảo 2 dòng này không bị comment:

*ServerName 127.0.0.1:80*

*Include conf/extra/httpd-vhosts.conf*

- Vào Start Menu → gõ Notepad.

- Chuột phải vào Notepad → chọn Run as administrator.

- Trong Notepad, chọn File → Open.

- Điều hướng đến đường dẫn: D:\Apache24\conf\httpd.conf

(ở phần “File type” chọn All Files (.) thì mới thấy file .conf).

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1aa2883e-3627-47aa-8b64-74202e238f5e" />

Mở file, chỉnh sửa theo hướng dẫn (bỏ dấu #, sửa ServerName, bật Include …).

<img width="937" height="703" alt="image" src="https://github.com/user-attachments/assets/fda2c6e1-74fe-4b02-81c2-a71fbc05b708" />

<img width="900" height="685" alt="image" src="https://github.com/user-attachments/assets/eef5aebd-58b2-451f-90ed-fe0bf13bc643" />

Nhấn Ctrl+S để lưu.

3.	Tạo thư mục web cá nhân

D:\Apache24\luongvanhoc

<img width="871" height="369" alt="image" src="https://github.com/user-attachments/assets/0d48d247-7373-4c4d-a709-0d93ed0f6f82" />

4. Sửa D:\Apache24\conf\extra\httpd-vhosts.conf (thêm VirtualHost mới)

<img width="1683" height="813" alt="image" src="https://github.com/user-attachments/assets/31044cb9-e0d7-4965-99b0-06ed9b80a89b" />

### 1.3. Fake domain vào hosts

- Mở Notepad Run as administrator → file: C:\Windows\System32\drivers\etc\hosts

- Thêm: 127.0.0.1    luongvanhoc.com

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/12eb762e-bf04-445a-803d-6c955e7605bc" />

### 1.4. Cài dịch vụ & khởi động Apache

- Mở CMD (Admin) tại D:\Apache24\bin:

httpd.exe -k install -n "Apache24"
httpd.exe -k start -n "Apache24"

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8353ab8a-1507-428a-8e11-360af4db1ca2" />

Test: mở trình duyệt đến http://luongvanhoc.com/ 

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e5e4a69e-6add-453a-a794-4afb0cadd9d5" />

## 2.Cài đặt nodejs và nodered













