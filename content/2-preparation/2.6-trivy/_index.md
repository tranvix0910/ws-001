---
title : "Trivy Security Scan Image"
date :  "`r Sys.Date()`" 
weight : 6 
chapter : false
pre : " <b> 2.6 </b> "
---

#### Cài đặt Docker và Docker Compose
Tạo file script `.sh` để cài đặt Docker và Docker Compose.
```bash
#!/bin/bash
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update -y
sudo apt install docker-ce -y
sudo systemctl start docker
sudo systemctl enable docker
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker -v
docker-compose -v
```
Chạy script trên bằng cách thực hiện lệnh sau:
```
sh <name-file>.sh
```
![alt text](/images/2-preparation/2.6-trivy/2-6-1.png)
Như vậy đã cài đặt thành công Docker và Docker Compose.

#### Cài đặt Trivy thông qua Docker Image
Trivy có các cách sử dụng sau:

```
# Cài đặt Trivy
brew install aquasecurity/trivy/trivy

# Quét một Docker image
trivy image your-docker-image

# Quét file hệ thống
trivy fs /path/to/your/filesystem

# Quét mã nguồn
trivy repo https://github.com/your/repo
```

Trong bài Workshop này chúng ta sẽ cài đặt Trivy thông qua Docker Image.

Chúng ta có thể thực hiện việc test mã nguồn kết hợp với việc kéo Image Trivy từ Docker Hub.
```
docker run aquasec/trivy fs .
```
![alt text](/images/2-preparation/2.6-trivy/2-6-2.png)

Có thể tạo báo cáo quét Security bằng Trivy.

- [Trivy Report Formats](https://aquasecurity.github.io/trivy/v0.28.1/docs/vulnerability/examples/report/)

```
$ trivy image --format template --template "@contrib/html.tpl" -o report.html golang:1.12-alpine
```









