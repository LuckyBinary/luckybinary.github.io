---
title: RESTful API
date: 2021-05-14 13:49:48
tags: [RESTful API]
---

## REST

### REST是什么

- 万维网软件架构**风格**
- 用来创建网络服务

### 为何叫REST

{% note success %}
Representational State Transfer
{% endnote %}

- `Representational`：数据的表现形式（JSON、XML...）
- `State`：当前状态或者数据
- `Transfer`：数据传输

### REST的6个限制

客户-服务器（Client-Server）

- 关注点分离
- 服务器专注数据存储，提升了简单性
- 前端专注用户界面，提升了可移植性

无状态

- 所有用户回话信息都保存在客户端
- 每次请求必须包括所有信息，不能依赖上下文信息
- 服务端不用保存会话信息，提升了简单性、可靠性、可见性

缓存（Cache）

- 所有服务端响应都要被标为可缓存或不可缓存
- 减少前后端交互，提升了性能

统一接口（Uniform Interface）

- 接口设计尽可能统一通用，提升了简单性、可见性
- 接口与实现解耦，使前后端可以独立开发迭代

分层系统（Layered System）

- 每层只知道相邻的一层，后面隐藏的就不知道了
- 客户端不知道是和代理还是真实服务器通信
- 其它层：安全层、负载均衡、缓存层等

按需代码（Code-On-Demand可选）

- 客户端可以下载运行服务端传来的代码（比如JS）
- 通过减少一些功能，简化了客户端

### 统一接口的限制

资源的标识

- 资源是任何可以命名的事务，比如用户、评论等
- 每个资源可以通过URI被唯一地识别
  - `http://api.github.com/users`
  - `http://api.github.com/users/123`

通过表述来操作资源

- 表述就是`Representation`，比如JSON、XML等
- 客户端不能直接操作（比如SQL）服务器资源
- 客户端应该通过表述（比如JSON）来操作资源

自描述消息

- 每个消息（请求或响应）必须提供足够的信息让接受者理解
- 媒体类型（application/json、application/xml）
- HTTP方法：GET（查）、POST（增）、DELETE（删）
- 是否缓存：Cache-Control

超媒体作为应用状态引擎

- 超媒体：带文字的链接
- 应用状态：一个网页
- 引擎：驱动、跳转
- 合起来：点击链接跳转到另一个网页

## RESTful API

### 什么是RESTful API

符合`REST`架构风格的API。

### RESTful API 构成

- 基本的URI，如`https://api.github.com/users`
- 标准的HTTP方法，如GET、POST、PUT、PATCH、DELETE
- 传输的数据媒体类型，如JSON、XML

举例：

- GET /users #获取user列表
- GET /users/12 #查看某个具体的user
- POST /users #新建一个user
- PUT /users/12 #更新user 12
- DELETE /users/12 #删除user 12

### RESTful API设计最佳实践

#### 请求设计规范

- URI使用名词，尽量用复数，如/users
- URI使用嵌套表示关联关系，如/users/12/repos/5
- 使用正确的HTTP方法，如GET/POST/PUT/DELETE
- 不符合CRUD的情况：POST/action/子资源

#### 响应设计规范

- 查询
- 分页
- 字段过滤
- 状态码
- 错误处理

#### 安全

- HTTPS
- 鉴权
- 限流

#### 开发者友好

- 文档
- 超媒体

#### 增删改查应该返回什么响应

- 增、改：当前增加或修改的数据
- 查：查询的数据
- 删：返回204
