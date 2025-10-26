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

### 2.1. Cài đặt nodejs

download file: [https://nodejs.org/en/download](https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi)

Tải file MSI và cài vào: D:\nodejs 

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/68c8ce2b-7a1e-45d8-aa18-490cc2e4d3f0" />

nodejs đẫ được cài

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c470c4b4-99b7-4a28-8ffd-a2e0e2d6921d" />

### 2.2. Cài đặt nodered

- chạy cmd, vào thư mục `D:\nodejs`, chạy lệnh: npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/de022cda-ae41-425f-a1ae-2690a2f94d94" />

- download file: https://nssm.cc/release/nssm-2.24.zip
    giải nén được file nssm.exe
    copy nssm.exe vào thư mục `D:\nodejs\nodered\`

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/324da56a-3825-402a-b99f-4c941d71fcc8" />

- tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):
@echo off
REM fix path
set PATH=D:\nodejs;%PATH%
REM Run Node-RED
node "D:\nodejs\nodered\node_modules\node-red\red.js" -u "D:\nodejs\nodered\work" %*

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1dd37d2d-6c83-4634-b1c2-6a3a56b89c87" />


  + mở cmd, chuyển đến thư mục: `D:\nodejs\nodered`
  + cài đặt service `a1-nodered` bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"
  + chạy service `a1-nodered` bằng lệnh: `nssm start a1-nodered`

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/de546ee0-bcb2-47fe-8b6c-16d7bdd47a87" />

Test: mở http://localhost:1880/ (mặc định chưa bật adminAuth).

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/94c18673-c8ed-42a6-84b6-d18aecfdc3ee" />

## 3. Tạo CSDL SQL Server 2022 + Data demo

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c3e5ed56-5019-4d65-8911-d3d782623b8e" />

- ip 127.0.0.1 

- port (mặc định 1433)

- username : sa

- password :123456

- db_name = WebDevDemo

- table_name = Students

## 4. Cài node thư viện trên Node-RED + bật adminAuth

### 4.1 truy cập giao diện nodered bằng url: http://localhost:1880

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6b707433-cdf2-4b11-b259-976c3f39eeaa" />

- Menu → Manage palette → Install:

 Tải các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus 

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/64cfb0a5-1ff4-4e72-9865-cd0d358e6bbe" />

Các thư viện khác cài tương tự.

### 4.2 Sửa file settings.js để bật đăng nhập admin

- Mở file bằng Notepad (Run as administrator)

- Đường dẫn: D:\nodejs\nodered\work\settings.js.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4108f8ef-f724-4159-bfa1-db475f4044ef" />

mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9260cd79-23bc-4533-9c30-a06b5fe478a7" />

tìm đến chỗ adminAuth, bỏ comment # ở đầu dòng (8 dòng), thay chuỗi mã hoá mật khẩu bằng chuỗi mới
```
    adminAuth: {
        type: "credentials",
        users: [{
            username: "admin",
            password: "$2y$10$j0J5eKKLGgUIBxAfI8zqL.y7dVFxnOmFqa4C37P8Q3A8y5pJlMVkm",
            permissions: "*"
        }]
    },   
```
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6d12b1d5-b8de-4bc0-b4aa-658add95976f" />

### 4.3 Lưu file → khởi động lại dịch vụ

chạy lại nodered bằng cách: mở cmd, vào thư mục `D:\nodejs\nodered` và chạy lệnh `nssm restart a1-nodered`

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d4df6a7f-100f-49f6-8358-13093b0cf56e" />

khi đó nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0382ed00-cfaf-46e2-ba61-e489f4590a12" />

## 5.tạo api back-end bằng nodered

API sẽ gồm 4 node chính:

[HTTP in] → [Function: xử lý tham số] → [MSSQL-PLUS] → [HTTP response]

Mỗi node đảm nhiệm:

HTTP in → Nhận request từ trình duyệt (GET /timkiem).

Function → Chuẩn bị câu SQL và tham số (query string).

MSSQL-PLUS → Thực thi truy vấn trong SQL Server.

HTTP response → Trả kết quả JSON cho client.

### 5.1. Cấu hình chi tiết từng node
A. HTTP in

Method: GET

URL: /timkiem

→ Khi gọi http://localhost:1880/timkiem Node-RED sẽ kích hoạt flow này.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fbae5c2a-c9d1-4420-ae94-d4d595b54145" />

B. Function (Xử lý tham số)

- Thêm node function đặt tên: Xử lý tham số.
- Thêm code:
```
//  Lấy tham số truy vấn từ URL, ví dụ: /timkiem?q=thi
let tukhoa = (msg.req && msg.req.query && msg.req.query.q)
    ? String(msg.req.query.q).trim()
    : "";

//  Header JSON + CORS (để frontend Apache gọi được)
msg.headers = {
    "Content-Type": "application/json; charset=utf-8",
    "Access-Control-Allow-Origin": "*"
};

//  Chuẩn bị câu truy vấn SQL + tham số theo đúng bài mẫu
//    - DÙNG THAM SỐ ĐẶT TÊN @keyword
//    - COLLATE Vietnamese_100_CI_AI để không phân biệt dấu/hoa thường
msg.payload = {
    query: `
    SELECT TOP 50
      Id,
      HoTen,         -- nếu bạn có cột 'Name' thì đổi 'HoTen' thành 'Name'
      Lop,
      DienThoai,
      Email,
      DiaChi
    FROM dbo.Students
    WHERE HoTen COLLATE Vietnamese_100_CI_AI LIKE @keyword
       OR Email  COLLATE Vietnamese_100_CI_AI LIKE @keyword
       OR Lop    COLLATE Vietnamese_100_CI_AI LIKE @keyword
    ORDER BY HoTen;
  `,
    parameters: [
        // tên param phải trùng @keyword ở trên
        { name: "keyword", type: "NVarChar", value: `%${tukhoa}%` }
    ]
};

//  KHÔNG gán msg.topic, KHÔNG gán msg.params, KHÔNG gán msg.payload chỗ khác
return msg;
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/df5f934d-d586-43d6-be71-0cf4b1811490" />


C. MSSQL-PLUS

Connection: trỏ tới SQL Server (Server: 127.0.0.1, Port: 1433, Database: WebDevDemo, Username/Password đúng; tick Trust Certificate)

Query mode: Query

Query: chọn msg. → gõ payload.query

Parameters: chọn msg. → gõ payload.parameters

Parse Mustache: bỏ chọn

Output property: msg.payload

Output type: Original output

Error Handling: Send in msg.error

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ceae7151-fda7-4043-a95a-e9929bbeb01b" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0e46eed2-f3d5-413b-9031-344843f98d29" />

D. Tạo node HTTP response

- Status code: 200

- Headers: add Content-Type: application/json

- Node này sẽ gửi msg.payload (chính là kết quả SQL) ra trình duyệt.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fee5a684-6dc5-464a-89ec-3450403dd13e" />

### 5.2. Kiểm thử API

- Trình duyệt: http://localhost:1880/timkiem?q=văn

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/18a80da5-efbe-4f0c-bbd8-b7e50c159903" />

## 5.Tạo giao diện front-end

### 5.1. Chuẩn bị thư mục web

Tạo thư mục: D:\Apache24\luongvanhoc

Trong thư mục này tạo 3 file:

index.html

luongvanhoc.js

luongvanhoc.css





