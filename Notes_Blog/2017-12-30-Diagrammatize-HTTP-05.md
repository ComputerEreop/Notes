---
title: 《图解HTTP》读书笔记
subtitle: 第05章 与HTTP协作的Web服务器
author: Angus Liu
cdn: header-off
header-img: https://i.loli.net/2017/12/30/5a477fadc7242.jpg
date: 2017-12-30 19:54:33
tags:
      - 《图解HTTP》
      - 读书笔记
      - HTTP
---
> Death is just a part of life, something we're all destined to do.
> 死亡是生命的一部分，是我们注定要做的一件事。
> <p align="right"> —— 罗伯特·泽米吉斯《阿甘正传》 </p>

## 5.1 用单台虚拟主机实现多个域名
(1) HTTP/1.1规范允许一台HTTP服务器搭建多个Web站点。这是因为利用了虚拟主机（Virtual Host，又称虚拟服务器）的功能，将物理层上的一台服务器虚拟为多台服务器。
(2) 在相同的IP地址下，由于虚拟主机可以寄存多个不同主机名和域名的Web网站，因此在发送HTTP请求时，必须在Host首部内完整指定主机名或域名的URI。
![2f0096e3-c3f4-4dc7-884e-16f0e26e6c5a](https://i.loli.net/2017/12/30/5a477ff9c56d3.jpg)

## 5.2 通信数据转发程序：代理、网关、隧道
(1) HTTP通信时，除客户端和服务器以外，还有一些用于通信数据转发的应用程序，例如代理、网关和隧道。它们可以配合服务器进行工作。
(2) 这些应用程序和服务器可以将请求转发给通信线路上的下一站服务器，并且能够接收从那台服务器发送的响应再转发给客户端。
(3) 代理
代理是一有转发功能的应用程序，它扮演了位于服务器和客户端“中间人”的角色，接收由客户端发送的请求并转发给服务器，同时也接收从服务器返回的响应并装发给客户端。
(4) 网关
网关是转发其他服务器通信数据的服务器，接收从客户端发送来的请求时，它就像自己拥有资源的源服务器一样对接请求进行处理。有时客户端可能都不会察觉，自己的通信目标是一个网关。
(5) 隧道
隧道是在相隔甚远的客户端和服务器两者之间进行中转，并保持双方通信连接的应用程序。
### 5.2.1 代理
![871c2cf3-39ea-4dcb-b6b2-324ce3a46ab3](https://i.loli.net/2017/12/30/5a47803b7f4e2.jpg)
(1) 代理服务器的基本行为就是接受客户端发送的请求后转发给其他服务器。代理不改变请求URI，会直接发送给前方持有资源的目标服务器。
(2) 持有资源实体的服务器被称为源服务器。从源服务器返回的响应经过代理服务器后在传给客户端。
![f8e73032-4090-44df-827e-3ab8f8774e60](https://i.loli.net/2017/12/30/5a478056ca5b6.jpg)
(3) 在HTTP通信过程中，可级联多台代理服务器。请求和响应的转发会经过数台类似锁链一样连接起来的代理服务器。转发时，需要附加Via首部字段以标记经过的主机信息。
![20839607-7de1-44d9-87ca-873f348b315a](https://i.loli.net/2017/12/30/5a47806c648c9.jpg)
(4) 使用代理服务器的理由有：利用缓存技术减少网络带宽的流量，组织内部针对特定网站的访问控制，以获取访问日志为主要目的等。
(5) 代理有多种使用方法，按两种基准分类。一种是是否使用缓存，另一种是是否会修改报文。
&nbsp;① 缓存代理
&nbsp;代理转发响应时，缓存代理（Caching Proxy）会预先将资源的副本（缓存）保存在代理服务器上。当代理再次接收到对相同资源的请求时，就可以不从源服务器那里获取资源，而是将之前缓存的资源作为响应返回。
&nbsp;② 透明代理
&nbsp;转发请求或响应时，不对报文做任何加工的代理类型被称为透明代理（Transparent Proxy）。反之，对报文内容进行加工的代理被称为非透明代理。
### 5.2.2 网关
![319865b9-a180-400c-a4e0-c143f6695cf5](https://i.loli.net/2017/12/30/5a4780afbf796.jpg)
(1) 网关的工作机制和代理十分相似。而网关能使通信线路上的服务器提供非HTTP协议服务。
(2) 利用网关能提高头通信的安全性，因为可以在客户端和网关之间的通信线路上加密以确保连接的安全。比如，网关可以连接数据库，使用SQL语句查询数据。另外，在Web购物网站上进行信用卡结算时，网关可以和信用卡结算系统联动。
5.2.3 隧道
(1) 隧道可按要求建立起一条与其他服务器的通信线路，届时使用SSL等加密手段进行通信。隧道的目的是确保客户端能与服务器进行安全的通信。
(2) 隧道本身不会去解析HTTP请求。也就是说，请求保持原样中转给之后的服务器。隧道会在通信双方断开连接时结束。
![2b6054e2-fe5a-4570-9bca-22e7530ea534](https://i.loli.net/2017/12/30/5a4780d799045.jpg)

## 5.3 保存资源的缓存
(1) 缓存是指代理服务器或客户端本地磁盘内保存的资源副本。利用缓存技术可减少对源服务器的访问，因此也就节省了通信流量和通信时间。
(2) 缓存服务器是代理服务器的一种，并归类在缓存代理类型中。当代理转发雄服务器返回的响应时，代理服务器会保存一份资源的副本。
![7b25221f-8eaa-404c-bfc1-4541358f7d76](https://i.loli.net/2017/12/30/5a4780f8e98f3.jpg)
(3) 缓存服务器的优势在于利用缓存可避免多次从源服务器转发资源。因此，客户端可就近从缓存服务器上获取资源，而源服务器也不必多次处理相同的请求。
### 5.3.1 缓存的有效期限
即使存在缓存，也会因为客户端的要求、缓存的有效期等因素，向源服务器确认资源的有效性。若判断缓存失效，缓存服务器将会再次从源服务器上获取“新”资源。
![5b6e9947-b013-473d-a4eb-b847b3764608](https://i.loli.net/2017/12/30/5a478119879a8.jpg)
### 5.3.2 客户端的缓存
(1) 缓存不仅可以存在于缓存服务器中，还可以存在客户端浏览器中。客户端缓存称为临时网络文件（Temporary Internet File）。
(2) 浏览器缓存如果有效，就可以直接从本地磁盘读取。当判定过期后，会向源服务器确认资源的有效性。若失效，浏览器会再次请求新资源。
