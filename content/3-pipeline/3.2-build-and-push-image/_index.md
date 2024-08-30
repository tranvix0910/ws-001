---
title : "Build và Push Docker Image"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 3.2 </b> "
---

#### Thiết lập các cấu hình cần thiết.

Trước khi tạo Dockerfile, bạn cần cập nhật giá trị BASE_URL trong frontend để trỏ đến địa chỉ server backend mà bạn sẽ deploy. Cụ thể, bạn cần thay đổi thông số này trong file cấu hình của frontend.

Để thực hiện điều này trên GitLab, hãy làm theo các bước sau:

Truy cập file cấu hình:

- Trong dự án frontend của bạn trên GitLab, điều hướng đến file cấu hình `src/config/utils.js`.

- Thay đổi giá trị `BASE_URL` để trỏ tới server deploy backend hiện tại. 

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-1.png)

Tiếp theo chúng ta sẽ thực hiện tạo một nhánh ( branch ) mới.

Để đảm bảo tính ổn định của nhánh chính (Main), hãy tạo một nhánh mới để thực hiện các thay đổi. Điều này giúp giữ cho nhánh Main không bị ảnh hưởng trong quá trình phát triển.

Truy cập vào dấu `+` và chọn `New Branch`.
![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-2.png)

Chúng ta sẽ đặt tên và tạo Branch.

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-3.png)

Chúng ta sẽ thực hiện thêm một số cấu hình sau để đảm bảo tính bảo mật.

Truy cập vào `Settings` -> `Repository`

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-4.png)

Thay đổi nhánh mặc định thành nhánh vừa tạo, việc này nhầm mục địch khi chúng ta truy cập vào Project, Source code của nhánh Develop sẽ xuất hiện đầu tiên.

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-5.png)

Tiếp theo chúng ta sẽ thực hiện Protect Branch, khi bảo vệ nhánh có thể giữ an toàn cho nhánh tránh các tình huống Push và Merge.

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-6.png)

#### Thêm User Gitlab-Runner vào Group Docker

Trong quá trình chạy Pipeline, User gitlab-runner sẽ được dùng để chạy dự án. 

Việc thêm gitlab-runner vào Group Docker giúp cho User có thể thực hiện các câu lệnh Docker như : Login, Logout Build và Push Image.

```
sudo usermod -aG docker gitlab-runner
```

- `sudo`: Chạy lệnh với quyền quản trị.

- `usermod`: Công cụ để sửa đổi tài khoản người dùng.

- `-aG docker`: Tùy chọn `-aG` thêm người dùng vào nhóm chỉ định mà không loại họ khỏi các nhóm khác.

- `gitlab-runner`: Tên của người dùng cần thêm vào nhóm docker.

#### Tạo Dockerfile

Truy cập vào Web IDE để tiện cho việc thao tác.

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-7.png)
![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-8.png)

Tạo Dockerfile cho Frontend:
```Dockerfile
##### Dockerfile #####
## build stage ##
FROM node:22-alpine AS build

WORKDIR /app
COPY . .
RUN npm install
RUN npm run build

## run stage ##
FROM nginx:alpine

COPY --from=build /app/build /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
#### Tạo Gitlab CI/CD

Tiến hành tạo file `.gitlab-ci.yml`.

{{% notice note %}}
Tệp `.gitlab-ci.yml` là một tệp cấu hình được sử dụng trong GitLab CI/CD. Nó định nghĩa các pipeline, các job, và các stage để tự động hóa các tác vụ như xây dựng, kiểm thử và triển khai mã nguồn khi có sự thay đổi trong kho lưu trữ (repository).
{{% /notice %}}

```yml
variables:
    PROJECT_USER: "wineapp"
    IMAGE_VERSION: "${PORTUS_URL}/${PROJECT_USER}/${CI_PROJECT_NAME}:${CI_COMMIT_TAG}_${CI_COMMIT_SHORT_SHA}"
    
stages:
    - build
    - push

build:
    stage: build
    variables: 
        GIT_STRATEGY: clone
    before_script:
        - docker login -u ${PORTUS_USER} -p ${PORTUS_PASSWORD} ${PORTUS_URL}
    script:
        - docker build -t ${IMAGE_VERSION} .
    after_script:
        - docker logout ${PORTUS_URL}
    tags:
        - wineapp-build-shell
    only:
        - tags

push:
    stage: push
    variables: 
        GIT_STRATEGY: none
    before_script:
        - docker login -u ${PORTUS_USER} -p ${PORTUS_PASSWORD} ${PORTUS_URL}
    script:
        - docker push ${IMAGE_VERSION}
    after_script:
        - docker logout ${PORTUS_URL}
    tags:
        - wineapp-build-shell
    only:
        - tags
