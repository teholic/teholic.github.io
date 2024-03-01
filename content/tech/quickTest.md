---
author: 'xhddxiin'
title: Guideline to build a simple CT
date: 2024-02-28
draft: false
description: "this is a simple demo"
thumbnail:
  url: /img/enjoy.jpg
  author: Shaurya Sagar
  authorURL: https://unsplash.com/@shauryasagar
  origin: Unsplash
  originURL: https://unsplash.com/photos/A4wa3SpyOsg
---
## Basic usage
what is continuous testing? \
Continuous testing aims to provide rapid and frequent feedback on the quality and reliability of the software being 
developed. It involves automatically executing a comprehensive set of tests, as part of the development and deployment 
pipeline.

## Prerequisites
here is the preparation that need to know and will use in the follow steps\
- docker
- github
- jenkins
- newman
- node
- allure
## Installation
#### 1. install docker
install jenkins and create a volume
{{< command >}}
docker pull jenkins/jenkins
{{< /command >}}
{{< command >}}
docker volume create jenkins-data
{{< /command >}}
#### 2. run jenkins in daemon state
{{< command >}}
docker run \
-u root \
--rm \
-d \
-p 8080:8080 \
-p 50000:50000 \
-v jenkins-data:/var/jenkins_home \
-v /var/run/docker.sock:/var/run/docker.sock \
jenkins/jenkins
{{< /command >}}
#### 3. install jenkins plugins
- NodeJS
- Allure
#### 4. config the plugin's locations
go to jenkins dashboard --> manage jenkins --> tools

{{< image src="img/jenkins-allure-config.png" class="rounded" >}}
{{< image src="img/jenkins-node-config.png" class="rounded" >}}

#### 5.create an item and config
in the jenkins dashboard, click to create a new item -->freestyle project -->config
- Source Code Management
  - choose git and paste you git url
  - "Branches to build", if have no customize requirement, leave it */main or */master(depends on gitlab or github)
- select "Provide Node & npm bin/ folder to PATH"
- build step
  - the json file can be exported from postman and uplaod to git
    {{< command >}}
    node --version
    npm -v
    npm install -g newman
    newman --version
    newman run demo.postman_collection.json -g demo.postman_environment.json -r allure
    {{< /command >}}
- post build actions 
  - choose Allure report
- Build Triggers
  - can choose Build periodically,so your project can be build and running at a specific time
## Run you project
click build now to run your project and get the report
{{< image src="img/jenkins-run.png" class="rounded" >}}
{{< image src="img/jenkins-allure-report.png" class="rounded" >}}

## More About
jenkins provides various plugins and support to different stack, it also support java and golang project continues 
build. it is very useful\
in a big company, there always a platform to control this flow and make it view more acceptable to technical and
non-technicals