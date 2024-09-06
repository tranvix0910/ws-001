---
title : "Gitlab CI/CD"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 1.2 </b> "
---

### Gitlab
**GitLab** là một nền tảng DevOps toàn diện, tích hợp nhiều công cụ để quản lý toàn bộ vòng đời phát triển phần mềm. Với GitLab, các nhóm phát triển có thể dễ dàng quản lý mã nguồn, tự động hóa quy trình kiểm thử và triển khai thông qua các tính năng **tích hợp liên tục (CI)** và **triển khai liên tục (CD)**. Ngoài ra, GitLab còn cung cấp các công cụ quản lý dự án, theo dõi tiến độ và cộng tác hiệu quả, giúp cải thiện chất lượng và tốc độ phát triển phần mềm.

GitLab có hai phiên bản chính: **GitLab CE (Community Edition)**, một phiên bản mã nguồn mở miễn phí với các tính năng cơ bản, và **GitLab EE (Enterprise Edition)**, phiên bản thương mại cung cấp nhiều tính năng nâng cao và hỗ trợ kỹ thuật chuyên nghiệp. 

### CI/CD

CI/CD là một phương pháp thường xuyên cung cấp ứng dụng cho khách hàng bằng cách đưa tự động hóa vào các giai đoạn phát triển ứng dụng. Khái niệm chính CI/CD là Continuous Integration, Continuous deployment, phân phối liên tục và triển khai liên tục. CI/CD là một giải pháp khăc phục các vấn đề khi tích hợp, deploy code mới trong quá trình dự án vận hành và phát triển

CI/CD giúp tự động hóa, giám sát liên tục trong suốt vòng đời phát triển phần mềm, từ giai đoạn tích hợp và thử nghiệm đến phân phối và triển khai.

Quá trình này là một phần của quy trình DevOps và DevSecOps:

![DevsecopsWorkflow](/images/1-introduce/1.2-gitlabcicd/devsecops-workflow.png)

### Gitlab CI/CD

**GitLab CI/CD (Continuous Integration and Continuous Deployment/Delivery)** là một phần của GitLab, một nền tảng DevOps mã nguồn mở tích hợp cung cấp các công cụ để quản lý toàn bộ vòng đời của một dự án phần mềm (software development life cycle). GitLab CI/CD giúp tự động hóa quy trình xây dựng (Build), kiểm thử (Test) và triển khai mã nguồn (Deploy), giúp cải thiện chất lượng phần mềm và tăng cường hiệu quả làm việc.

Các thành phần chính của GitLab CICD:

- **Pipeline**: Là một tập hợp các Jobs (công việc) được thực thi theo một thứ tự xác định. Mỗi pipeline có thể có nhiều stages (giai đoạn), và mỗi stage có thể có nhiều jobs.

- **Job**: Là một đơn vị công việc cụ thể, ví dụ như biên dịch mã nguồn, chạy kiểm thử, hoặc triển khai ứng dụng. Các jobs trong cùng một stage được thực thi song song, còn các stages được thực thi tuần tự.

- **Runner**: Là các agent chịu trách nhiệm thực thi các jobs. GitLab Runner có thể được cài đặt trên máy chủ riêng hoặc sử dụng các runners được cung cấp bởi GitLab.

- **.gitlab-ci.yml**: Là tệp cấu hình được lưu trữ trong kho mã nguồn (repository) của bạn. Tệp này xác định các pipelines, stages và jobs cần thực thi. Đây là nơi bạn định nghĩa logic CI/CD cho dự án của mình.

Trong bài workshop này chúng ta sẽ tiến hành self-host Gitlab để triển khai dự án đảm bảo tính bảo mật, có thể toàn quyền kiểm soát Gitlab trong môi trường doanh nghiệp phù hợp với thực tế.
