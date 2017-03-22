---
title: Laravel-Angular-Material中文文档——Angular 生成器
tags:
  - Laravel
  - Angular
categories:
  - Laravel-Angular-Material中文文档
date: 2017-01-16 15:29:44
---
## 简介
### Angular生成器
在初始化的项目中包含一系列的Angular生成器来方便你生成前端代码文件模板。  
这个功能是通过使用[ LaravelAngular/generators recipe ](https://github.com/jadjoubran/laravel-ng-artisan-generators)提供的功能实现的。
<!--more-->

> 默认的目录/文件扩展名
> 你可以通过修改`config/generators.php`来自定制自动生成的文件名以及后缀名

## 生成器
> 使用`-`作为分隔符

> 不要使用word作为服务(Service)、组件(Component)或者指令(Directive)的名称

> 自动导入(Auto Import)
> 所有的生成器都会在对应的文件自动更新引入的文件和模块。对于组件，`index.component.js`会自动更新。你也可以在`config/generators.php`中设置取消这一行为，也可以在命令后增加 `--no-import` 标示

下面展示可用的生成器列表
### artisan ng:page {settings}
通过生成以下文件来在`/angular/app/pages`中创建一个新的页面  

* angular/app/pages/settings.page.html
* angular/app/pages/settings.less

### artisan ng:component {user-profile}
通过生成以下文件来在`/angular/components`中创建一个新的组件  

* angular/directives/components/user-profile.component.html  
* angular/directives/components/user-profile.component.js  
* angular/directives/components/user-profile.less  
还包括一个 ngDescribe test 文件
* tests/angular/app/components/user-profile.spec.js  
> 组件 vs 指令  
> 建议阅读Angular官方文档中对components（组件）和directives（指令）不同点的说明  
> 在大多数场景中，你应该使用component来构建页面中的某一块  

### artisan ng:directive {is-admin}
通过生成以下文件来在`/angular/directives`中创建一个新的指令  

* angular/directives/is-admin/is-admin.directive.js  
还包括一个 ngDescribe test 文件
* tests/angular/app/directive/is-admin.spec.js  

### artisan ng:dialog {login}
通过生成以下文件来在`/angular/dialogs`中创建一个新的模态框  

* angular/dialog/login.html  
* angular/dialog/login.dialog.js
* angular/dialog/login.less

### artisan ng:service {cache}
通过生成以下文件来在`/angular/service`中创建一个新的服务  

* angular/service/cache.service.js
还包括一个 ngDescribe test 文件  
* tests/angular/services/cache.spec.js

### artisan ng:filter {ucfirst}
通过生成以下文件来在`/angular/filters`中创建一个新的服务  

* angular/filters/ucfirst.filter.js

### artisan ng:config {http}
通过生成以下文件来在`/angular/config`中创建一个新的配置  

* angular/config/http.config.js
