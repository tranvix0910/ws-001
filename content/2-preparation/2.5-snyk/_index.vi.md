---
title : "Source Code Security - Snyk"
date :  "`r Sys.Date()`" 
weight : 5 
chapter : false
pre : " <b> 2.5 </b> "
---
#### Cài đặt NodeJS và npm
```shell
sudo apt install npm -y
```
Kiểm tra Version.
```shell
tranvi0910@deployment-server:~$ node -v
v18.20.4
```
```shell
tranvi0910@deployment-server:~$ npm -v
v10.7.0
```

#### Cài đặt Snyk với npm
Cài đặt Snyk ở Server **Build**.
```sh
npm install snyk -g

# Format HTML

npm install snyk-to-html -g
```
```shell script
root@build-server:~# npm install snyk -g

added 36 packages in 56s

12 packages are looking for funding
  run `npm fund` for details
root@development-server:~# npm install snyk-to-html -g

added 23 packages in 5s

1 package is looking for funding
  run `npm fund` for details
```

Đăng nhập vào trang web chính thức của Snyk và tiến hành xác thực.

- [Link Snyk](https://app.snyk.io/)

![alt text](/images/2-preparation/2.5-snyk/2-5-1.png)
![alt text](/images/2-preparation/2.5-snyk/2-5-2.png)

Lấy **API Token** từ Account:
![alt text](/images/2-preparation/2.5-snyk/2-5-3.png)

Thực hiện câu lệnh sau ở **Server Development** để tiến hành xác thực Snyk.
```sh
snyk auth ab089484-2b45-4f10-b991-xxxxxxxxxxx
```
```sh
root@build-server:~# snyk auth ab089484-2b45-4f10-b991-d101f237fecb
Executable doesn't exist, trying to download.
Downloading from 'https://static.snyk.io/cli/v1.1292.1/snyk-linux' to '/usr/lib/node_modules/snyk/wrapper_dist/snyk-linux'
Shasums:
- actual:   xxxxxxxxxxxxxxx
- expected: xxxxxxxxxxxxxxx
Downloaded successfull!

Your account has been authenticated. Snyk is now ready to be used.
```

Di chuyển vào thư mục chứa dự án và tiến hành test:
```
cd /projects/wineapp/wineapp-frontend

snyk test
```

![alt text](/images/2-preparation/2.5-snyk/2-5-4.png)

Có thể xem chi tiết các vấn đề bằng cách sử dụng lệnh sau:
```
snyk monit
```
![alt text](/images/2-preparation/2.5-snyk/2-5-5.png)

![alt text](/images/2-preparation/2.5-snyk/2-5-6.png)

Có thể test và lưu ra 1 file HTML.
```sh
snyk test --json | snyk-to-html -o <name_file>.html
```
![alt text](/images/2-preparation/2.5-snyk/2-5-7.png)

Như vậy đã cài đặt và thiết lập Snyk thành công.

