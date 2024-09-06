---
title : "DevSecOps Tools"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 1.3 </b> "
---

### Table of Content

1. [Gitlab Runner](#gitlab-runner)
2. [Docker](#docker)
3. [Snyk](#snyk)
4. [Trivy Scan Image](#trivy-scan-image)
5. [Portus](#portus)
6. [Arachni](#arachni)
7. [k6](#k6)


### Gitlab Runner

**Gitlab Runner** là một công cụ mã nguồn mở, được viết bằng ngôn ngữ Go và do chính Gitlab tạo ra để phục vụ cho việc CI/CD cho các dự án được tạo trên Gitlab. Người dùng có thể tự cài đặt cho mình một runner trên server của mình hoặc sử dụng những runner do Gitlab cung cấp. Người dùng chỉ cần tạo 1 file `.gitlab-ci.yml` ở thư mục gốc của dự án, để khởi tạo CI/CD pipeline và chỉ định Gitlab runner nào sẽ được sử dụng.

#### Các loại Gitlab Runner
**Shared Runner**

- Shared runners được chia sẻ giữa tất cả các dự án trong một instance GitLab. Chúng có thể được sử dụng bởi bất kỳ dự án nào mà không cần cấu hình đặc biệt.

- **Ưu điểm**: Tiết kiệm tài nguyên và dễ dàng quản lý, đặc biệt hữu ích cho các dự án nhỏ hoặc các dự án không đòi hỏi tài nguyên đặc biệt.

- **Cách triển khai**: Quản trị viên của instance GitLab sẽ cấu hình và đăng ký shared runners.

**Group Runner**

- Group runners chỉ được sử dụng bởi các dự án nằm trong cùng một nhóm GitLab. Điều này giúp kiểm soát và phân phối tài nguyên cho các dự án liên quan.

- **Ưu điểm**: Cung cấp sự kiểm soát tốt hơn về việc sử dụng tài nguyên giữa các dự án trong cùng một nhóm, đồng thời dễ dàng quản lý và theo dõi.

- **Cách triển khai**: Chủ sở hữu của nhóm sẽ cấu hình và đăng ký group runners thông qua phần cài đặt của nhóm.

**Specific Runner**

- Specific runners được gán trực tiếp cho một dự án cụ thể và chỉ phục vụ cho dự án đó. Điều này giúp đảm bảo rằng tài nguyên được dành riêng cho dự án này.

- **Ưu điểm**: Đảm bảo tài nguyên được tối ưu và bảo mật hơn cho các dự án yêu cầu cấu hình đặc biệt hoặc có nhu cầu tài nguyên cao.

- **Cách triển khai**: Chủ sở hữu của dự án sẽ cấu hình và đăng ký specific runners thông qua phần cài đặt CI/CD của dự án.

Mỗi loại runner đều có những ưu điểm và hạn chế riêng, tùy thuộc vào nhu cầu và quy mô của dự án mà bạn có thể chọn loại runner phù hợp để triển khai.

### Docker
**Docker** là một nền tảng mã nguồn mở cho phép tạo, triển khai và quản lý các ứng dụng trong môi trường **container** hóa. Với Docker, các ứng dụng và các thành phần phụ thuộc của chúng được đóng gói vào các container, giúp đảm bảo tính nhất quán khi chạy trên mọi môi trường từ phát triển đến sản xuất. 

**Docker** cung cấp tính di động cao, hiệu suất tối ưu, và đơn giản hóa quy trình DevOps, giúp các nhóm phát triển và vận hành làm việc hiệu quả hơn. Các container nhẹ hơn so với máy ảo, khởi động nhanh hơn và có thể được chia sẻ qua Docker Hub, nơi chứa hàng ngàn hình ảnh container có sẵn.
#### Quy trình tạo một container với Docker
1. **Viết Dockerfile**: Tạo một tệp `Dockerfile` chứa các chỉ thị để Docker biết cách xây dựng hình ảnh (image) cho ứng dụng, bao gồm việc chọn base image, cài đặt các phụ thuộc, và thiết lập cấu hình.

2. **Xây dựng hình ảnh Docker**: Sử dụng lệnh docker build để tạo ra một hình ảnh Docker từ Dockerfile.

3. **Chạy container**: Sau khi tạo xong hình ảnh, sử dụng lệnh `docker run` để tạo và chạy một container từ hình ảnh đó.

4. **Kiểm tra và quản lý container**: Sử dụng các lệnh như `docker ps`, `docker logs`, và `docker exec` để kiểm tra và quản lý container đang chạy.

Docker giúp đơn giản hóa việc phát triển, triển khai và vận hành các ứng dụng bằng cách sử dụng công nghệ container hóa. Điều này giúp đảm bảo tính nhất quán giữa các môi trường khác nhau và tối ưu hóa tài nguyên hệ thống.

### Snyk
**Snyk** là một nền tảng bảo mật dành cho nhà phát triển, giúp **phát hiện và khắc phục các lỗ hổng bảo mật trong mã nguồn**, các thư viện mã nguồn mở, container, và cơ sở hạ tầng dưới dạng mã (Infrastructure as Code - IaC). Snyk tích hợp sâu vào quy trình phát triển phần mềm, cho phép các nhà phát triển kiểm tra và sửa chữa các lỗ hổng bảo mật ngay từ giai đoạn phát triển, trước khi phần mềm được triển khai.

#### Các tính năng chính của Snyk:
1. **Quét mã nguồn mở (Open Source Security)**: Snyk có thể quét các dự án mã nguồn mở và phát hiện các lỗ hổng bảo mật trong các thư viện và phụ thuộc mà dự án sử dụng. Nó cung cấp các giải pháp cụ thể để sửa chữa các lỗ hổng này.

2. **Quét container (Container Security)**: Snyk kiểm tra các hình ảnh Docker để tìm ra các lỗ hổng bảo mật và đề xuất các giải pháp tối ưu, giúp giảm thiểu rủi ro khi triển khai ứng dụng trong container.

3. **Quét mã nguồn (Code Security)**: Snyk có khả năng quét mã nguồn của bạn để tìm các lỗ hổng bảo mật tiềm ẩn và giúp phát hiện các vấn đề ngay từ giai đoạn viết mã.

4. **Quét IaC (Infrastructure as Code Security)**: Snyk có thể kiểm tra các tệp cấu hình cơ sở hạ tầng (như Terraform, AWS CloudFormation) để đảm bảo chúng không chứa các lỗi cấu hình bảo mật.

5. **Tích hợp DevOps**: Snyk dễ dàng tích hợp vào các công cụ CI/CD như Jenkins, GitLab CI, CircleCI, cũng như các IDE và hệ thống quản lý mã nguồn như GitHub, GitLab, Bitbucket.

Snyk giúp các nhà phát triển và đội ngũ DevOps chủ động bảo vệ phần mềm và hệ thống của mình bằng cách phát hiện sớm và sửa chữa các lỗ hổng bảo mật trong suốt quá trình phát triển và triển khai.

### Trivy Scan Image
**Trivy** là một công cụ mã nguồn mở dùng để **quét các lỗ hổng bảo mật và các vấn đề cấu hình trong các container**, mã nguồn mở, và cơ sở hạ tầng dưới dạng mã (Infrastructure as Code - IaC). Trivy có khả năng quét các hình ảnh container để phát hiện các lỗ hổng bảo mật, giúp bảo vệ ứng dụng trước các rủi ro an ninh tiềm ẩn.

#### Các tính năng chính của Trivy:
1. **Quét hình ảnh Docker**: Trivy có thể quét các lớp trong hình ảnh Docker để phát hiện các lỗ hổng bảo mật đã biết. Điều này giúp đảm bảo rằng các hình ảnh container mà bạn triển khai không chứa các vấn đề bảo mật nghiêm trọng.

2. **Tích hợp dễ dàng**: Trivy có thể tích hợp vào quy trình CI/CD để tự động quét các hình ảnh container trước khi chúng được triển khai, giúp phát hiện các vấn đề bảo mật sớm trong quá trình phát triển.

3. **Hỗ trợ nhiều ngôn ngữ và hệ thống**: Ngoài container, Trivy còn hỗ trợ quét các kho mã nguồn mở và cơ sở hạ tầng dưới dạng mã, bao gồm Terraform, CloudFormation và Kubernetes.

4. **Kết quả chi tiết và dễ hiểu**: Trivy cung cấp các báo cáo chi tiết về các lỗ hổng, bao gồm mức độ nghiêm trọng (Critical, High, Medium, Low), phiên bản bị ảnh hưởng, và nếu có, cách khắc phục lỗ hổng.

Trivy là một công cụ bảo mật mạnh mẽ và dễ sử dụng, giúp các nhà phát triển và nhóm DevOps bảo vệ ứng dụng của mình bằng cách phát hiện sớm các lỗ hổng bảo mật trong các hình ảnh container, mã nguồn mở, và cơ sở hạ tầng dưới dạng mã.

### Portus
**Portus** là một giao diện quản lý và **dịch vụ bảo mật cho các registry Docker** có thể self-host, giúp quản lý và kiểm soát các hình ảnh Docker trong một tổ chức. Được phát triển bởi SUSE, Portus cung cấp các tính năng bổ sung mà Docker Registry không có, như kiểm soát truy cập, giám sát, và quản lý người dùng. Điều này giúp đảm bảo rằng chỉ những người hoặc dịch vụ được ủy quyền mới có thể truy cập và quản lý các hình ảnh container.

#### Các tính năng chính của Portus:
1. **Quản lý người dùng và nhóm**: Portus cho phép quản lý người dùng và nhóm trong tổ chức, giúp thiết lập quyền truy cập khác nhau dựa trên vai trò của từng người dùng.

2. **Kiểm soát truy cập**: Với Portus, bạn có thể thiết lập các chính sách truy cập cho các dự án hoặc kho chứa (repositories) riêng biệt. Điều này giúp bảo vệ các hình ảnh quan trọng bằng cách hạn chế quyền truy cập.

3. **Tích hợp Docker Registry**: Portus tích hợp trực tiếp với Docker Registry, cung cấp một giao diện thân thiện và dễ sử dụng để quản lý các hình ảnh Docker. 

4. **Thông báo và giám sát**: Portus cung cấp khả năng giám sát và gửi thông báo về các hoạt động trong Docker Registry. Điều này giúp các quản trị viên dễ dàng theo dõi các sự kiện quan trọng, như việc đẩy hoặc kéo hình ảnh.

Portus là một giải pháp quản lý và bảo mật cho Docker Registry, cung cấp các tính năng mở rộng như kiểm soát truy cập, quản lý người dùng và giám sát, giúp các tổ chức quản lý hình ảnh Docker một cách an toàn và hiệu quả.

### Arachni
**Arachni** là một công cụ mã nguồn mở dùng để **quét và phát hiện các lỗ hổng bảo mật trong ứng dụng web**. Nó có khả năng phát hiện nhiều lỗ hổng phổ biến như **SQL Injection, XSS, và CSRF**. 

Arachni cung cấp cả giao diện dòng lệnh (CLI) và giao diện web, giúp người dùng dễ dàng tích hợp vào các quy trình bảo mật. 

Công cụ này cũng hỗ trợ mở rộng thông qua các plugin hoặc module tùy chỉnh và cung cấp các báo cáo chi tiết ở nhiều định dạng như HTML, JSON, và XML.

### k6
**k6** là một công cụ **kiểm thử tải và đo hiệu suất mã nguồn mở**, được phát triển bởi Grafana Labs. Với k6, người dùng có thể viết các kịch bản kiểm thử bằng JavaScript và thực hiện các bài kiểm thử hiệu suất với khả năng tạo ra hàng ngàn yêu cầu mỗi giây. 

**k6** không yêu cầu giao diện đồ họa, phù hợp cho việc tự động hóa và tích hợp vào các hệ thống CI/CD. Công cụ này cũng hỗ trợ tích hợp với các dịch vụ giám sát hiệu suất như Grafana và Prometheus để theo dõi và phân tích kết quả kiểm thử.



