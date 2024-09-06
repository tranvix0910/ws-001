---
title : "DevSecOps Tools"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 1.3 </b> "
---

### Table of Content

1. [GitLab Runner](#gitlab-runner)
2. [Docker](#docker)
3. [Snyk](#snyk)
4. [Trivy Scan Image](#trivy-scan-image)
5. [Portus](#portus)
6. [Arachni](#arachni)
7. [k6](#k6)


### GitLab Runner

**GitLab Runner** is an open-source tool, written in Go, developed by GitLab for the CI/CD of projects created on GitLab. Users can install their own runners on their server or use the runners provided by GitLab. You just need to create a `.gitlab-ci.yml` file in the root directory of the project to initialize a CI/CD pipeline and specify which GitLab runner will be used.

#### Types of GitLab Runner

**Shared Runner**

- Shared runners are shared among all projects within a GitLab instance and can be used by any project without special configuration.

- **Advantages**: Resource-efficient and easy to manage, especially useful for small projects or those not requiring specific resources.

- **Deployment**: The GitLab instance administrator configures and registers shared runners.

**Group Runner**

- Group runners are only used by projects within the same GitLab group, allowing control and distribution of resources for related projects.

- **Advantages**: Provides better resource usage control across projects within the same group, and is easy to manage and monitor.

- **Deployment**: The group owner configures and registers group runners via the group settings.

**Specific Runner**

- Specific runners are assigned to a specific project and only serve that project. This ensures dedicated resources for the project.

- **Advantages**: Ensures resources are optimized and more secure for projects requiring specific configurations or high-resource needs.

- **Deployment**: The project owner configures and registers specific runners via the CI/CD settings of the project.

Each type of runner has its own advantages and limitations, depending on the needs and scale of the project.

### Docker

**Docker** is an open-source platform that allows the creation, deployment, and management of applications in a **containerized** environment. With Docker, applications and their dependencies are packaged into containers, ensuring consistency when running across different environments from development to production.

**Docker** offers high portability, optimal performance, and simplifies the DevOps process, enabling development and operations teams to work more efficiently. Containers are lighter than virtual machines, start up faster, and can be shared through Docker Hub, which hosts thousands of pre-built container images.

#### Steps to Create a Container with Docker

1. **Write Dockerfile**: Create a `Dockerfile` that contains the instructions for Docker to know how to build an image for the application, including choosing a base image, installing dependencies, and setting up configurations.

2. **Build Docker Image**: Use the `docker build` command to create a Docker image from the Dockerfile.

3. **Run Container**: After creating the image, use the `docker run` command to create and run a container from that image.

4. **Check and Manage Containers**: Use commands like `docker ps`, `docker logs`, and `docker exec` to check and manage running containers.

Docker simplifies the process of developing, deploying, and operating applications using containerization technology. It ensures consistency across different environments and optimizes system resources.

### Snyk

**Snyk** is a security platform for developers that helps **detect and fix security vulnerabilities in source code**, open-source libraries, containers, and Infrastructure as Code (IaC). Snyk integrates deeply into the software development process, allowing developers to check and repair security issues early during development, before the software is deployed.

#### Key Features of Snyk:

1. **Open Source Security Scanning**: Snyk can scan open-source projects and detect security vulnerabilities in libraries and dependencies used by the project. It provides specific solutions to fix these vulnerabilities.

2. **Container Security Scanning**: Snyk checks Docker images for security vulnerabilities and suggests optimal fixes, reducing risks when deploying containerized applications.

3. **Source Code Security Scanning**: Snyk can scan your source code to identify potential security vulnerabilities, helping to detect issues early during the coding phase.

4. **Infrastructure as Code Security Scanning**: Snyk can check infrastructure configuration files (such as Terraform, AWS CloudFormation) to ensure they don’t contain security misconfigurations.

5. **DevOps Integration**: Snyk easily integrates with CI/CD tools like Jenkins, GitLab CI, CircleCI, and with IDEs and version control systems like GitHub, GitLab, and Bitbucket.

Snyk helps developers and DevOps teams proactively secure their software and systems by identifying and fixing security vulnerabilities early throughout the development and deployment process.

### Trivy Scan Image

**Trivy** is an open-source tool for **scanning security vulnerabilities and configuration issues in containers**, open-source code, and Infrastructure as Code (IaC). Trivy can scan container images to detect security vulnerabilities, protecting applications from potential security risks.

#### Key Features of Trivy:

1. **Docker Image Scanning**: Trivy can scan the layers in Docker images to detect known security vulnerabilities. This ensures that the container images you deploy do not contain serious security issues.

2. **Easy Integration**: Trivy can integrate into the CI/CD process to automatically scan container images before they are deployed, detecting security issues early in the development process.

3. **Supports Multiple Languages and Systems**: Besides containers, Trivy also supports scanning open-source repositories and Infrastructure as Code, including Terraform, CloudFormation, and Kubernetes.

4. **Detailed and Understandable Results**: Trivy provides detailed reports on vulnerabilities, including severity levels (Critical, High, Medium, Low), affected versions, and if available, how to fix the vulnerabilities.

Trivy is a powerful and easy-to-use security tool that helps developers and DevOps teams protect their applications by detecting security vulnerabilities in container images, open-source code, and Infrastructure as Code.

### Portus

**Portus** is a management interface and **security service for self-hosted Docker registries**, helping manage and control Docker images within an organization. Developed by SUSE, Portus offers additional features that Docker Registry lacks, such as access control, monitoring, and user management. This ensures that only authorized individuals or services can access and manage container images.

#### Key Features of Portus:

1. **User and Group Management**: Portus allows user and group management within an organization, enabling different access rights based on user roles.

2. **Access Control**: With Portus, you can set access policies for individual projects or repositories. This protects important images by restricting access.

3. **Docker Registry Integration**: Portus integrates directly with Docker Registry, providing a user-friendly interface to manage Docker images.

4. **Monitoring and Notifications**: Portus provides monitoring capabilities and sends notifications about activities within Docker Registry, making it easy for administrators to track important events, such as image pushes or pulls.

Portus is a Docker Registry management and security solution that offers extended features like access control, user management, and monitoring, helping organizations manage Docker images safely and efficiently.

### Arachni

**Arachni** is an open-source tool used to **scan and detect security vulnerabilities in web applications**. It can detect many common vulnerabilities such as **SQL Injection, XSS, and CSRF**.

Arachni offers both a command-line interface (CLI) and a web interface, making it easy to integrate into security workflows.

This tool also supports extensions via custom plugins or modules and provides detailed reports in various formats such as HTML, JSON, and XML.

### k6

**k6** is an open-source **load and performance testing tool**, developed by Grafana Labs. With k6, users can write testing scripts in JavaScript and perform performance tests capable of generating thousands of requests per second.

**k6** doesn’t require a graphical user interface, making it ideal for automation and integration into CI/CD systems. It also supports integration with performance monitoring services like Grafana and Prometheus to monitor and analyze test results.
