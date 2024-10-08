---
title : "Gửi Report vào Telegram"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 3.4 </b> "
---

#### Chuẩn Bị

Để có thể gửi Report vào Telegram chúng ta cần tạo một tài khoản Telegram.

Sau khi tạo thành công chúng ta sẽ tìm kiếm `BotFather` để tiến hành tạo Bot.

![alt text](/images/3-pipeline/3.4-send-report/3-4-1.png)

![alt text](/images/3-pipeline/3.4-send-report/3-4-2.png)

Sau khi Start chúng ta sẽ tiến hành tạo Bot mới bằng cách nhập `/newbot`.

![alt text](/images/3-pipeline/3.4-send-report/3-4-3.png)

Khi thực hiện xong các yêu cầu chúng ta sẽ được cung cấp 1 con Bot có thể truy cập thông qua đường link và một Token.

![alt text](/images/3-pipeline/3.4-send-report/3-4-4.png)

Để Bot có thể thông báo, chúng ta sẽ tiến hành tạo Group và thêm Bot vào.

![alt text](/images/3-pipeline/3.4-send-report/3-4-5.png)

![alt text](/images/3-pipeline/3.4-send-report/3-4-6.png)

Chúng ta cần biết ID của Group để có thể gửi tin nhắn vào, nên chúng ta cần thêm Bot `RawDataBot` để cung cập ID.

![alt text](/images/3-pipeline/3.4-send-report/3-4-7.png)

Chúng ta có thể test gửi một Message thông qua Server bằng lệnh sau:
```
curl -X POST "https://api.telegram.org/bot<Bot_Token>/sendMessage" -d "chat_id=<chat_id>" -d "text=test"
```

```
curl -X POST "https://api.telegram.org/bot7538065344:AAEpk4atmTnmbl7knIbdQohOxrAfiG5tKi4/sendMessage" -d "chat_id=-4515541652" -d "text=test"
```

![alt text](/images/3-pipeline/3.4-send-report/3-4-8.png)

#### Send Report Stage

Chúng ta sẽ thêm một Stage để có thể gửi Report ngay sau khi cúng ta Test hoàn tất.

Stage này chúng ta sẽ có 2 Job.

Job `send report from build server` nó sẽ thực hiện gửi 2 tệp báo cáo **Code Security Scan** và **Image Scan** được thực hiện trước khi chúng ta thực hiện triển khai dự án.

Job `send report from dev server` tương tự như job trên, nó cũng thực hiện gửi các file Report sau khi chúng ta triển khai dự án.

```yml
send report from build server:
    stage: send report
    variables:
        GIT_STRATEGY: none
    script:
        - curl -F "chat_id=${TELE_GROUP_CHAT_ID}" -F 'media=[{"type":"document","media":"attach://file1"}, {"type":"document","media":"attach://file2"}]' -F "file1=@$(pwd)/${SNYK_SECURITY_SCAN_REPORT}.html" -F "file2=@$(pwd)/${TRIVYFS_SCAN_IMAGE_REPORT}.html" "https://api.telegram.org/bot${API_BOT}/sendMediaGroup"
    tags:
        - wineapp-build-shell
    only:
        - tags
```

```yml
send report from dev server:
    stage: send report
    variables:
        GIT_STRATEGY: none
    script:
        - curl -F "chat_id=${TELE_GROUP_CHAT_ID}" -F 'media=[{"type":"document","media":"attach://file1"}, {"type":"document","media":"attach://file2"}, {"type":"document","media":"attach://file3"}]' -F "file1=@$(pwd)/${ARACHNI_WEBSITE_REPORT}.html.zip" -F "file2=@$(pwd)/${K6_PERFORMANCE_TEST_REPORT}.html" -F "file3=@$(pwd)/${K6_PERFORMANCE_TEST_REPORT}.csv" "https://api.telegram.org/bot${API_BOT}/sendMediaGroup"
    tags:
        - wineapp-dev-shell
    only:
        - tags
```

Sau khi chạy pipeline Bot sẽ gửi cho chúng ta những file report ở các Stage trước.

![alt text](/images/3-pipeline/3.4-send-report/3-4-9.png)



