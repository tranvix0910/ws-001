---
title : "Security Scan và Performance Test"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3.3 </b> "
---

#### Code Security Scan Stage

Sau khi thiết lập build và push image chúng ta sẽ tiến hành thêm một Stage để quét bảo mật Source Code.

```yml
snykscan:
    stage: snykscan
    variables: 
        GIT_STRATEGY: clone
    before_script:
        - snyk auth ${SNYK_API_TOKEN}
    script:
        - snyk test --json | snyk-to-html -do ${SNYKSCAN_FILE}.html || true
    tags:
        - wineapp-build-shell
    artifacts:
        paths: 
            - ${SNYKSCAN_FILE}.html
        expire_in: 1 day
```

Trong GitLab CI/CD, `artifacts` là các tệp hoặc thư mục được lưu lại từ một job sau khi job đó hoàn tất thực thi. 

Các artifacts thường được sử dụng để lưu trữ kết quả của quá trình build, kiểm tra, hoặc các thông tin quan trọng khác mà bạn muốn giữ lại và sử dụng trong các job khác trong pipeline.

**paths**: 
- `${SNYKSCAN_FILE}.html`
- Định nghĩa các file artifact mà job sẽ lưu lại sau khi thực thi. Trong trường hợp này, file HTML chứa kết quả quét bảo mật.

**expire_in: 1 day**
- Đặt thời gian tồn tại của artifact là 1 ngày. Sau thời gian này, artifact sẽ bị xóa.

Chúng ta sẽ tiến hành thêm Stage này vào `.gitlab-ci.yml`.

![alt text](/images/3-pipeline/3.3-security-performance/3-3-1.png)

Chú ý tạo các Variables như SNYKSCAN_FILE và SNYK_API_TOKEN.
```
SNYKSCAN_FILE: "SNYK_SECURITY_SCAN_${CI_PROJECT_NAME}_${CI_COMMIT_TAG}_${CI_COMMIT_SHORT_SHA}"
```
![alt text](/images/3-pipeline/3.3-security-performance/3-3-2.png)

Chúng ta sẽ tạo tag và pipeline sẽ được khởi động.

![alt text](/images/3-pipeline/3.3-security-performance/3-3-3.png)

Sau khi chạy pipeline thành công sẽ có thêm 1 stage là Code Security Scan và có thêm một file chứa báo cáo chi tiết về quá trình Test, có thể tải ở đây:

![alt text](/images/3-pipeline/3.3-security-performance/3-3-4.png)

Hoặc chúng ta có thể truy cập vào mục `Artifacts`:

![alt text](/images/3-pipeline/3.3-security-performance/3-3-5.png)

Sau khi tải về tiến hành giải nén file và kiểm tra file report dưới dạng HTML.

![alt text](/images/3-pipeline/3.3-security-performance/3-3-6.png)

#### Scan Image Stage

Việc quét bảo mật của Image chúng ta sẽ thực hiện ngay sau khi chúng ta Build Image thành công.

Chúng ta sẽ thêm một stage để quét bảo mật Image:

```yml
trivy scan image:
    stage: trivy scan image
    variables:
        GIT_STRATEGY: none
    script:
        - docker run --rm -v $(pwd):/wineapp-frontend -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy clean --all
        - docker run --rm -v $(pwd):/${CI_PROJECT_NAME} -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image --format template --template "@contrib/html.tpl" --output /${CI_PROJECT_NAME}/${TRIVY_IMAGE_REPORT}.html ${IMAGE_VERSION}
    tags:
        - wineapp-build-shell
    only:
        - tags
    artifacts:
        paths:
        - ${TRIVYFS_REPORT}.html
        expire_in: 1 day
```

Tạo một Variable để định nghĩa tên file report.
```yml
TRIVY_IMAGE_REPORT: "TRIVYFS_SCAN_IMAGE_REPORT_${CI_PROJECT_NAME}_${CI_COMMIT_TAG}_${CI_COMMIT_SHORT_SHA}"
```

Ở stage này chúng ta có 2 phần , cả hai đều sử dụng công cụ Trivy để quét các lỗ hổng bảo mật trong một Docker Image.

- **Phần 1**: Xóa tất cả dữ liệu tạm thời của Trivy để chuẩn bị cho lần quét mới.

    - `docker run --rm`: Chạy một container Docker và tự động xóa container sau khi hoàn thành công việc.

    - `-v $(pwd):/wineapp-frontend`: Gắn thư mục hiện tại ($(pwd)) vào đường dẫn /wineapp-frontend trong container.

    - `-v /var/run/docker.sock:/var/run/docker.sock`: Gắn Docker socket từ máy chủ vào container, cho phép container thực hiện các thao tác liên quan đến Docker.

    - `aquasec/trivy clean --all`: Sử dụng lệnh clean của Trivy để xóa tất cả dữ liệu tạm thời hoặc dữ liệu cũ mà Trivy đã tạo trong quá trình quét trước đó.


