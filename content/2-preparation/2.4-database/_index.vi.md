---
title : "Database Server"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 2.4 </b> "
---

#### Cài đặt MongoDB
Tạo một EC2 Instance với cấu hình sau:

![alt text](/images/2-preparation/2.4-database/2-4-1.png)

Quá trình tạo EC2 xem lại phần 2.2.

Sau khi tạo thành công tiến hành Connect vào EC2 và tạo file **Bash Script** cài đặt MongoDB.

```
#!/bin/bash
sudo apt-get install gnupg curl
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
sudo apt-get update
sudo apt-get install -y mongodb-org
echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-database hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-mongosh hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections
```
Chạy file **Bash Script** trên bằng cách thực hiện lệnh sau:
```
sh <name-file>.sh
```
![alt text](/images/2-preparation/2.4-database/2-4-2.png)
Sau khi cài đặt thành công, tiến hành khởi động MongoDB Community Edition.
```
sudo systemctl start mongod
```
Kiểm tra xem MongoDB đã khởi động thành công chưa bằng lệnh:
```
sudo systemctl status mongod
```
![alt text](/images/2-preparation/2.4-database/2-4-3.png)

Có thể sử dụng tùy chọn để đảm bảo rằng MongoDB sẽ khởi động sau khi khởi động lại hệ thống bằng lệnh:
```
sudo systemctl enable mongod
```

#### Cấu hình MongoDB
Truy cập vào file `/etc/mongod.conf` để thay đổi phạm vi địa chỉ IP mà MongoDB lắng nghe và chấp nhận kết nối.

Chuyển từ `bindIp: 127.0.0.1` thành `bindIp: 0.0.0.0`

- **127.0.0.1 (localhost)**: Đây là địa chỉ loopback của máy cục bộ (localhost). Nếu bindIp được đặt là 127.0.0.1, MongoDB sẽ chỉ chấp nhận kết nối từ chính máy tính nơi MongoDB đang chạy. Các kết nối từ bên ngoài (từ máy tính khác hoặc mạng khác) sẽ bị từ chối.

- **0.0.0.0**: Khi bindIp được đặt là 0.0.0.0, MongoDB sẽ lắng nghe trên tất cả các địa chỉ IP của máy chủ. Điều này có nghĩa là MongoDB sẽ chấp nhận kết nối từ bất kỳ IP nào, bao gồm cả máy tính cục bộ và các máy tính khác trong mạng.

Điều này giúp chúng ta có thể truy cập từ máy cục bộ đến máy ảo cài đặt MongoDB.

![alt text](/images/2-preparation/2.4-database/2-4-4.png)

Sau khi thay đổi, tiến hành khởi động lại MongoDB.
```
sudo systemctl restart mongod
```
![alt text](/images/2-preparation/2.4-database/2-4-5.png)

Tạo Firewall Rule để mở MongoDB Port ở EC2 Instance.

Truy cập vào **Instance** -> **Security**.
![alt text](/images/2-preparation/2.4-database/2-4-6.png)

Chọn **Security Group** chứa Instance.
![alt text](/images/2-preparation/2.4-database/2-4-7.png)

Chọn **Edit Inbound Rules.**

![alt text](/images/2-preparation/2.4-database/2-4-8.png)

Chọn **Add Rule** và chọn theo cấu hình sau:
![alt text](/images/2-preparation/2.4-database/2-4-9.png)

**27017** là Port dùng để chạy MongoDB.

{{% notice warning %}}
Việc mở port 27017 cho toàn bộ internet (0.0.0.0/0) có thể tạo ra rủi ro bảo mật cao. Nếu có thể bạn hãy giới hạn truy cập bằng cách chỉ cho phép địa chỉ IP hoặc dải IP đáng tin cậy.
{{% /notice %}}

#### Kết nối MongoDB Compass tới MongoDB trong EC2

Bạn có thể cài đặt MongoDB Compass ở link sau:

- [MongoDB Compass](https://downloads.mongodb.com/compass/mongodb-compass-1.43.6-win32-x64.exe)

Đây là giao diện chính của MongoDB Compass.

![alt text](/images/2-preparation/2.4-database/2-4-10.png)

Ở EC2 Instance chúng ta có `Public IPv4 address` và `Public IPv4 DNS`, chúng ta có thể truy cập đến Database thông qua chúng.

![alt text](/images/2-preparation/2.4-database/2-4-11.png)

Tiến hành truy cập vào Database thông qua MongoDB Compass.

Thay đổi `localhost` thành địa chỉ IP Public của Instance và kết nối:

![alt text](/images/2-preparation/2.4-database/2-4-12.png)

Sau khi kết nối thành công chúng ta đã có Database tương tự như trên Server:

![alt text](/images/2-preparation/2.4-database/2-4-13.png)

Có thể kiểm tra Database ở EC2 bằng lệnh:
```sh
mongosh             # Truy cập vào DB
show dbs            # Show danh sách DB
```

![alt text](/images/2-preparation/2.4-database/2-4-14.png)

Như vậy chúng ta đã cài đặt thành công Database Server.










