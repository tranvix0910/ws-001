---
title : "Private Container Registry - Portus"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2.2 </b> "
---

#### Cài Đặt Portus
Trước khi tạo Server Portus bạn cần thuê một Domain để có thể truy cập Portus thông qua Domain.
```
portus.tranvix.online
```
Chúng ta sẽ thực hiện tạo **EC2 Instance** để tiến hành cài đặt Portus.

Để có thể tạo Instance chúng ta cần chuẩn bị một tài khoản AWS và truy cập vào dịch vụ EC2.

Chọn `Launch Instance` để tạo Instance.

![alt text](/images/2-preparation/2.2-containerregistry/2-2-1.png)

Thiết lập tên của Server.
![alt text](/images/2-preparation/2.2-containerregistry/2-2-2.png)

Chọn hệ điều hành, ở đây mình chọn Ubuntu 24.04.
![alt text](/images/2-preparation/2.2-containerregistry/2-2-3.png)

Chọn cấu hình Instance phù hợp và thiết lập Key Pair.

{{% notice warning %}}
Khi tạo Key Pair, Private Key sẽ được tải về máy, chú ý không để mất và không được chia sẽ cho ai.
{{% /notice %}}
![alt text](/images/2-preparation/2.2-containerregistry/2-2-4.png)

Chú ý tick chọn `Allow HTTPS traffic from the internet`. 

Thiết lập này cho phép mọi người truy cập máy chủ web một cách an toàn từ bất cứ đâu thông qua HTTPS.
![alt text](/images/2-preparation/2.2-containerregistry/2-2-5.png)

Chọn dung lượng cho Instance.
![alt text](/images/2-preparation/2.2-containerregistry/2-2-6.png)

Sau khi tạo Instance thành công, Tiến hành truy cập vào Instance.
![alt text](/images/2-preparation/2.2-containerregistry/2-2-7.png)
![alt text](/images/2-preparation/2.2-containerregistry/2-2-8.png)

Như vậy là đã truy cập thành công vào Instance.
![alt text](/images/2-preparation/2.2-containerregistry/2-2-9.png)

Để cài đặt Portus chúng ta cần cài đặt một số công cụ cần thiết.
```
sudo apt install -y docker.io docker-compose certbot net-tools
```
***Lấy chứng chỉ SSL/TLS bằng Certbot***: Certbot tự động hóa quy trình đăng ký chứng chỉ từ Let's Encrypt. Chỉ cần cung cấp thông tin cần thiết và Certbot như Domain và Email.

Chạy một lệnh Certbot để lấy chứng chỉ SSL cho một domain cụ thể. 
```
certbot certonly --standalone -d portus.tranvix.online --preferred-challenges http --agree-tos -m your-email@gmail.com --keep-until-expiring
```
```
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
shellare your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y
Account registered.
Requesting a certificate for portus.tranvix.online

Successfully received certificate.
Certifiate is saved at: /etc/letsencrypt/live/portus.tranvix.online/fullchain.pem     <----------------
Key is saved at:         /etc/letsencrypt/live/portus.tranvix.online/privkey.pem      <----------------
This certificate expires on 2024-10-16.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```
Chú ý đường dẫn tới **Certifiate** và **Key** vừa tạo.

Clone Portus từ Github.
``` 
git clone https://github.com/SUSE/Portus.git
```
Di chuyển đến thư mục cài đặt Portus.
```
cd Portus/examples/compose
```
```
root@ip-172-31-44-184:/tools/portus/Portus/examples/compose# ls -l
total 48
-rw-r--r-- 1 root root 3715 Jul 18 10:31 README.md
drwxr-xr-x 2 root root 4096 Jul 18 10:31 clair
-rw-r--r-- 1 root root 4345 Jul 18 10:31 docker-compose.clair-ssl.yml
-rw-r--r-- 1 root root 3540 Jul 18 10:31 docker-compose.clair.yml
-rw-r--r-- 1 root root 3048 Jul 18 10:31 docker-compose.insecure.yml
-rw-r--r-- 1 root root 4640 Jul 18 10:31 docker-compose.ldap.yml
-rw-r--r-- 1 root root 3656 Jul 18 10:31 docker-compose.yml
drwxr-xr-x 2 root root 4096 Jul 18 10:31 nginx
drwxr-xr-x 2 root root 4096 Jul 18 10:31 registry
drwxr-xr-x 2 root root 4096 Jul 18 10:31 secrets
```

