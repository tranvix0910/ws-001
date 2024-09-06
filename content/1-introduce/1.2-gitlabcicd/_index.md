---
title : "GitLab CI/CD"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 1.2 </b> "
---

### GitLab
**GitLab** is a comprehensive DevOps platform that integrates various tools to manage the entire software development lifecycle. With GitLab, development teams can easily manage source code, automate testing and deployment processes through **Continuous Integration (CI)** and **Continuous Deployment (CD)** features. Additionally, GitLab offers project management tools, progress tracking, and effective collaboration, enhancing both the quality and speed of software development.

GitLab comes in two main versions: **GitLab CE (Community Edition)**, a free, open-source version with basic features, and **GitLab EE (Enterprise Edition)**, a commercial version offering advanced features and professional technical support.

### CI/CD

CI/CD is a method of delivering applications to customers frequently by automating the stages of application development. The core concepts of CI/CD are Continuous Integration, Continuous Deployment, and Continuous Delivery. CI/CD provides a solution for overcoming issues related to integrating and deploying new code during the operation and development of a project.

CI/CD enables automation and continuous monitoring throughout the software development lifecycle, from integration and testing to distribution and deployment.

This process is part of the DevOps and DevSecOps workflow:

![DevsecopsWorkflow](/images/1-introduce/1.2-gitlabcicd/devsecops-workflow.png)

### GitLab CI/CD

**GitLab CI/CD (Continuous Integration and Continuous Deployment/Delivery)** is part of GitLab, an open-source DevOps platform that provides tools to manage the entire software development lifecycle. GitLab CI/CD automates the process of building, testing, and deploying source code, improving software quality and work efficiency.

Key components of GitLab CI/CD:

- **Pipeline**: A set of jobs executed in a defined order. Each pipeline can have multiple stages, and each stage can have multiple jobs.
  
- **Job**: A specific unit of work, such as compiling code, running tests, or deploying applications. Jobs within the same stage are executed in parallel, while stages are executed sequentially.

- **Runner**: Agents responsible for executing jobs. GitLab Runner can be installed on private servers or use runners provided by GitLab.

- **.gitlab-ci.yml**: A configuration file stored in your repository. This file defines the pipelines, stages, and jobs to be executed. It is where you define the CI/CD logic for your project.

In this workshop, we will proceed with self-hosting GitLab to deploy a project securely, ensuring full control of GitLab in an enterprise environment suited to real-world needs.