- **Phần 2**: Quét lỗ hổng bảo mật trong một Docker Image, tạo report dưới dạng HTML và lưu nó vào mục Artifacts.

    - `aquasec/trivy image`: Sử dụng lệnh image của Trivy để quét lỗ hổng bảo mật trên Docker Image chúng ta vừa tạo.

    - `--format template`: Chỉ định định dạng đầu ra là template.

    - `--template "@contrib/html.tpl"`: Sử dụng tệp mẫu HTML (html.tpl) để định dạng kết quả quét.

    - `--output /${CI_PROJECT_NAME}/${TRIVY_IMAGE_REPORT}.html`: Chỉ định đường dẫn và tên tệp đầu ra cho báo cáo HTML. Báo cáo này sẽ được lưu trong thư mục của dự án với tên tệp phụ thuộc vào giá trị của biến ${TRIVY_IMAGE_REPORT}.

Thêm Stage vào file cấu hình và tạo tag, chúng ta sẽ kiểm tra Pipeline:

![alt text](/images/3-pipeline/3.3-security-performance/3-3-7.png)

Sau khi chạy Pipeline thành công, kiểm tra mục Aritfacts để tải file Report từ Stage quét bảo mật Image.

![alt text](/images/3-pipeline/3.3-security-performance/3-3-8.png)

![alt text](/images/3-pipeline/3.3-security-performance/3-3-9.png)

Kiểm tra file Report:

![alt text](/images/3-pipeline/3.3-security-performance/3-3-10.png)

#### Deploy Dự Án

Như vậy chúng ta đã thực hiện xong các bước test và kiểm tra trước khi chúng ta Deploy dự án, trong mục này chúng ta sẽ tiến hành Deploy dự án Front-End.

Chúng ta sẽ Deploy dự án ở Server Development.

```yml
deploy:
    stage: deploy
    variables:
        GIT_STRATEGY: none
    before_script:
        - docker login -u ${PORTUS_USER} -p ${PORTUS_PASSWORD} ${PORTUS_URL}
    script:
        - sudo su ${PROJECT_USER} -c "docker pull ${IMAGE_VERSION}; docker rm -f ${CI_PROJECT_NAME}; docker run --name ${CI_PROJECT_NAME} -dp ${FE_PORT} ${IMAGE_VERSION}"
    after_script:
        - docker logout ${PORTUS_URL}
    tags:
        - wineapp-dev-shell
    only:
        - tags
```

Ở phần `tags` chúng ta sẽ để là `wineapp-dev-shell` do đây là tag của Gitlab-Runner được thiết lập ở Server Development.

Chúng ta sẽ tạo biến `FE_PORT` với giá trị là `3000:80` có nghĩa là khi bạn truy cập `http://<ip_server_dev>:3000`, yêu cầu này sẽ được chuyển tới cổng 80 của container, nơi ứng dụng bên trong container sẽ xử lý yêu cầu đó.

Job `deploy` này là một bước triển khai tự động trong pipeline CI/CD của GitLab. 

Nó thực hiện các thao tác liên quan đến Docker để Pull một Docker Image mà chúng ta đã Push lên, dừng container hiện tại, và khởi động lại container với phiên bản mới của ứng dụng. 

Mọi việc được thực hiện dưới quyền của một người dùng cụ thể (PROJECT_USER) để đảm bảo bảo mật và phân quyền.

**Chú ý**:

- Trong phần `script`, chúng ta sử dụng lệnh `sudo su` để chuyển đổi người dùng. Thông thường, khi sử dụng lệnh này, bạn sẽ cần nhập mật khẩu. Tuy nhiên, trong quá trình chạy Pipeline, không có cách nào để nhập mật khẩu theo cách thủ công.

- Do đó, để tránh tình huống này, chúng ta sẽ cấu hình để người dùng GitLab Runner có thể chạy lệnh `sudo su` mà không cần nhập mật khẩu.

- Trên giao diện dòng lệnh, bạn cần nhập lệnh `sudo visudo` và cung cấp mật khẩu. Sau đó, thêm dòng sau vào tệp cấu hình để hoàn tất việc thiết lập:

```
gitlab-runner ALL=(ALL) NOPASSWD: /bin/su
```

![alt text](/images/3-pipeline/3.3-security-performance/3-3-11.png)

Để lưu file chúng ta nhấn `F2 + Y + Enter`.

{{% notice note %}}
Thực hiện thay đổi ở Server Development.
{{% /notice %}}

