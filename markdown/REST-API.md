---
title: Laravel-Angular-Material中文文档——REST API
tags:
  - Laravel
  - Angular
categories:
  - Laravel-Angular-Material中文文档
date: 2017-01-16 15:37:29
---

## 概述
在现代的web app中REST API是一个被频繁使用的功能。  
这也是为什么我们专注于使这个过程保持一致。  
这可以使你在你的API中更加方便地开发新的接口。  
打比方来说，在RestAngular中验证错误会自动展示。  
Laravel Angular Material Starter可以帮助你更好地规范API返回的数据格式  
错误响应对应着一个具体的格式，成功的响应对应另一个。  
前端也会受益于这种一致性，通过配置响应拦截器，在接受到错误的返回后它可以自动弹出一个验证错误的弹出层  
所有这一切都提供了可选的[Json Web Token Authentication](https://laravel-angular.readme.io/v3.4/docs/jwt-authenticated-routes)支持和有用的[API test helpers](https://laravel-angular.readme.io/v3.4/docs/api-test-helpers)，这将使集成测试更容易。
<!--more-->
## 响应宏（Response Macros）
[Response Macros](http://laravel.com/docs/5.1/responses#response-macros)本来是Laravel框架中的一个特性  
在上面我们提到了REST APIs需要保持一致性，所以我们提供了两个默认的响应宏来从你的接口返回成功、失败时的数据  

**PHP**

```php
<?php

class PostsController
{
    public function get()
    {
        $posts = App\Post::all();  
      
        return response()->success('posts', $posts);
    }
  
}
```

**PHP**

```php
<?php

class SettingsController
{
    public function update()
    {
        if ( !\Auth::user()->is_verified ){
          return response()->error('Not Authorized', 401);
        }
    }
  
}
```

注意即使验证错误通过了`$this->validate($request, [])`之后返回了与`response()->error()`相同的错误格式，但是会携带422的错误状态码（不可处理实体）  
这意味着你期望所有的正确数据返回同一种格式的数据，错误数据也是一样（包括验证）  
得益于这种一致性，你可以在RestAngular中配置选择在Toast中展示验证错误信息。  
当然，在这样的前提下，API test helper也可以很容易地断言被用来返回的正确响应。  
例子：`->seeApiSuccess()`，`->seeValidationError()`，`->seeApiError(401)`。

> 验证响应  
> 如果你计划修改_error_的响应宏，你需要在基础控制器文件`App\Http\Controllers\controller.php`中进行应用和修复。


## RestAngular
> RestAngular 是一个用来简化常用的GET、POST、DELETE、UPDATE请求的AngularJS服务，这样你可以使用最少的客户端代码来实现它，对于通过REST API来处理数据的web app来说，这是最适合的。

这使得它与这个库可以完美地配合起来。
> RestAngular 是`$http`上的一层

## 默认配置
这个库提供给你一个叫做API的服务(文件路径：`angular/services/api.service.js`)，它配置了如下内容：

* Content negotiation (header)
* Content type (header)  
* Base URL: 这意味着你在调用API端口时不需要写`/api/`作为前缀  
* error interceptor（异常拦截器），它会将第一条错误信息展示在红色的弹出层中
* JWT 支持

所有这些配置都与我们期望的API（也内联到dingo / api的配置）是一致的。  
你可以浏览他们提供的可选的其他选项——[their repository](https://github.com/mgonto/restangular)

> 避免使用Restangular service,通过API来调用  
> 推荐你避免使用Restangular服务  
> 使用继承了Restangular的服务（如API）可以让你为其他API提供其他的服务或配置，可以更加灵活。

> 虽然RestAngular对于初学者来说并不是那么容易掌握，但是这是非常值得的，一旦你掌握了它，你可以用非常流畅的语法来调用你的接口。

## JWT Authenticated routes
在`config/api.php`中，JWT被配置作为`dingo/api`的一个验证提供者。  
它允许你使用`api.auth`中间件为需要验证的路由提供验证服务  
如果用户没有登录，或者验证头丢失，他们将无法访问接口。  
如果你想增加更复杂的内容，你可以创建自己的中间件。
