---
title : "Thiết lập các Server cho dự án"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 2.1 </b> "
---

#### Cài Đặt Ubuntu Server

Đầu tiên, chúng ta sẽ sử dụng hệ điều hành Ubuntu phiên bản 24.04 để cài đặt trên máy ảo.

- [Link file ISO Ubuntu Server 24.04](https://ubuntu.com/download/server/thank-you?version=24.04&architecture=amd64&lts=true)

Chúng ta sẽ tạo máy ảo trên ứng dụng **VMware Workstation**, để cài đặt ứng dụng có thể tham khảo đường link sau:

- [Set up VMware Workstation Pro 17 on Windows 10/11](https://thelinuxforum.com/articles/960-how-to-install-free-vmware-workstation-pro-17-on-windows-10-11)

Tiến hành tạo máy ảo trên ứng dụng **VMware Workstation** thực hiện theo các bước sau:

![alt text](/images/2-preparation/2.1-setupservers/2-1-1.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-2.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-3.png)

Chọn file ISO Ubuntu Server chúng ta vừa tải về.

![alt text](/images/2-preparation/2.1-setupservers/2-1-4.png)

Đặt tên máy ảo và chọn thư mục do bạn quản lý.

![alt text](/images/2-preparation/2.1-setupservers/2-1-5.png)

Tùy theo cấu hình của máy bạn mà có thể chọn **Processors** và **Cores** phù hợp, tuy nhiên Gitlab cần tối thiểu **2 CPU (Processor Cores)**.

![alt text](/images/2-preparation/2.1-setupservers/2-1-6.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-7.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-8.png)

Tiếp theo chọn theo các cấu hình mặc định.

![alt text](/images/2-preparation/2.1-setupservers/2-1-9.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-10.png)

Sau khi cài đặt máy ảo thành công, chúng ta khởi động máy ảo và tiến hành cấu hình Ubuntu Server.

![alt text](/images/2-preparation/2.1-setupservers/2-1-11.png)

Tiếp tục chọn theo cấu hình mặc định.

Ở mục **Storage Configuration** thiết lập phân vùng **Root ( / )** bằng toàn bộ dung lượng còn lại.

![alt text](/images/2-preparation/2.1-setupservers/2-1-12.png)
![alt text](/images/2-preparation/2.1-setupservers/2-1-13.png)

Tick chọn cài đặt OpenSSH.

![alt text](/images/2-preparation/2.1-setupservers/2-1-14.png)

Đợi Ubuntu thiết lập thành công và tiến hành Reboot.

![alt text](/images/2-preparation/2.1-setupservers/2-1-15.png)

Sau khi Reboot, đăng nhập vào Server vào username và pass vừa tạo. **Chú ý IPv4 khi vừa đăng nhập, dùng để SSH vào Server**.
![alt text](/images/2-preparation/2.1-setupservers/2-1-16.png)

{{% notice tip %}}
Có thể sử dụng lệnh `ip a` để xem IPv4 và các thông tin khác của Server.
{{% /notice %}}

Mở **CMD** trên Windows để SSH vào Server thông qua IPv4 và Username.

![alt text](/images/2-preparation/2.1-setupservers/2-1-17.png)

Tuy nhiên khi tắt Server và bật lại IPv4 có thể thay đổi nên chúng ta cần thiết lập IP tĩnh cho Server. Khi khởi động lại IP vẫn giữ nguyên giúp chúng ta dễ cấu hình hơn.

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

Lưu file lại và thực hiện các câu lệnh sau:

```shell
$ netplan apply

$ reboot
```

{{% notice info %}}
Dòng **gateway4** là IP Gateway của NAT. Chúng ta có thể lấy nó ở VMware Workstation bằng cách truy cập vào **Edit -> Virtual Network Editor -> NAT Setting**
{{% /notice %}}

![alt text](/images/2-preparation/2.1-setupservers/2-1-18.png)

Sau khi chỉnh sửa IP tĩnh thành công chúng ta sẽ tiến hành tạo **Snapshot** và **Clone** ra các Server khác.

![alt text](/images/2-preparation/2.1-setupservers/2-1-19.png)

Click chuột phải vào **Gitlab Server** -> **Snapshot** -> **Take Snapshot** và điền các thông tin.

![alt text](/images/2-preparation/2.1-setupservers/2-1-21.png)

![alt text](/images/2-preparation/2.1-setupservers/2-1-22.png)

Như vậy chúng ta đã tạo Snapshot thành công, sau đó chúng ta sẽ thực hiện Clone Server.

Truy cập và Snapshot Manager và thực hiện các bước sau:

![alt text](/images/2-preparation/2.1-setupservers/2-1-23.png)

![alt text](/images/2-preparation/2.1-setupservers/2-1-20.png)

Sau đó chúng ta sẽ thực hiện cấu hình Server theo bảng sau:

| Server             | Hostname           | IP Tĩnh         |
|--------------------|--------------------|-----------------|
| Gitlab Server      | gitlab-server      | 192.168.181.101 |
| Development Server | development-server | 192.168.181.102 |
| Build Server       | build-server       | 192.168.181.103 |