```

Tệp cấu hình GitLab CI/CD được viết bằng YAML.

Trước khi giải thích chi tiết về file cấu hình trên, chúng ta sẽ tìm hiểu về **Biến (Variables)** trong Gitlab CI/CD.

Variables có 2 loại chính:

**Biến mặc định (Predefined Variables)**:
- Được GitLab cung cấp tự động trong mỗi pipeline.
- Ví dụ:
    - **CI_COMMIT_SHA**: SHA của commit hiện tại.
    - **CI_JOB_ID**: ID của công việc (job) hiện tại.
    - **CI_PROJECT_NAME**: Tên của dự án.
    - **CI_COMMIT_REF_NAME**: Tên nhánh (branch) hoặc tag của commit hiện tại.
    - **CI_PIPELINE_ID**: ID của pipeline.

**Biến do người dùng định nghĩa (User-defined Variables)**:

- Được người dùng tự định nghĩa trong tệp `.gitlab-ci.yml` hoặc trong phần cài đặt của dự án trên GitLab. 
- Định nghĩa trong `.gitlab-ci.yml`:
```yaml
variables:
    PROJECT_USER: "wineapp"
    PORTUS_URL: "https://registry.example.com"
```
- Định nghĩa trong cài đặt dự án `Project Settings > CI/CD > Variables`:

    - Có thể được thiết lập ở mức dự án, nhóm, hoặc pipeline cụ thể.

    - Hỗ trợ tính năng protected, masked, và scoped (cho phép giới hạn biến chỉ trong một số môi trường hoặc nhánh cụ thể).

Trong dự án, Đầu tiên sẽ khai báo các biến (Variables):

- **PROJECT_USER**: Được dùng để định nghĩa User để triển khai dự án.

- **IMAGE_VERSION**: Tạo tag cho Docker image sử dụng một số biến:

    - `${PORTUS_URL}`: URL của Docker registry (có thể là Portus).

    - `${PROJECT_USER}`: Người dùng dưới đó image sẽ được lưu trữ.

    - `${CI_PROJECT_NAME}`: Tên dự án.

    - `${CI_COMMIT_TAG}`: Tag gắn với commit (có thể là số phiên bản).

    - `${CI_COMMIT_SHORT_SHA}`: Phiên bản rút gọn của SHA commit.

{{% notice note %}}
Chúng ta có thể lấy giá trị của biến bằng cách `${<Variable>}`
{{% /notice %}}

Tiếp theo chúng ta sẽ tạo các **Stage** cho Pipeline:

Chúng ta sẽ định nghĩa pipeline chúng ta có 2 giai đoạn chính là **build** và **push**.

- **build**: Giai đoạn mà Docker image được xây dựng.

- **push**: Giai đoạn mà Docker image đã xây dựng được đẩy lên Docker registry.

**Stage Build** chúng ta sẽ thực hiện những bước sau:

- `GIT_STRATEGY: clone` : Chỉ định rằng kho mã nguồn sẽ được clone (hành vi mặc định).

- `before_script` : Các lệnh được thực hiện trước khi chạy script chính. Đăng nhập vào Docker registry sử dụng thông tin đăng nhập được lưu trong các biến môi trường `${PORTUS_USER}` và `${PORTUS_PASSWORD}`.

- `script` : Phần chính của giai đoạn này, xây dựng Docker image với tag `IMAGE_VERSION` đã được chỉ định.

- `after_script` : Đăng xuất khỏi Docker registry sau khi quá trình build hoàn thành.

- `tags` : Chỉ định runner (wineapp-build-shell) sẽ được sử dụng cho công việc này.

- `only` : Đảm bảo công việc này chỉ chạy khi một **Git Tag** được đẩy lên.

{{% notice note %}}
Thông thường khi code chúng ta có thay đổi Gitlab CI/CD sẽ tiến hành khởi động Pipeline, Tuy nhiên khi chúng ta khai báo `only: -tags`, việc chạy Pipeline sẽ được tiến hành khi chúng ta tạo Tag.
{{% /notice %}}

**Stage Push** tương tự như Stage Build tuy nhiên chúng ta sẽ khai báo khác một tí:

- `GIT_STRATEGY : none` : chúng ta sẽ không cần clone mã nguồn về ở giai đoạn này, vì mã nguồn đã được clone ở Stage trước.

- `script`: Đẩy Docker image đã được xây dựng lên Docker registry.

#### Tạo Variables

Sau khi tạo file thành công chúng ta sẽ Commit và tạo các Variables.

Truy cập `Project Settings > CI/CD > Variables`:

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-9.png)

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-10.png)

Tiến hành tạo một số biến chúng ta đã định nghĩa tại Pipeline ( trừ một số biến mặc định của Gitlab CI/CD ).

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-11.png)

**Protect variable** : Các biến chỉ chạy trên các Branch và Tag được Protect.

**Expand variable reference**: Bất kỳ ký tự $ nào trong giá trị của biến sẽ được xử lý như một tham chiếu đến một biến khác.

- Ví dụ :
    - `PROJECT_NAME` có giá trị là `myapp`.
    
    - `PORTUS_URL` có giá trị là `https://registry.example.com/$PROJECT_NAME`.

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-12.png)

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-13.png)

Như vậy đã thêm các biến thành công.

#### Khởi tạo Pipeline

Chúng ta sẽ tiến hành tạo Tag:

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-14.png)

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-15.png)

Đặt tên Tag và chú ý chọn đúng Branch.

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-16.png)

Sau khi tạo Tag, Pipeline sẽ được khởi động.

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-17.png)

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-18.png)

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-19.png)

**Job Build** Thành công.

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-20.png)

**Job Push** Thành công.

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-21.png)

Khi truy cập vào Portus, Repository đã được Push thành công.

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-22.png)

![alt text](/images/3-pipeline/3.2-build-and-push-image/3-2-23.png)
