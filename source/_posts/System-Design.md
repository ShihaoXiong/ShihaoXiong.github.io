---
title: System Design
date: 2021-09-11 21:00:46
tags: [Java, Design]
toc: true
categories: Design
---

## Introduction

**System Design** is the process of defining the architecture, modules, interfaces, and data for a system to satisfy specified **requirements**.

- requirements
  - functional
  - non-functional
- architecture
- modules
- interfaces
- data

<!-- more -->

### System Design Interview

Design a product, or a feature of a product (mostly web applications)

- availability
- scalability vs. extendsibility
- performance
- reliability CAP (consistency, availability, partition tolerance)
- robustness
- cost
- ...

### How can I do well in a system design Interview?

- Understand the goal
  - **Scenarious** and **Use-cases**
  - **Ask** the right questions
  - **Clarify** the problem you need to solve
  - Functional vs. Non-Functional
- Understand the **scope**
  - How many DAU(daily active users)
  - What is the QPS level?
- Start with the **Big-picture**, then **Drill-down**
  - Done is better than perfect
- Provide a **functional** solution
  - Simplify the priblem and provide a straw-man design
  - Web application and data storage
- Reiterate and **optimize**
  - Availability
  - Scalability
  - Bottlenecks
  - Single point failures
  - ...
- Trade-offs
- Make it easy for the Interviewr to put down your positive points
- Keep system scaling, performance, and availability in mind

---

## Web Application

### 典型架构

![](/assets/system-design/01.png)

**Client**

- 与用户的交互
  - 内容呈现
  - 功能的体现
  - 用户输入 (文字、图形、其他...)
- 与后端的信息交流, HTML, XML, JSON
- 数据的本地缓存
- 与其他终端应用的交互，如 Map, Phone call

**HTTP (Hypertext Transfer Protocol) Server**

- HTTP Server 是用于处理和响应终端的 HTTP 请求的，然后根据不同请求的需求，分发到相应的应用服务器 (Application Server)

**Application Server**

- An application server is a software framework that provides both facilities to create web applications and a server environment to run them.

**Storage**

- 存储数据
- 文件系统：网络文件系统 GFS
- 数据库：关系数据库 (MySQL, MSSQL, Oracle)，非关系数据库，SQL

### 系统间的通信

- 域名解析, <u>DNS</u>
- 应用层协议, <u>HTTP</u>, Client 与 HTTP Server 的信息交互
  - HTTP Request 操作类型. GET, POST, PUT, DELETE
  - HTTP Response Code. 200, 300, 4xx, 5xx ...
- 应用层协议是建立在网络通信协议之上的
  - TCP (Transmission Control Protocol) 传输控制协议
    - 可靠的端到端协议通信协议
    - 三次握手, SYN, SYN ACK, ACK
    - 四次挥手
  - UDP
  - IP Internet Protocol
    - 寻址
    - 完成数据包点对点的寻址和传输
    - 不保证数据包已被接受

### 主要的涉及到性能改进的技术和部分

#### Load Balancer

- 分流，分配工作
- 保证服务器不过载
- 当新服务加入，可自动分配，同理，当服务器被移除，或者 fail，取消分配请求

![](/assets/system-design/02.png)
![](/assets/system-design/03.png)

#### Caches

- 提高数据获取速度
- 降低数据服务器的负载
- 增加了写的开销

#### Indexing

- 用于更快的访问数据
- 更多的时候用于数据库
- 牺牲存储空间和写的速度换取更快的读的速度

**Bandwidth throttling (rate limiter)**

- 限制服务器最大的响应 QPS = 1000
- 典型的面试题之一
  - 单位：分/秒/小时
  - 怎么支持 spike 的情况
  - Fix window
  - Sliding window
  - Token bucket
    - 记录上一个 request 的 Timestamp 和当前可以接受的 request 数量
    - 收到新的 request 的时候，取新的 request 的 Timestamp
    - 比较当前 request 的 timestamp 和之前记录的那个的差，相应的给予补给
    - 只要 request credit 数为正，否则丢弃
    - 设置 request credit 的上限

### Web Crawler and Story Feed

**Policies**
The behavior of a Web crawler is the outcome of a combination of policies:

- a selection policy which states the pages to download,
- a re-visit policy which states when to check for changes to the pages,
- a politeness policy that states how to avoid overloading Web sites.
- a parallelization policy that states how to coordinate distributed web crawlers.

**算法和数据结构**
算法: BFS, 数据结构: Queue

1. Get web page content by URL (seed, e.g., sina.com.cn)
2. Parse the page and get hyperlinks
3. Deduplicate the links (do not download the same URL twice)
4. Append new target links to a Queue
5. Get head from the Queue. Go back to 1.

---

## Data Processing

- Data collection
- Storage of data
- Processing of data
- Data analysis
- Data presentation and conclusions
