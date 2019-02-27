<p align="center"><img width="100" src="http://php.net/images/logos/php-logo.svg"></p>

## 介绍

<p>PHP session 变量用于存储有关用户会话的信息，或更改用户会话的设置。Session 变量保存的信息是单一用户的，并且可供应用程序中的所有页面使用。</p>

<p>会话是创建动态网站或Web应用程序的关键组件。如果您正在构建这些类型的网站，您肯定会被要求在某些时候处理Sessions</p>

### 什么是会话？

<p>在我们开始实施Sessions解决方案之前，确切了解它们是什么非常重要。</p>

<p>默认情况下，Internet是“无状态”环境。这意味着您在浏览器中发出的每个请求都是匿名的，因为它不以任何方式与先前的请求相关联。当您登录网站时，应用程序必须“记住”您已登录并且您应该与帐户关联。但是，为了维持这种“状态”，我们需要使用Sessions。</p>

<p>会话是记录在服务器和浏览器中的唯一标识符。通过记录此信息，服务器能够跨多个页面请求维护状态。</p>

<p>基本上，每次刷新页面时，互联网都无法记住您。会话只是一个小注释，使服务器和浏览器能够维护某些数据。</p>

## 常见的会话存储方式

- files
- DB
- Redis



### 文件存储

<p>通常情况下，session会被默认存储在文件中.缺点是适合单台服务器部署，多台服务器部署时，会出现Session会话无法保持，用户登陆异常退出.</p>

<p>常见解决方法是：1. session文件共享; 2.nginx ip_hash;</p>

### DB

<p>防止此问题的另一种方法是将Sessions存储在数据库中，而不是存储在文件存储中。幸运的是，数据库中的会话非常简单，并且不会对您的用户产生负面影响。</p>

<p>如果需要将应用程序扩展到多个服务器，则在数据库中存储会话也是一个优势。通过将会话数据存储在可由任何其他服务器访问的中央服务器中，您可以消除可能出现的问题。</p>

### Redis

<p>Redis 会话存储是企业级应用的首选，支持集群部署。</p>

## 实施方法

### 文件存储设置 - Session 默认存储方式，不做过多介绍.

### DB存储设置 - [readme_db.md](https://github.com/amoswdh/PHP-Help/blob/php-session/readme_db.md)

### Redis存储设置

<p>我们将学习如何创建实现PHP SessionHandlerInterface接口的自定义会话处理程序</p>

## session_set_save_handler - 设置用户级会话存储功能

[PHP官网介绍](http://php.net/manual/en/function.session-set-save-handler.php)

## 备注

<p>目前主流的PHP框架都支持Session存储的配置，自定义存储类文件各自有微小差异，但都遵循PHP官方SessionHandlerInterface接口的规范.</p>

