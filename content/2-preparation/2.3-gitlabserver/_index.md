---
title : "Gitlab Server"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 2.3 </b> "
---

#### Cài đặt Gitlab
Tại **Gitlab Server** chúng ta đã tạo, tiến hành cài đặt và thiết lập các Dependency cần thiết.

```sh
sudo apt-get update
sudo apt-get install -y curl openssh-server ca-certificates tzdata perl
```

Thêm GitLab package repository và cài đặt package.
```sh
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash

```

Ở đây chúng ta cài đặt **Gitlab Self-host** có nghĩa là bạn thiết lập và chạy GitLab trên máy chủ của riêng mình, thay vì sử dụng dịch vụ lưu trữ của GitLab.

Điều này giúp bạn có toàn quyền kiểm soát môi trường GitLab, bao gồm việc quyết định nơi lưu trữ, cách cấu hình và ai có quyền truy cập.

Tiếp theo, cài đặt gói GitLab. Theo DNS mà bạn muốn dùng để truy cập vào Gitlab Server.

```sh
sudo EXTERNAL_URL="http://gitlab.tranvix.vn" apt-get install gitlab-ee
```
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-1.png)

Như vậy Gitlab đã cài đặt thành công.

#### Cấu hình Gitlab

Tiến hành Add Host ở máy Windows.

Mở Notepad với quyền quản trị viên.

Mở tệp `hosts` ở địa chỉ `C:\Windows\System32\drivers\etc`

Ở cuối tệp hosts, thêm một dòng mới với định dạng sau:
```
<IP address>   <domain name>
```
Chúng ta sẽ thêm vào file địa chỉ IP máy ảo Gitlab Server và Domain chúng ta dùng truy cập vào Gitlab.
```
192.168.181.101   gitlab.tranvix.vn
```
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-2.png)

Tiến hành truy cập vào Gitlab thông qua Domain `http://gitlab.tranvix.vn`

![alt text](/images/2-preparation/2.3-gitlabserver/2-3-3.png)

Lấy Password bằng cách thực hiện câu lệnh sau và tiến hành đăng nhập với Username `root` và Password vừa lấy:
```sh
cat /etc/gitlab/initial_root_password
```
```sh
root@gitlab-server:~# cat /etc/gitlab/initial_root_password
# WARNING: This value is valid only in the following conditions
#          1. If provided manually (either via `GITLAB_ROOT_PASSWORD` environment variable or via `gitlab_rails['initial_root_password']` setting in `gitlab.rb`, it was provided before database was seeded for the first time (usually, the first reconfigure run).
#          2. Password hasn't been changed manually, either via UI or via command line.
#
#          If the password shown here doesn't work, you must reset the admin password following https://docs.gitlab.com/ee/security/reset_user_password.html#reset-your-root-password.

Password: frA2Y0Aquj3AS3hLvKowNvUkMyfHD6mU4IdhDnOaSGU=

# NOTE: This file will be automatically deleted in the first reconfigure run after 24 hours.
```
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-4.png)
Như vậy đã đăng nhập thành công.

#### Tạo Group, User và Project.
Đầu tiên chúng ta tiến hành tạo Group để chứa dự án.
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-5.png)
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-6.png)
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-7.png)
Tiếp theo chúng ta sẽ tiến hành tạo User và thêm User và Group.

Truy cập `Admin` và chọn `New User`
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-8.png)
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-9.png)
Sau khi tạo User thành công tiến hành tạo Password.
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-10.png)
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-11.png)
Thêm User vào Group.
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-12.png)
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-13.png)

Truy cập vào Group, Tiến hành tạo Project.
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-14.png)
Chọn `Create blank project`.
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-15.png)
Đặt tên cho Project và chú ý bỏ chọn `Initialize repository with a README` phù hợp nếu bạn muốn đẩy một kho lưu trữ hiện có lên hoặc tự tạo các tệp sau.
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-16.png)
Tạo Project thành công.
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-17.png)
Thực hiện theo hướng dẫn có trên ảnh và tiến hành Push code dự án hiện có lên Gitlab.

Tải về dự án tại link bên dưới và di chuyển đến Server Development để có thể push code vào Gitlab.

Tại thư mục lưu trữ dự án ở Windows của bạn ấn chuột phải, tiến hành mở Terminal và chạy lệnh sau:

![alt text](/images/2-preparation/2.3-gitlabserver/2-3-18.png)

```
scp .\* tranvi0910@192.168.181.102:/home/tranvi0910
```
Lệnh dùng để sao chép tất cả tệp `./*` và thư mục từ thư mục hiện tại trên máy cục bộ sang thư mục `/home/tranvi0910` trên máy chủ từ xa có địa chỉ IP `192.168.181.102`, với tài khoản người dùng `tranvi0910`.

![alt text](/images/2-preparation/2.3-gitlabserver/2-3-19.png)

Tiến hành kiểm tra ở Server Development.

![alt text](/images/2-preparation/2.3-gitlabserver/2-3-20.png)

Tạo thư mục mới để clone project về, sau đó giải nén source code chúng ta vừa di chuyển từ máy Windows qua Server và đẩy code lên Gitlab.

```
mkdir -p /projects/wineapp && cd /projects/wineapp
```

Tiến hành Add Host ở Server Development để có thể clone dự án từ Gitlab.
```
vi /etc/hosts
```
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-21.png)

Clone Project.
```
git clone http://gitlab.tranvix.vn/wineapp/wineapp-frontend.git
```
```shell
root@development-server:/projects/wineapp# git clone http://gitlab.tranvix.vn/wineapp/wineapp-frontend.git
Cloning into 'wineapp-frontend'...
Username for 'http://gitlab.tranvix.vn': wineappdev
Password for 'http://wineappdev@gitlab.tranvix.vn':
warning: You appear to have cloned an empty repository.
```
Cài đặt package **Unzip**.
```
sudo apt install unzip
```

Giải nén file zip source code Frontend.
```
unzip wine-website-FE.zip
unzip wine-website-BE.zip
```

Sau khi giải nén xong chúng ta có 2 thư mục.
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-22.png)

Di chuyển Source Code từ thư mục chúng ta vừa giải nén vào Project Frontend vừa Clone về.
```sh
cp -rf wine-website-FE-main/* /projects/wineapp/wineapp-frontend/
```
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-23.png)

Push Code.

```sh
git add .

git commit -m 'project(base): add base project'

git push
```

Kiểm tra dự án tại Gitlab.
![alt text](/images/2-preparation/2.3-gitlabserver/2-3-24.png)

Như vậy dự án đã được đẩy lên Gitlab thành công, thực hiện tương tự với **Project Backend**