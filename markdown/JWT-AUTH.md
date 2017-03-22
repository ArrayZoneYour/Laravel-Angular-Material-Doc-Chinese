---
title: Laravel-Angular-Material中文文档——JWT AUTH
tags:
  - Laravel
  - Angular
categories:
  - Laravel-Angular-Material中文文档
date: 2017-01-16 15:40:32
---

## 基本配置
当你使用`composer create-project`来安装时，下面的命令也将执行  
`php artisan jwt:generate`  
它将生成JWT运行所需的`JWT_SECRET`  

JSON Web Token是无状态的，这也是为什么我们从不把它储存在数据库的原因

JWT认证的样本代码在`LoginController`中有提供  
你的API知道，如果用户发送了`Authorization: Bearer {token}`的头，这个用户是经过验证的。  
这已经在API Service中为你自动配置

> 推荐你将`config/jwt`中生成的token移至`.env`中，这个问题将在下一个版本中修复

> 如果你想修改默认的验证模型，确保你更新了`config/jwt.php`中的内容来匹配你做的改动。特别是你想要更新`user` & `identifier`的时候。

<!--more-->
## 认证路由
参考上一章中的JWT Authenticated routes部分

## 多租户
应用中往往需要多个用户角色，对于这种问题你可以创建两个路由组(比方说老师和学生)

* 一个只允许学生（验证用户的学生身份）
* 一个只允许老师（验证用户的教师身份）  
为他们各自创建一个中间件  
当然，也可以创建一个带参数的中间件  

**PHP**

```php
<?php

class TeacherAuthMiddleware
{
    public function handle($request, Closure $next)
    {
        $user = \Auth::user();

        if (!$user || $user->type !== 'teacher') {
            return response()->error('Not Authorized', 401);
        }

        return $next($request);
    }
}
```

**PHP**

```php
<?php

class StudentAuthMiddleware
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        $user = \Auth::user();

        if (!$user || $user->type !== 'student') {
            return response()->error('Not Authorized', 401);
        }

        return $next($request);
    }
}
```

不要忘记将这些中间件添加至你的Http Kerenal中  
接下来，你可以在**routes.php**中使用它们  

**PHP**

```php
<?php

//Public endpoints
$api->group([], function ($api) {
  
});

//Protected endpoints (Mentor or Mentee)
$api->group(['middleware' => 'api.auth'], function ($api) {
  
});

//Protected: Teacher only
$api->group(['middleware' => ['api.auth', 'auth.teacher']], function ($api) {
  
});

//Protected: Student only
$api->group(['middleware' => ['api.auth', 'auth.student']], function ($api) {
  
});
```

## 测试
参考下一章节中的JWT Auth Test部分

## 注册 & 登录
### Satelizer
Laravel & Angular material starter引入了[satellizer](https://github.com/sahat/satellizer)来管理token  
你可以在`angular/config/satellizer.config.js`中自定制satellizer的配置

### 注册功能
![注册图片](https://files.readme.io/NsKdOEzjRmKncNS6JN9g_Screen%20Shot%202016-04-21%20at%2012.48.07%20AM.png)
注册功能是预置的功能  
`/register`路由的配置文件为`routes.config.js`  
你有一个包括`register-form`组件的页面文件`register.page.html`  
`register-form`组件通过使用satellizer和`Auth\AuthController.php`文件来实现用户注册功能  
如果你想添加新的字段，那么你应该同时编辑view，component和AuthController  
### 登录功能
![登录图片](https://files.readme.io/l5vRJ9UDRvyrZ4oxwf4z_Screen%20Shot%202016-04-21%20at%2012.48.21%20AM.png)
登录功能也是预置功能。  
登录的路由`/login`是在`route.config.js`中配置的。  
在`login.page.html`中包含着`login-form`组件。  
`login-form`组件通过使用satellizer和`Auth\AuthController.php`实现用户的认证功能  
我们可以在`Auth\AuthController.php`中检验登录功能的逻辑
## 重置密码
### 忘记密码
![忘记密码](https://files.readme.io/JeTq2IfRZe7hrEEGyPxD_Screen%20Shot%202016-04-21%20at%202.28.19%20AM.png)
忘记密码功能是预置的。  
忘记密码的路由`/forget-password`的配置文件被配置在`routes.config.js`中  
`forget_password.page.html`中包含着`forget-password`组件  

这个组件会发送一个带有重置密码链接的电子邮件。  
你可以在`Auth\PasswordResetController.php`文件中检查它的逻辑  

> 重置链接  
> 默认的链接前缀是`localhost:8000`，你需要在`.env`增加对应的变量并合理地管理它。  

> Email的发送地址  
> 在使用这一新特性之前，你需要替换`Auth\PasswordResetController.php`为你自己使用的邮箱名

### 重置密码
![](https://files.readme.io/qI8pUzE1SFCTsQu5op4S_Screen%20Shot%202016-04-21%20at%202.29.32%20AM.png)
点击邮件中的重置密码链接会转到`/reset-password/:email/:token`的路由  
当加载图案旋转时，`reset-password`组件将会立刻检测token的有效性  
一旦数据库验证通过，重置密码的表单就会出现，提交表单就可以成功重置密码了。  
## 验证路由（前端）
如果你打开`angular/config/routes.config.js`文件，你会注意到我们有一个空的参数`data: {}`，每个state都继承了这一属性因为它在抽象路由`app`被定义。  
尽管这是一个可选的参数，我们可以在`angular/run/routes.run.js`中自定义我们自己所需的逻辑，然后通过检查`data`对象中的值并据此完成认证逻辑。  
如果你设置成下面的样子  

```javascript
data: {
	auth: true
}
```

这些路由就会被保护起来（他们需要认证权限）  
这一行为可以在单独的路由中启用，也可以通过创建抽象的路由来定义一个路由群组来启用。  
我们通过设置一个字符串值来继承这一行为，比如`auth: 'admin'`，然后你可以在`routes.run.js`中定义你自己的逻辑
