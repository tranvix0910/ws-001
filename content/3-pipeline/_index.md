---
title : "Thiết lập CI/CD Pipeline"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---

### Diagram

![DevsecopsPipeline](/images/devsecops-pipeline.png) 

Sơ đồ này mô tả quy trình CI/CD tự động cho một ứng dụng web sử dụng MERN stack (MongoDB, Express.js, React.js, Node.js). Quy trình này giúp đảm bảo rằng mã nguồn được xây dựng, kiểm thử, quét bảo mật và triển khai một cách liên tục và hiệu quả.

**Push Code (Người dùng đến GitLab):**

- Developer push source code của ứng dụng MERN stack lên kho lưu trữ GitLab. Hành động này sẽ kích hoạt quy trình CI/CD.

**Quy trình CI/CD (GitLab CI/CD):**

- **Commit**: Khi mã nguồn được đẩy lên, một commit được tạo trong kho lưu trữ GitLab.

- GitLab CI/CD pipeline bắt đầu và bao gồm nhiều công việc (jobs) được thực hiện bởi các runner.

**Build Job (GitLab Runner):**

- **Build Frontend** (React.js): GitLab Runner thực hiện việc build ứng dụng frontend bằng React.js, bao gồm cài đặt các dependencies và tạo các tệp tĩnh cần thiết.

- **Build Backend** (Node.js và Express.js): Tương tự, backend được build bằng Node.js và Express.js, đảm bảo tất cả các dependencies được cài đặt và mã nguồn được biên dịch nếu cần.

- **Build Image Docker**: Toàn bộ ứng dụng MERN stack được đóng gói vào một Docker image để dễ dàng triển khai và quản lý.

**Quét Image (Docker và Snyk):**

- Docker image được quét để phát hiện các lỗ hổng bảo mật sử dụng công cụ như Snyk.

- **Gửi Báo cáo (đến Telegram)**: Kết quả quét bảo mật được gửi qua Telegram để thông báo cho nhóm phát triển về tình trạng bảo mật của image.

**Công việc Deploy (GitLab Runner):**

- Nếu quá trình build và quét bảo mật thành công, bước tiếp theo là triển khai ứng dụng.
- Triển khai Frontend và Backend: GitLab Runner triển khai frontend (React.js) và backend (Node.js và Express.js) lên môi trường sản xuất hoặc staging.

**Quản lý Image (Portus):**

- **Pull Image (Portus)**: Docker image được quản lý thông qua Portus, nơi image được kéo từ registry này.

- **Push Image (Portus)**: Sau khi quản lý, image được đẩy lên Harbor để lưu trữ và quét thêm các lỗ hổng bảo mật nếu cần.

**Quét Bảo mật (arachni):**

- Ứng dụng đã triển khai được quét bảo mật bằng công cụ arachni để phát hiện các lỗ hổng trong ứng dụng web.

**Kiểm thử Hiệu năng (k6):**

- Sau khi đảm bảo về bảo mật, kiểm thử hiệu năng được thực hiện bằng công cụ k6 để đảm bảo ứng dụng hoạt động tốt dưới tải cao.

- Gửi Báo cáo: Kết quả kiểm thử hiệu năng được gửi qua Telegram để thông báo cho nhóm phát triển và vận hành.

### Nội Dung

- [Thiết lập Gitlab Runner](3.1-gitlab-runner)
- [Build và Push Docker Image](3.2-build-and-push-image)
- [Security Scan và Performance Test](3.3-security-performance)
- [Gửi Report vào Telegram](3.4-send-report)