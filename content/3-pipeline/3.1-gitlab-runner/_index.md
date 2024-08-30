---
title : "Thiết lập Gitlab Runner"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 3.1 </b> "
---

#### Cài đặt Gitlab Runner

Để đảm bảo mỗi server có thể thực hiện nhiệm vụ build và triển khai một cách hiệu quả, chúng ta sẽ tiến hành cài đặt GitLab Runner trên cả Server **Development** và Server **Build**.

Cài đặt Gitlab Runner theo các lệnh sau:
```
apt update -y

curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash

apt install gitlab-runner
```

Trước đây, chúng ta đã thảo luận về các loại GitLab Runner khác nhau và cách chúng phù hợp với các nhu cầu dự án ( mục 1.3 ). Khi triển khai, bạn có thể chọn chiến lược phù hợp nhất với đặc thù dự án của bạn.

Trong buổi Workshop này, chúng ta sẽ tập trung vào việc triển khai **Group Runner**, một lựa chọn linh hoạt và mạnh mẽ cho phép tất cả các dự án trong nhóm được xử lý bởi cùng một Runner. Điều này giúp tối ưu hóa tài nguyên và đảm bảo tính nhất quán trong quá trình CI/CD.

![alt text](/images/3-pipeline/3.1-gitlab-runner/3-1-1.png)

Gitlab Runner đã cài đặt thành công ở 2 Server.

#### Cấu hình Gitlab Runner

Truy cập `Groups` -> `Build` -> `Runners`.

![alt text](/images/3-pipeline/3.1-gitlab-runner/3-1-2.png)

Chọn `New group runner`.

![alt text](/images/3-pipeline/3.1-gitlab-runner/3-1-3.png)

Đặt Tags và Tạo Runner, chú ý tick chọn `Run untagged jobs`.

![alt text](/images/3-pipeline/3.1-gitlab-runner/3-1-4.png)

Sau đó, Gitlab sẽ hướng dấn chúng ta cách để đăng ký Gitlab-Runner với máy ảo:

![alt text](/images/3-pipeline/3.1-gitlab-runner/3-1-5.png)

Thực hiện Step 1:

```
gitlab-runner register  --url http://gitlab.tranvix.vn  --token glrt-DneB3yejRa5zd3F5xLKs
```

![alt text](/images/3-pipeline/3.1-gitlab-runner/3-1-6.png)

Sau khi thực hiện thành công ở cả 2 Server tiến hành kiểm tra tại Gitlab.

![alt text](/images/3-pipeline/3.1-gitlab-runner/3-1-7.png)

Như vậy đã thiết lập Gitlab thành công.