Sau khi cấu hình, chúng ta sẽ thêm Stage vào Pipeline và tạo Tag.

![alt text](/images/3-pipeline/3.3-security-performance/3-3-12.png)

Như vậy chúng ta đã chạy Pipeline thành công, tiến hành kiểm tra ở địa chỉ sau:
```text
192.168.181.102:3000
```
- `192.168.181.102` là IP của Server Dev nơi chúng ta Deploy ứng dung.

- `3000` là Port dùng để chạy ứng dụng được khai báo khi chúng ta Run Container.

![alt text](/images/3-pipeline/3.3-security-performance/3-3-13.png)

Như vậy Web đã được Deploy thành công, chúng ta có thể kiểm tra Container bằng lệnh:
```
docker container ps
```
![alt text](/images/3-pipeline/3.3-security-performance/3-3-14.png)

#### Security Scan Website - Arachni

Sau bước Deploy chúng ta sẽ tiến hành test bảo mật của trang Web chúng ta vừa triển khai.

Chúng ta sẽ thiết lập Arachni như ở **Mục 2.7**.

- [Web Application Security Scan](https://ws-001.tranvix.online/2-preparation/2.7-arachni/)

```yml
security scan website:
    stage: security scan website
    variables:
        ARACHNI_USER: "arachni"
        PATH_TO_ARACHNI_VERSION: "/home/arachni/arachni-1.6.1.3-0.6.1.1/"
        GIT_STRATEGY: none
    script:
        - sudo su ${ARACHNI_USER} -c "cd ${PATH_TO_ARACHNI_VERSION}; bin/arachni --output-verbose --scope-include-subdomains ${ADDRESS_FRONTEND} --report-save-path=/tmp/${ARACHNI_WEBSITE_REPORT}.afr > /dev/null 2>&1"
        - sudo su ${ARACHNI_USER} -c "cd ${PATH_TO_ARACHNI_VERSION}; bin/arachni_reporter /tmp/${ARACHNI_WEBSITE_REPORT}.afr --reporter=html:outfile=/tmp/${ARACHNI_WEBSITE_REPORT}.html.zip"
        - sudo chmod 777 /tmp/${ARACHNI_WEBSITE_REPORT}.html.zip
        - cp /tmp/${ARACHNI_WEBSITE_REPORT}.html.zip .
    tags:
        - wineapp-dev-shell
    artifacts:
        paths:
            - ${ARACHNI_WEBSITE_REPORT}.html.zip
        expire_in: 1 day
    only:
        - tags
```

Job **security scan website** này thực hiện quét bảo mật trên một website, tạo ra báo cáo bảo mật và lưu báo cáo đó dưới dạng một file .zip để bạn có thể tải về sau khi pipeline hoàn tất. 

`ADDRESS_FRONTEND` là địa chỉ của web chúng ta **192.168.181.102:3000**.

`ARACHNI_WEBSITE_REPORT` là tên của file report.

Khi thực hiện lệnh `sudo chmod` cũng yêu cầu nhập mật khẩu nên chúng ta cũng cần cấu hình như ở trên.
```
gitlab-runner ALL=(ALL) NOPASSWD: /bin/chmod
```
Tiến hành thêm Stage vào Pipeline và tạo các Variable cần thiết, sau đó tạo tag và kiểm tra Pipeline.

![alt text](/images/3-pipeline/3.3-security-performance/3-3-18.png)

Chúng ta sẽ có Report như sau:
![alt text](/images/3-pipeline/3.3-security-performance/3-3-15.png)

#### Performance Test - k6

Để kiểm tra hiệu năng của Website bằng k6 chúng ta sẽ tạo ra một file test được viết bằng JavaScript.
```js
import http from 'k6/http';
import { check, sleep } from 'k6';

export let options = {
    vus: 1,
    duration: '10s',
};

export default function () {
    const BASE_URL = `${__ENV.FE_HOST}`;
    let res = http.get(BASE_URL);
    check(res, {
        'homepage status is 200': (r) => r.status === 200,
    });
    sleep(1);
}
```
Sau đó chúng ta sẽ thêm file test vào repository chứa code.

![alt text](/images/3-pipeline/3.3-security-performance/3-3-16.png)

Thêm Stage vào Pipeline:
```yml
performance testing:
    stage: performance testing
    variables:
        GIT_STRATEGY: none
        SCRIPT_PATH: performance_testing_script
    script:
        - sudo chmod -R 754 ./${SCRIPT_PATH}
        - docker run --user ${ID_USER_GITLAB_RUNNER}:${ID_GROUP_GITLAB_RUNNER} --rm -v $(pwd)/${SCRIPT_PATH}:/${SCRIPT_PATH} grafana/k6 run -e FE_HOST=${FE_HOST} --summary-export=/${SCRIPT_PATH}/summary_perf.json /${SCRIPT_PATH}/smoke_test.js
        - docker run --user ${ID_USER_GITLAB_RUNNER}:${ID_GROUP_GITLAB_RUNNER} --rm -v $(pwd)/${SCRIPT_PATH}:/${SCRIPT_PATH} grafana/k6 run -e FE_HOST=${FE_HOST} /${SCRIPT_PATH}/smoke_test.js
        - mv ./${SCRIPT_PATH}/summary.html $(pwd)/${K6_PERFORMANCE_TEST_REPORT}.html
        - cat ./${SCRIPT_PATH}/summary_perf.json | jq -r '["metric", "avg", "min", "med", "max", "p(90)", "p(95)"], (.metrics | to_entries[] | [.key, .value.avg, .value.min, .value.med, .value.max, .value["p(90)"], .value["p(95)"]]) | @csv' > ${K6_PERFORMANCE_TEST_REPORT}.csv
    after_script:
        - sudo chown -R gitlab-runner $(pwd)
    tags:
        - wineapp-dev-shell
    artifacts:
        paths:
            - ${K6_PERFORMANCE_TEST_REPORT}.html
            - ${K6_PERFORMANCE_TEST_REPORT}.csv
        expire_in: 1 day
    only:
        - tags
```

Job **performance testing** trong GitLab CI/CD được thiết kế để thực hiện kiểm thử hiệu suất cho dự án bằng cách sử dụng công cụ k6.

Docker container dưới quyền user GitLab Runner thông qua các biến `ID_USER_GITLAB_RUNNER` và `ID_GROUP_GITLAB_RUNNER`.

Docker container với k6 được khởi chạy để thực hiện kiểm thử dựa trên kịch bản **smoke_test.js**. Kết quả test được xuất ra dưới dạng tệp JSON **summary_perf.json**. Sau đó kết quả được di chuyển và chuyển đổi thành tệp HTML và CSV và lưu trữ vào **artifacts**.

Sau khi Pipeline chạy thành công chúng ta sẽ có file Report dưới dạng HTML.

![alt text](/images/3-pipeline/3.3-security-performance/3-3-19.png)

![alt text](/images/3-pipeline/3.3-security-performance/3-3-17.png)
```

          /\      |‾‾| /‾‾/   /‾‾/
     /\  /  \     |  |/  /   /  /
    /  \/    \    |     (   /   ‾‾\
   /          \   |  |\  \ |  (‾)  |
  / __________ \  |__| \__\ \_____/ .io

     execution: local
        script: performance_testing_script/smoke_test.js
        output: -

     scenarios: (100.00%) 1 scenario, 1 max VUs, 40s max duration (incl. graceful stop):
              * default: 1 looping VUs for 10s (gracefulStop: 30s)


     ✓ homepage status is 200

     checks.........................: 100.00% ✓ 10       ✗ 0
     data_received..................: 8.1 kB  808 B/s
     data_sent......................: 860 B   86 B/s
     http_req_blocked...............: avg=240.69µs min=3.15µs   med=4.21µs   max=2.36ms   p(90)=241.93µs p(95)=1.3ms
     http_req_connecting............: avg=124.41µs min=0s       med=0s       max=1.24ms   p(90)=124.41µs p(95)=684.3µs
     http_req_duration..............: avg=512.44µs min=318.21µs med=491.68µs max=679.04µs p(90)=652.66µs p(95)=665.85µs
       { expected_response:true }...: avg=512.44µs min=318.21µs med=491.68µs max=679.04µs p(90)=652.66µs p(95)=665.85µs
     http_req_failed................: 0.00%   ✓ 0        ✗ 10
     http_req_receiving.............: avg=90.44µs  min=38.49µs  med=87.98µs  max=129.07µs p(90)=128.62µs p(95)=128.84µs
     http_req_sending...............: avg=47.97µs  min=15.66µs  med=17.02µs  max=305.24µs p(90)=64.64µs  p(95)=184.94µs
     http_req_tls_handshaking.......: avg=0s       min=0s       med=0s       max=0s       p(90)=0s       p(95)=0s
     http_req_waiting...............: avg=374.02µs min=229.34µs med=365.44µs max=529.58µs p(90)=519.81µs p(95)=524.69µs
     http_reqs......................: 10      0.998173/s
     iteration_duration.............: avg=1s       min=1s       med=1s       max=1s       p(90)=1s       p(95)=1s
     iterations.....................: 10      0.998173/s
     vus............................: 1       min=1      max=1
     vus_max........................: 1       min=1      max=1


running (10.0s), 0/1 VUs, 10 complete and 0 interrupted iterations
default ✓ [======================================] 1 VUs  10s
```


















