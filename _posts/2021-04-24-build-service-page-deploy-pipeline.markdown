---
layout: post
title: "Build servce page deploy pipeline"
date: 2021-04-24 12:11:00 +0900
categories: [Web, Deployment, CI/CD, Docker]
tags: [Web, Deployment, CI/CD, Docker]
---

## Intro

I wrote the process while constructing the development pipeline of the ongoing EcoX service page. First of all, the sources developed and handed over are composed of HTML, CSS, and JS, and there were about 140MB of images and video files inside. Before configuring the development environment, we decided to build it using the tools that are the simplest and most essential for maintenance, depending on how the service page will be maintained and utilized in the future.

## Large Image and Video files

I tried to manage all code in git repository for deployment and source management, but the given sources included large amounts of images (.png, .jpg) and image files (.mp4) and I couldn't include them in scm like git. That's why these media files had to be placed separately in storage outside or inside the server, and only the remaining code had to be managed by scm.

![](https://images.velog.io/images/sapphire317/post/81e63c1b-765f-4eeb-b03c-51926aeeeefd/Screen%20Shot%202021-04-24%20at%208.45.38%20PM.png)

In order to solve the above problems, I had to think about the following problems.

- How to build a CI/CD environment?
- Where will the media files (images, images) be located?
- Position externally (S3 Storage)
- Place it inside (directly stored in the server)

## How to build a CI/CD environment?

There was a sample project that we built a distribution pipeline using Docker and Jenkins that we are currently studying, and we decided to use it because we didn't have much time. Basically, the result of building in a library such as React, a front-end library, was the same as the file structure, so we decided to use Dockerfile for containerization, and we could easily build a CI/CD environment because Jenkins' pipeline was the same except for the build process.

![](https://images.velog.io/images/sapphire317/post/ce34ee51-a1bc-4a64-84b2-f1099d2558b7/Screen%20Shot%202021-04-24%20at%208.52.59%20PM.png)

Basically, there are `Local`, `Dev`, and `Prod` enviornments and I put the pipeline which deploys to the server putting `dev` branch and `master`branch in Github.

## Where will the media files (images, images) be located?

In order to build a distribution pipeline such as above, media files needed to be managed separately from normal code. Because of the large capacity, if you make it into a docker image together, the image is very large and the body is heavy in distribution, so you had to avoid it. So I thought of it in two main ways, one of them being S3 storage, which is used in general projects, or putting files directly on an internal server.

**S3 Storage**

The use of S3storage also has a huge advantage in maintenance for later media files, but I thought that the fact that they are not currently located in S3 and that there is no DB that stores code or url for S3 is not enough time to build. In addition, when I put media files in S3, I thought that there was not enough time at the moment because the path to load the files in the code also had to be modified.

**Save in Server**

The way to save it in the server is to send media files directly to the server through ftp and so on, and the codes directly load the files in the server. However, all the codes were containerized and a separate method was needed to access the files directly within the EC2 server. As a result of googling to solve this problem, we found that using Docker's Volume can be easily solved.

[What is Docker Volume ?](https://www.daleseo.com/docker-volumes-bind-mounts/)

Docker Volume is a technology that creates a separate space to position data that can be shared by a container and allow one or more containers to share that space.With reference to this, we placed each of the necessary image files in the corresponding Docker Volume and placed the volume in the path of the existing image file in the serving container.

![](https://images.velog.io/images/sapphire317/post/3830f944-d6a4-4a63-80ab-4bb6179346b4/Screen%20Shot%202021-04-24%20at%209.19.56%20PM.png)

And I proceeded with the contents in the following order.

- Create Docker Volume

- Media file located in that volume

- Run by mounting the volume to the service container

### 1. Create docekr volume

```
# Image for Desktop
$ docker volume create img-d

# Image for Mobile
$ docker volume create img-m
```

### 2. Locate media file to target

- `Route of img-d` volume
  ` /var/lib/docker/volumes/img-d/_data`

- `Route of img-m` volume Path
  `/var/lib/docker/volumes/img-m/_data`

### 3. Run by Volume Mounting the Service Container

- Images for mobile :
  Mount Path : `/usr/share/nginx/html/m/img`

- Images for desktop :
  Mount Path : `/usr/share/nginx/html/img`

And when you run the container, mount the two volumes to the desired location and run.

## Conclusion

Through Jenkins distribution, it was confirmed that the image was loaded well without code modification and there was no problem.
