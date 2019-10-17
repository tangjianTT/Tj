https://www.cnblogs.com/ysocean/p/9384880.html

### 1、nginx.conf 的主体结构

　　打开此文件，内容如下：

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

　　# 开头的表示注释内容，我们去掉所有以 # 开头的段落，精简之后的内容如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 worker_processes  1;
 2 
 3 events {
 4     worker_connections  1024;
 5 }
 6 
 7 
 8 http {
 9     include       mime.types;
10     default_type  application/octet-stream;
11 
12 
13     sendfile        on;
14 
15     keepalive_timeout  65;
16 
17     server {
18         listen       80;
19         server_name  localhost;
20 
21         location / {
22             root   html;
23             index  index.html index.htm;
24         }
25 
26         error_page   500 502 503 504  /50x.html;
27         location = /50x.html {
28             root   html;
29         }
30 
31     }
32 
33 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 　根据上述文件，我们可以很明显的将 nginx.conf 配置文件分为三部分：

### 2、全局块

　　从配置文件开始到 events 块之间的内容，主要会设置一些影响nginx 服务器整体运行的配置指令，主要包括配置运行 Nginx 服务器的用户（组）、允许生成的 worker process 数，进程 PID 存放路径、日志存放路径和类型以及配置文件的引入等。

　　比如上面第一行配置的：

```
worker_processes  1;
```

　　这是 Nginx 服务器并发处理服务的关键配置，worker_processes 值越大，可以支持的并发处理量也越多，但是会受到硬件、软件等设备的制约，这个后面会详细介绍。

### 3、events 块

　　比如上面的配置：

```
events {
    worker_connections  1024;
}
```

　　events 块涉及的指令主要影响 Nginx 服务器与用户的网络连接，常用的设置包括是否开启对多 work process 下的网络连接进行序列化，是否允许同时接收多个网络连接，选取哪种事件驱动模型来处理连接请求，每个 word process 可以同时支持的最大连接数等。

　　上述例子就表示每个 work process 支持的最大连接数为 1024.

　　这部分的配置对 Nginx 的性能影响较大，在实际中应该灵活配置。

### 4、http 块

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 http {
 2     include       mime.types;
 3     default_type  application/octet-stream;
 4 
 5 
 6     sendfile        on;
 7 
 8     keepalive_timeout  65;
 9 
10     server {
11         listen       80;
12         server_name  localhost;
13 
14         location / {
15             root   html;
16             index  index.html index.htm;
17         }
18 
19         error_page   500 502 503 504  /50x.html;
20         location = /50x.html {
21             root   html;
22         }
23 
24     }
25 
26 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

　　这算是 Nginx 服务器配置中最频繁的部分，代理、缓存和日志定义等绝大多数功能和第三方模块的配置都在这里。

　　需要注意的是：http 块也可以包括 **http全局块**、**server 块**。



#### ①、http 全局块

　　http全局块配置的指令包括文件引入、MIME-TYPE 定义、日志自定义、连接超时时间、单链接请求数上限等。



#### ②、server 块

　　这块和虚拟主机有密切关系，虚拟主机从用户角度看，和一台独立的硬件主机是完全一样的，该技术的产生是为了节省互联网服务器硬件成本。后面会详细介绍虚拟主机的概念。

　　每个 http 块可以包括多个 server 块，而每个 server 块就相当于一个虚拟主机。

　　而每个 server 块也分为全局 server 块，以及可以同时包含多个 locaton 块。

　　**1、全局 server 块**

　　最常见的配置是本虚拟机主机的监听配置和本虚拟主机的名称或IP配置。

　　**2、location 块**

　　一个 server 块可以配置多个 location 块。

　　这块的主要作用是基于 Nginx 服务器接收到的请求字符串（例如 server_name/uri-string），对虚拟主机名称（也可以是IP别名）之外的字符串（例如 前面的 /uri-string）进行匹配，对特定的请求进行处理。地址定向、数据缓存和应答控制等功能，还有许多第三方模块的配置也在这里进行。