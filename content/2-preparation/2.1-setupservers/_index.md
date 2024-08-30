---
title : "Thiết lập các Server cho dự án"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 2.1 </b> "
---

#### Gitlab Server
Chúng ta sẽ tiến hành thiết lập GitLab EE (Enterprise Edition) trên máy chủ nhằm lưu trữ mã nguồn và cấu hình quá trình CI/CD.

Để thực hiện, chúng ta sẽ sử dụng hệ điều hành Ubuntu phiên bản 24.04 để cài đặt trên máy ảo.

- [Link file ISO Ubuntu Server 24.04](https://ubuntu.com/download/server/thank-you?version=24.04&architecture=amd64&lts=true)

Tiến hành tạo máy ảo theo các bước sau:

![alt text](/images/2-preparation/2.1-setupservers/2-1-1.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-2.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-3.png)

Chú ý chọn file ISO vừa tải về.

![alt text](/images/2-preparation/2.1-setupservers/2-1-4.png)

Đặt tên và chọn thư mục lưu trữ Server nhầm mục đích dễ quản lý.

![alt text](/images/2-preparation/2.1-setupservers/2-1-5.png)

Tùy theo cấu hình của máy bạn mà có thể chọn **Processors** và **Cores** phù hợp, tuy nhiên Gitlab cần tối thiểu **2 CPU (Processor Cores)**.

![alt text](/images/2-preparation/2.1-setupservers/2-1-6.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-7.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-8.png)

Tiếp theo chọn theo các cấu hình mặc định.

![alt text](/images/2-preparation/2.1-setupservers/2-1-9.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-10.png)

Tiếp theo thiết lập Ubuntu Server 

![alt text](/images/2-preparation/2.1-setupservers/2-1-11.png)

Tiếp tục chọn theo cấu hình mặc định.
Ở mục **Storage Configuration** thiết lập phân vùng **Root ( / )** bằng toàn bộ dung lượng còn lại.

![alt text](/images/2-preparation/2.1-setupservers/2-1-12.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-13.png)

Cài đặt OpenSSH.

![alt text](/images/2-preparation/2.1-setupservers/2-1-14.png)

Đợi Ubuntu thiết lập thành công và tiến hành Reboot.

![alt text](/images/2-preparation/2.1-setupservers/2-1-15.png)

Sau khi Reboot, đăng nhập vào Server vào username và pass vừa tạo. Chú ý IPv4 khi vừa đăng nhập, dùng để **SSH** vào Server.
![alt text](/images/2-preparation/2.1-setupservers/2-1-16.png)

{{% notice tip %}}
Có thể sử dụng lệnh `ip a` để xem IPv4 và các thông tin khác của Server.
{{% /notice %}}

Mở **CMD** trên Windows để SSH vào Server thông qua IPv4 và Username.

![alt text](/images/2-preparation/2.1-setupservers/2-1-17.png)

Tuy nhiên khi tắt Server và bật lại IPv4 có thể thay đổi, chúng ta tiến hành thiết lập IP tĩnh cho Server.

Chuyển sang quyền Root bằng lệnh `sudo -i` cho quá trình thiết lập dễ thao tác.

Chỉnh sửa cấu hình trong file `50-cloud-init.yaml` bằng lệnh `vi /etc/netplan/50-cloud-init.yaml` và viết theo cấu hình sau:
``` yaml
# This is the network config written by 'subiquity'
network:
  ethernets:
    ens33:
      dhcp4: no
      addresses: [192.168.181.100/24]
      gateway4: 192.168.181.2
      nameservers:
              addresses: [8.8.8.8, 8.8.4.4]
  version: 2
```
Tiến hành lưu file bằng tổ hợp phím sau: `ESC + :wq` và tiến hành chạy lệnh:
```shell
$ netplan apply

$ reboot
```


{{% notice info %}}
Ở dòng `gateway4` là IP Gateway của NAT có thể lấy ở VMware bằng cách truy cập vào **Edit -> Virtual Network Editor -> NAT Setting**
{{% /notice %}}

![alt text](/images/2-preparation/2.1-setupservers/2-1-18.png)

Sau khi chỉnh sửa IP tĩnh thành công tiến hành tạo Snapshot và Clone ra các Server khác.

![alt text](/images/2-preparation/2.1-setupservers/2-1-19.png)

Click phải vào **Gitlab Server** -> **Snapshot** -> **Take Snapshot** và điền các thông tin.

Sau đó chọn theo ảnh và Clone ra các Server khác.

![alt text](/images/2-preparation/2.1-setupservers/2-1-20.png)

Clone Server và thay đổi IP tĩnh, Hostname của các Server theo bảng sau:

| Server             | Hostname           | IP              |
|--------------------|--------------------|-----------------|
| Gitlab Server      | gitlab-server      | 192.168.181.100 |
| Development Server | development-server | 192.168.181.101 |
| Build Server       | build-server       | 192.168.181.102 |
| Database Server    | database-server    | 192.168.181.103 |
