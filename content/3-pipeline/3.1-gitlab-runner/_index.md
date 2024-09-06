---
title : "Setting up GitLab Runner"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 3.1 </b> "
---

#### Installing GitLab Runner

To ensure that each server can efficiently perform build and deployment tasks, we will install GitLab Runner on both the **Development Server** and **Build Server**.

Install GitLab Runner using the following commands:

```
apt update -y

curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash

apt install gitlab-runner
```


Previously, we discussed the different types of GitLab Runners and how they fit into the project’s needs in [Section 1.3](https://ws-001.tranvix.online/1-introduce/1.3-tools/#gitlab-runner). When deploying, you can choose the strategy that best suits your project's characteristics.

In this workshop, we will focus on deploying a **Group Runner**, a flexible and powerful option that allows all projects within the group to be handled by the same Runner. This helps optimize resources and ensures consistency in the CI/CD process.

![alt text](/images/3-pipeline/3.1-gitlab-runner/3-1-1.png)

GitLab Runner has been successfully installed on both servers.

#### Configuring GitLab Runner

Navigate to **Groups** -> **Build** -> **Runners**.

![alt text](/images/3-pipeline/3.1-gitlab-runner/3-1-2.png)

Select **New group runner**.

![alt text](/images/3-pipeline/3.1-gitlab-runner/3-1-3.png)

Set Tags and Create the Runner, ensuring to tick the **Run untagged jobs** option.

![alt text](/images/3-pipeline/3.1-gitlab-runner/3-1-4.png)

GitLab will then guide us on how to register the GitLab-Runner with the virtual machine:

![alt text](/images/3-pipeline/3.1-gitlab-runner/3-1-5.png)

Execute Step 1:

```
gitlab-runner register  --url http://gitlab.tranvix.vn  --token glrt-DneB3yejRa5zd3F5xLKs
```

![alt text](/images/3-pipeline/3.1-gitlab-runner/3-1-6.png)

After successfully registering on both servers, check in GitLab.

![alt text](/images/3-pipeline/3.1-gitlab-runner/3-1-7.png)

GitLab has now been successfully set up.


