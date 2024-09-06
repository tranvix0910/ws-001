---
title : "Thiết lập dự án"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---

### Tổng quan
![alt text](/images/2-preparation/2-0-0.png)
Trong buổi Workshop này, chúng ta sẽ cùng nhau triển khai (deploy) một ứng dụng web sử dụng MERN Stack. 

MERN Stack bao gồm bốn thành phần chính: MongoDB, Express.js, React, và Node.js.

- **MongoDB** – NoSQL database.
- **ExpressJS** – Backend web-application framework cho NodeJS.
- **ReactJS** – JavaScript library dùng cho phát triển UIs từ UI components. 
- **NodeJS** – Môi trường runtime cho JavaScript, cho phép chạy mã JavaScript bên ngoài trình duyệt, cùng với các tính năng khác.

Chúng ta sẽ triển khai dự án trong môi trường On-premise bằng cách tạo các máy chủ Ubuntu thông qua nền tảng ảo hóa VMware.

Dưới đây là danh sách các Server cần thiết:

| #   | OS     | Version | Server            | Ram  | CPU | Disk | IP              | Username   | Domain             | Description |
| --- | ------ | ------- | ----------------- | ---- | --- | ---- | --------------- | ---------- | ------------------ | ----------- |
| 1   | Ubuntu | 24.04   | Gitlab-Server      | 3Gb  | 2   | 20Gb | 192.168.181.101 | tranvi0910 | gitlab.tranvix.vn  |             |
| 2   | Ubuntu | 24.04   | Development Server | 2Gb  | 1   | 20Gb | 192.168.181.102 | tranvi0910 |                    |             |
| 3   | Ubuntu | 24.04   | Build Server       | 2Gb  | 1   | 20Gb | 192.168.181.103 | tranvi0910 |                    |             |

- **Gitlab Server** : 
    - Đây là server chính dùng để chạy GitLab, một nền tảng quản lý mã nguồn và DevOps. GitLab cung cấp các tính năng như quản lý dự án, kho lưu trữ mã nguồn, CI/CD (Continuous Integration/Continuous Deployment).
    - **IP & Domain**: 192.168.181.100, gitlab.tranvix.vn - Cho phép truy cập GitLab qua địa chỉ IP cục bộ hoặc domain `gitlab.tranvix.vn`.

- **Development Server** :
    - Server này được sử dụng cho môi trường phát triển. Các nhà phát triển có thể triển khai và kiểm thử mã nguồn tại đây trước khi đưa lên GitLab hoặc các môi trường khác.

- **Build Server** :
    - Server này dành riêng cho việc xây dựng (build) mã nguồn từ các dự án. Nó có thể tích hợp với GitLab để tự động hóa quá trình build khi có mã mới được đẩy lên.

- **Database Server** :
    - Đây là server chuyên dụng để lưu trữ cơ sở dữ liệu. Các ứng dụng khác, chẳng hạn như GitLab hoặc các ứng dụng trên Development Server, sẽ kết nối đến server này để truy vấn và lưu trữ dữ liệu.

Việc sử dụng domain name thay vì địa chỉ IP cụ thể trong mục Domain có nhiều lợi ích, đặc biệt là trong bối cảnh truy cập GitLab.
Domain name như `gitlab.tranvix.vn` dễ nhớ hơn so với một địa chỉ IP như `192.168.181.101`. Điều này giúp người dùng dễ dàng truy cập vào GitLab mà không cần phải ghi nhớ hoặc tra cứu IP mỗi khi cần.

### Nội dung

1. [Thiết lập các Server cho dự án](2.1-setupservers)
2. [Private Container Registry - Portus](2.2-containerregistry)
3. [Gitlab Server](2.3-gitlabserver)
4. [Database Server](2.4-database)
5. [Source Code Security - Snyk](2.5-snyk)
6. [Trivy Security Scan Image](2.6-trivy)
7. [Web Application Security Scan](2.7-arachni)
8. [Performance Testing - k6](2.8-k6)