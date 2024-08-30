---
title : "Quy trình DevSecOps"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1.1 </b> "
---

### Cấu trúc quy trình DevSecOps

Quy trình DevSecOps là một chuỗi các quy trình và công cụ tự động giúp xây dựng, thử nghiệm và phân phối ứng dụng trong môi trường sản xuất cũng như bảo mật chúng ở mọi giai đoạn.
Thông thường các công ty sẽ có 3 môi trường để triển khai dự án chính là Dev, Stagging, Production, một số các công ty lớn hơn thì có nhiều môi trường hơn như UAT, Sandbox.

![DevsecopsDiagram](/images/1-introduce/1.1-implementationprocess/devsecops-diagram.png)

#### Dev (Development Phase)
- **SAST (Static Application Security Testing)**: Đây là bước đầu tiên trong giai đoạn phát triển. SAST phân tích mã nguồn của ứng dụng để phát hiện các lỗ hổng bảo mật tiềm ẩn. Việc kiểm tra này diễn ra mà không cần chạy chương trình, giúp phát hiện sớm các vấn đề bảo mật ngay trong quá trình phát triển.
- **SCA (Software Composition Analysis)**: Sau SAST, SCA được thực hiện để kiểm tra các thành phần phần mềm bên thứ ba (như thư viện và gói mã nguồn mở) được sử dụng trong dự án. SCA giúp phát hiện các lỗ hổng bảo mật đã biết trong các thành phần này.
- **Build**: Sau khi kiểm tra bảo mật, mã nguồn sẽ được xây dựng thành một ứng dụng có thể thực thi. Quá trình này có thể bao gồm việc biên dịch mã, tạo ra các gói ứng dụng, và các bước cần thiết khác để chuẩn bị ứng dụng sẵn sàng để triển khai.
- **Deploy**: Ứng dụng sau khi xây dựng được triển khai lên môi trường kiểm thử hoặc staging để tiếp tục kiểm tra.
- **Unit Test**: Ở giai đoạn này, các bài kiểm tra đơn vị (Unit Test) được thực hiện để đảm bảo từng thành phần của mã nguồn hoạt động đúng theo yêu cầu.

#### Repository (Artifact Management Phase)
- **Artifact Registry**: Sau khi ứng dụng đã được kiểm thử và triển khai, nó sẽ được lưu trữ trong một kho lưu trữ (artifact registry). Kho lưu trữ này sẽ lưu giữ các phiên bản khác nhau của ứng dụng để dễ dàng quản lý và triển khai sau này.
- **Image Scan**: Ảnh Docker hoặc các hình ảnh triển khai khác sẽ được quét để phát hiện các lỗ hổng bảo mật. Đây là bước kiểm tra bảo mật cho các thành phần triển khai (deployment artifacts) trước khi chúng được chuyển sang các giai đoạn tiếp theo.

#### Pre-Prod (Pre-Production Phase)
- **Container**: Ứng dụng sẽ được triển khai dưới dạng container để chuẩn bị cho việc kiểm thử trước khi triển khai lên môi trường sản xuất.
- **DAST (Dynamic Application Security Testing)**: Kiểm thử bảo mật động (DAST) được thực hiện trên ứng dụng đang chạy để phát hiện các lỗ hổng bảo mật có thể chỉ xuất hiện khi ứng dụng hoạt động.
- **Performance Test**: Kiểm thử hiệu suất được thực hiện để đánh giá khả năng chịu tải và hiệu suất của ứng dụng dưới các điều kiện khác nhau. Điều này giúp đảm bảo rằng ứng dụng có thể hoạt động tốt khi triển khai lên môi trường sản xuất.

#### Prod (Production Phase)
- **K8s (Kubernetes)**: Ứng dụng được triển khai lên môi trường sản xuất, thường là trên nền tảng Kubernetes (K8s) để quản lý và vận hành.
- **Pen Test (Penetration Testing)**: Kiểm thử xâm nhập (Pen Test) được thực hiện trên môi trường sản xuất để phát hiện các lỗ hổng bảo mật mà có thể đã bị bỏ sót trong các giai đoạn trước đó.
- **Report**: Cuối cùng, các báo cáo về bảo mật, hiệu suất và kết quả kiểm thử sẽ được tổng hợp lại để đánh giá toàn diện về tình trạng của ứng dụng trước và sau khi triển khai lên sản xuất.