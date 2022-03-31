# test2222

这里大家写自己的开头语
>这一行是引用，灰色，按需要加。下面8行*是目录.你也可以通过我的目录来改，保证格式不变，只修改文字

* [Java相关书籍](#java相关书籍)
* [前端相关书籍](#前端相关书籍)
* [c/c\+\+相关书籍](#cc相关书籍)
* [python相关书籍](#python相关书籍)
* [算法相关书籍](#算法相关书籍)
* [数据结构相关书籍](#数据结构相关书籍)
* [数据库](#数据库)
* [免责声明](#免责声明)

## 这里用的是第二大标题，提醒#后面有一个空格
- 书名，-后面也有空格[显示链接名称](链接) 密码：中括号和小括号都是英文输入法
- 程序员在囧途[百度网盘链接](https://pan.baidu.com/s/18xEuDHb9tuLSeC5EQ0ZyPQ) 密码：u7wg


## 前端相关书籍

## c/c++相关书籍

## python相关书籍

## 算法相关书籍

## 数据结构相关书籍

## 数据库

## 免责声明

# spring-security-jwt-guide

**如果国内访问缓慢的话，可以通过码云查看：** https://gitee.com/SnailClimb/spring-security-jwt-guide 。

## 前言

[Spring Security](https://spring.io/projects/spring-security) 是 Spring 全家桶中非常强大的一个用来做身份验证以及权限控制的框架，我们可以轻松地扩展它来满足我们当前系统安全性这方面的需求。

但是 Spring Security 相比于其他一些技术比如 JPA 来说更难上手，很多人初学的时候很难通过看视频或者文档发就很快能独立写一个 Demo 出来，于是后面可能就放弃了学习这个东西。

刚来公司的时候的入职培训实战项目以及现在正在做的项目都用到了 Spring Security 这个强大的安全验证框架，可以看出这个框架在身份验证以及权限验证领域可以说应该是比较不错的选择。由于之前经历项目的这部分模块都不是自己做的，所以对于 Spring Security 并不是太熟悉。于是自/己抽时间对这部分知识学习了一下，并实现了一个简单的 Demo 。这个 Demo 主要用到了 **Spring Security** 和 **Spring Boot** 这两门技术，并且所有的依赖采用的都是最新的稳定版本。初次之外，这个项目还用到了 JPA 这门技术。

由于自己的能力以及时间有限，所以一定还有很多可以优化的地方，有兴趣的朋友可以一起完善，期待你的 PR。

## 介绍

**项目用到的一些框架/服务：**

- **数据库**： H2内存数据库，无需手动安装。
- **缓存**： Redis
- **权限框架** ：Spring Security
- **ORM框架** ：JPA （低SQL）
- **接口文档** ： swagger。在线 API 文档地址：http://localhost:9333/api/swagger-ui/ 。目前使用 knife4j 增强了 swagger 功能，地址： http://localhost:9333/api/doc.html （推荐👍）

**你能从这个项目中学习到什么？**

1. Spring Security +JWT 实现登入登出以及权限校验
2. JPA 实现审计功能、多对多的映射关系如何通过关联表实现
3. ![image-20220331203453908](C:\Users\udyr\AppData\Roaming\Typora\typora-user-images\image-20220331203453908.png)

## 教程

1. [项目讲解/分析](https://github.com/Snailclimb/spring-security-jwt-guide/blob/master/docs/SpringSecurity介绍.md) （内容待重构）
2. [swagger3.0整合](https://github.com/Snailclimb/spring-security-jwt-guide/blob/master/docs/swagger.md)

## 代办

-  增加H2内存数据库支持，无须MySQL，一键启动项目启动后访问 http://localhost:9333/api/h2-console (用户名:root,密码:123456)
-  增加Swagger，方便调用接口
-  异常处理部分代码重构，优化返回结构
-  新建一个role表，然后通过新建一个role_user表的形式，将用户与角色关联起来
-  文件结构重构
-  增加jpa审计功能
-  login（登录）接口在controller层暴露出来
-  登出功能：redis保存token信息（key->user id,value->token），登出后将 redis中的token信息删除
-  重新登录将上一次登录生成的token弄失效（解决未过期的token还是可以用的问题）：重新登录会将 redis 中保存的 token 信息进行更新
-  重构详解文章

## 项目概览

为了区分，我把 Spring Security相关的都单独放在了一个文件夹下面。

[![img](https://github.com/Snailclimb/spring-security-jwt-guide/raw/master/pictures/structure.png)](https://github.com/Snailclimb/spring-security-jwt-guide/blob/master/pictures/structure.png)

## 如何运行项目

1. git clone https://github.com/Snailclimb/spring-security-jwt-guide.git
2. 打开项目并且等待 Maven 下载好相关依赖。建议使用 Intellij IDEA 打开，并确保你的 Intellij IDEA 下载了 lombok 插件。
3. 下载 redis 并`application.yaml`中redis的配置
4. 运行项目（相关数据表会被自动创建，不了解的看一下 JPA）

## 示例

### 1.注册一个账号

**URL:**

```
POST http://localhost:9333/api/users/sign-up
```

**RequestBody:**

```
{"userName":"123456","fullName":"shuangkou","password":"123456"}
```

[![注册](https://github.com/Snailclimb/spring-security-jwt-guide/raw/master/pictures/sign-up.png)](https://github.com/Snailclimb/spring-security-jwt-guide/blob/master/pictures/sign-up.png)

新注册的用户默认绑定的角色为：用户（USER）和管理者（MANAGER）。

### 2.登录

**URL:**

```
POST http://localhost:9333/api/auth/login
```

**RequestBody:**

```
{"username": "123456", "password": "123456","rememberMe":true}
```

[![登录](https://github.com/Snailclimb/spring-security-jwt-guide/raw/master/pictures/login.png)](https://github.com/Snailclimb/spring-security-jwt-guide/blob/master/pictures/login.png)

### 3.使用正确 Token 访问需要进行身份验证的资源

我们使用 GET 请求访问 `/api/users`，这个接口的访问权限是

```
@PreAuthorize("hasAnyRole('ROLE_USER','ROLE_MANAGER','ROLE_ADMIN')")
```

[![Access resources that require authentication](https://github.com/Snailclimb/spring-security-jwt-guide/raw/master/pictures/access-resources-that-require-authentication.png)](https://github.com/Snailclimb/spring-security-jwt-guide/blob/master/pictures/access-resources-that-require-authentication.png)

### 4.不带 Token 或者使用无效 Token 访问

我们使用 GET 请求访问 `/api/users`，但是不带token或者带上无效token。

[![Access resources that require authentication without token or with invalid token](https://github.com/Snailclimb/spring-security-jwt-guide/raw/master/pictures/access-resources-that-require-authentication2.png)](https://github.com/Snailclimb/spring-security-jwt-guide/blob/master/pictures/access-resources-that-require-authentication2.png)

### 5.带了正确Token但是访问权限

我们使用 DELETE 请求访问 `/api/users?username=xxx`，携带有效的 token ，但是 token 的访问权限不够。

[![img](https://github.com/Snailclimb/spring-security-jwt-guide/raw/master/pictures/not-have-enough-permission.png)](https://github.com/Snailclimb/spring-security-jwt-guide/blob/master/pictures/not-have-enough-permission.png)

## 参考

- https://dev.to/keysh/spring-security-with-jwt-3j76