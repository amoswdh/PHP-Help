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

### files


### DB


### Redis
