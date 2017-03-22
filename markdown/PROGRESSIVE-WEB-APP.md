---
title: Laravel-Angular-Material中文文档——PROGRESSIVE WEB APP
tags:
  - Laravel
  - Angular
categories:
  - Laravel-Angular-Material中文文档
date: 2017-01-16 15:35:21
---

# Progressive WEB APP
## 概述
受益于最新的技术，Progressive WEB APP可以带给用户最好的移动站点和native app。并且是可靠、迅速的，因此吸引了许多开发者。——[Google Web Fundamentals](https://developers.google.com/web/progressive-web-apps/)

<!--more-->
## DEMO
你可以从浏览器（Chrome、Firefox、或Opera）打开[flipkart.com](https://flipkart.com/)并将它添加到桌面。
progressive web app利用了浏览器最新的功能（因此叫Progressive，先进），将它添加到桌面之后，它可以从桌面启动，离线使用，甚至可以推送消息。

## Web App Manifest
web app manifest将以一个文档的形式展现给而用户关于它的信息（像名字、作者、图标、简介等等）  
它的初级目标是创建一个先进的web app：web app无需用户通过App store就可以被安装至系统桌面（此外还可以离线使用，app内容更改、升级时还可以推送消息）——[Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/Manifest)  

## php artisan pwa:manifest
这个生成器将帮助你创建你自己的`manfiest.json`  
你可以跳过它的问题来自己直接修改内容

## App Shell
App Shell 是用来搭建用户界面和确保app高性能以及可靠性的组件之一的最小化的HTML、CSS和JavaScript，它在第一次使用的时候就会迅速加载，并且会被缓存起来。这意味着它不需要每次都被重新加载一次，app只需要加载必要的内容即可。[Google Web Fundamentals](https://developers.google.com/web/fundamentals/getting-started/your-first-progressive-web-app/step-01?hl=en)

## App Shell in index.blade.php

app shell 已经在`resource/views/index.blade.php`中为你配置完毕  
你可以在index.blade.php中看看app shell以是怎样的形式在页面中出现的。

## critial.scss
Critial CSS将为你的App Shell服务，这是app shell用来展示所需的最基本的CSS文件。它是通过sass预处理的，所以你可以使用变量。它们被排除在`final.css`之外所以不会重复（这里你只需要关心App Shell本身即可）

## sw precache
一个用来生成可以缓存指定资源的服务对应代码的nodejs模块——[GoogleChrome/sw-precache](https://github.com/GoogleChrome/sw-precache)

## sw precache & gulp
sw-precache已经在你的gulpfile.js中配置完毕，它会从`precache-config.json`读取配置，当你改动其中的配置后无需重新执行`gulp watch`命令  
默认情况下，我们会缓存app shell的文件结构，字体样式，以及你最新的资源文件（CSS & JavaScript）

## 运行缓存
运行缓存允许你自己进行配置[sw-toolbox]

## sw toolbox
一个用来处理运行时请求的服务工具集合——[GoogleChrome/sw-toolbox](https://github.com/GoogleChrome/sw-toolbox)

## precache-config.json
你可以在`precache-config.json`中的runruntimeCaching对象中自定制你的缓存策略。默认情况下我们为Github的button和网页字体指定了`cacheFirst`的策略。
