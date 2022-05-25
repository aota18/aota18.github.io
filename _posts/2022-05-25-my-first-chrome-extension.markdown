---
layout: post
title: "My first chrome extension"
date: 2022-05-25 19:26:00 +0900
categories: [Project, Web, Chrome, Extension, i18n]
tags: ["Project", "Web", "Chrome", "Extension", "i18n"]
---

![Alt Text](https://media.giphy.com/media/UwfnPW7ZrNVLjpHi0L/giphy.gif)

# Intro

In this post, I want to introduce my small project. It is Bible Search chrome web extension.
The idea started from my question when I was in church. I was youth leader on my church and I had to create and upload small bulletin on messenger. Everytime when I had to search the bible, I had to visit the bible website and click and drag to find the verses. Like every developers try to improve iterative process more efficiently, I thought chrome extension was really good tool to achieve this goal.

## Bible Search Extension

_Little drops make the might ocean_

I love this quotes because It explains basic principle of our life. Same as on my project, I just focused on the basic search features and the simplicity. These are the features this extension provides:

- Search bible with simple abbreviated keywords
- Copy on clipboard
- Multi languages support (Korean, English)

### Development Process

Basically, I can structurize and explain each parts of this development.

- Hosting & Database
- Backend
- Frontend

#### Hosting & Database

I used AWS EC2(t2.micro) as a backend server and installed MySQL inside of server. Also, there are open bible data you can access with sql. So I downloaded from them and manually imported on my ec2 server.

- [Korean version link](https://sir.kr/g5_tip/4160) (개역개정)
- [English version link](https://github.com/scrollmapper/bible_databases) (multi version supported)

#### Backend

Used Node.JS as backend framework which is lightweight and easy to set up. I just created Restful APIs for fetching parsed data from database.

#### Frontend

In the case of chrome extension, you don't need to deploy the front-end code on your server. All you need to do is just upload your source code including `manifest.json` which is [essential config file](https://developer.chrome.com/docs/extensions/mv3/manifest/) for extension. This is a simple structure of client source code:

```
..
|
|-- index.html
|--....
|-- manifest.json
|-- script.js
|-- ....

```

## What I've learned

It was just a first step to become a great developer. I just learned the whole process from ideation to deployment on market where user can downloads. It was much harder than its size but it is valuable. These are the new things I could experienced:

- Create chrome extension and deployment process
- how to use i18n (internationalization) on project.

## Further features

So many features are waiting for this project to be a good product for user:

### Development

- Applying Unit test (TDD) : it is not easy to test manually as features are added and the product is getting bigger. Implementig unit test on this project is higher priority.

### Features

- Verse search with keywords : this is an idea from my pastor. Most of websites provides this feature and will be updated on next version.
- More versions of bible : For now, only one bible version for each language is supported but will include more versions on next update.