Tiến hành cấu hình **Nginx**:

```
vi nginx/nginx.conf
```

```
ssl on; ------> Comment dòng này 

# Certificates
ssl_certificate /secrets/portus.crt;        <-------
ssl_certificate_key /secrets/portus.key;    <-------
```

Chú ý đường dẫn tới 2 Key: `/secrets/portus.crt`, `/secrets/portus.key`.

Tiến hành copy và đổi tên cặp key SSL mà chúng ta đã tạo ở trên vào 2 thư mục này.

```shell
root@ip-172-31-44-184:/tools/portus/compose# cp /etc/letsencrypt/live/portus.tranvix.online/fullchain.pem secrets/portus.crt
root@ip-172-31-44-184:/tools/portus/compose# cp /etc/letsencrypt/live/portus.tranvix.online/privkey.pem secrets/portus.key
```
Cấu hình file `.env`
```shell
root@ip-172-31-44-184:/tools/portus/compose# cat .env
MACHINE_FQDN=172.17.0.1 <--------

SECRET_KEY_BASE=b494a25faa8d22e430e843e220e424e10ac84d2ce0e64231f5b636d21251eb6d267adb042ad5884cbff0f3891bcf911bdf8abb3ce719849ccda9a4889249e5c2
PORTUS_PASSWORD=12341234
DATABASE_PASSWORD=portus
```
Thay đổi địa chỉ localhost thành Domain của chúng ta.

```shell
MACHINE_FQDN=portus.tranvix.online

SECRET_KEY_BASE=b494a25faa8d22e430e843e220e424e10ac84d2ce0e64231f5b636d21251eb6d267adb042ad5884cbff0f3891bcf911bdf8abb3ce719849ccda9a4889249e5c2
PORTUS_PASSWORD=12341234
DATABASE_PASSWORD=portus
```

Tiến hành chạy file `docker-compose.clair-ssl.yml`.

```shell
docker-compose -f docker-compose.clair-ssl.yml up -d
```

![alt text](/images/2-preparation/2.2-containerregistry/2-2-10.png)
Như vậy Portus đã cài đặt thành công.

#### Cấu Hình Portus
Thông qua IP Public từ Instance vừa tạo chúng ta có thể tạo Record từ Domain để trỏ tới Server Portus để truy cập Instance thông qua Domain.
![alt text](/images/2-preparation/2.2-containerregistry/2-2-11.png)
Tiến hành truy cập vào Domain.
![alt text](/images/2-preparation/2.2-containerregistry/2-2-12.png)
Tạp User Admin và đăng nhập.
Tạo Registry.
![alt text](/images/2-preparation/2.2-containerregistry/2-2-13.png)
![alt text](/images/2-preparation/2.2-containerregistry/2-2-14.png)
Tạo User và Group cho dự án.
![alt text](/images/2-preparation/2.2-containerregistry/2-2-15.png)
![alt text](/images/2-preparation/2.2-containerregistry/2-2-16.png)
Cấu hình User vừa tạo có quyền Admin.
![alt text](/images/2-preparation/2.2-containerregistry/2-2-17.png)
Tạo Group.
![alt text](/images/2-preparation/2.2-containerregistry/2-2-18.png)
![alt text](/images/2-preparation/2.2-containerregistry/2-2-19.png)
Như vậy đã tạo User và Group thành công.

Vậy chúng ta đã vừa tạo và thiết lập thành công Private Registry với Portus.