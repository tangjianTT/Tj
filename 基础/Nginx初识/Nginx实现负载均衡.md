# 一、负载均衡

　Nginx 服务器是介于客户端和服务器之间的中介，通过上一篇博客讲解的反向代理的功能，客户端发送的请求先经过 Nginx ，然后通过 Nginx 将请求根据相应的规则分发到相应的服务器。

　　![img](https://images2018.cnblogs.com/blog/1120165/201809/1120165-20180908131700302-1529457842.png)

　　主要配置指令为上一讲的 pass_proxy 指令以及 upstream 指令。负载均衡主要通过专门的硬件设备或者软件算法实现。通过硬件设备实现的负载均衡效果好、效率高、性能稳定，但是成本较高。而通过软件实现的负载均衡主要依赖于均衡算法的选择和程序的健壮性。均衡算法又主要分为两大类：

　　静态负载均衡算法：主要包括轮询算法、基于比率的加权轮询算法或者基于优先级的加权轮询算法。

　　动态负载均衡算法：主要包括基于任务量的最少连接优化算法、基于性能的最快响应优先算法、预测算法及动态性能分配算法等。

　　静态负载均衡算法在一般网络环境下也能表现的比较好，动态负载均衡算法更加适用于复杂的网络环境。



## 1、普通轮询算法

 这是Nginx 默认的轮询算法。 

```java
例子：两台相同的Tomcat服务器，通过 localhost:8080 访问Tomcat1，通过 localhost:8081访问Tomcat2，现在我们要输入 localhost 这个地址，可以在这两个Tomcat服务器之间进行交替访问。
```

- **确保两台主机能够访问到Tomcat服务器**

![1570872866884](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570872866884.png)

![1570872877438](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570872877438.png)

- **修改Nginx配置文件**

```
    upstream OrdinaryPolling{
       server 192.168.111.111:8088;
       server 192.168.111.112:8080;
    }
    
       
    server {
        listen       88;
        server_name  localhost;

        location ^~ /tomcat/ {
               proxy_pass http://OrdinaryPolling/;
        }
               
        root   /usr/local/nginx/html/dist;
         
        location / {
            try_files  $uri   $uri/  @router;
            index  index.html;
        }

        location @router{
             rewrite ^.*$ /index.html last;
        }
```

注意：这是一个坑啊 https://my.oschina.net/liuxgnuorg/blog/224493

- 启动 nginx。然后在浏览器输入localhost 地址，观看页面变化：

 　![img](https://images2018.cnblogs.com/blog/1120165/201809/1120165-20180908152509125-1859907953.gif)



## 2、基于比例加权轮询

  　上述两台Tomcat服务器基本上是交替进行访问的。但是这里我们有个需求：

　　**由于Tomcat1服务器的配置更高点，我们希望该服务器接受更多的请求，而 Tomcat2 服务器配置低，希望其处理相对较少的请求。**

　　那么这时候就用到了加权轮询机制了。

　　nginx.conf 配置文件如下：

```
   upstream OrdinaryPolling{
    server 192.168.111.111:8088 weight=5;
    server 192.168.111.112:8080 weight=10;
    }
    
       
    server {
        listen       88;
        server_name  localhost;

        location ^~ /tomcat/ {
               proxy_pass http://OrdinaryPolling/;
        }
               
        root   /usr/local/nginx/html/dist;
         
        location / {
            try_files  $uri   $uri/  @router;
            index  index.html;
        }

        location @router{
             rewrite ^.*$ /index.html last;
        }

```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

　　其实对比上面不加权的轮询方式，这里在 upstream 指令中多了一个 weight 指令。该指令用于配置前面请求处理的权重，默认值为 1。

　　也就是说：第一种不加权的普通轮询，其实其加权值 weight 都为 1。

　　下面我们看页面相应结果：

　　![img](https://images2018.cnblogs.com/blog/1120165/201809/1120165-20180908153300294-418662600.gif)

 

 　明显 8080 端口号出现的次数更多，试验的次数越多越接近我们配置的比例。

## 3、基于IP路由负载

　　我们知道一个请求在经过一个服务器处理时，服务器会保存相关的会话信息，比如session，但是该请求如果第一个服务器没处理完，通过nginx轮询到第二个服务器上，那么这个服务器是没有会话信息的。

　　最典型的一个例子：用户第一次进入一个系统是需要进行登录身份验证的，首先将请求跳转到Tomcat1服务器进行处理，登录信息是保存在Tomcat1 上的，这时候需要进行别的操作，那么可能会将请求轮询到第二个Tomcat2上，那么由于Tomcat2 没有保存会话信息，会以为该用户没有登录，然后继续登录一次，如果有多个服务器，每次第一次访问都要进行登录，这显然是很影响用户体验的。

　　这里产生的一个问题也就是集群环境下的 session 共享，如何解决这个问题？

　　通常由两种方法：

　　1、第一种方法是选择一个中间件，将登录信息保存在一个中间件上，这个中间件可以为 Redis 这样的数据库。那么第一次登录，我们将session 信息保存在 Redis 中，跳转到第二个服务器时，我们可以先去Redis上查询是否有登录信息，如果有，就能直接进行登录之后的操作了，而不用进行重复登录。

　　2、第二种方法是根据客户端的IP地址划分，每次都将同一个 IP 地址发送的请求都分发到同一个 Tomcat 服务器，那么也不会存在 session 共享的问题。

　　而 nginx 的基于 IP 路由负载的机制就是上诉第二种形式。大概配置如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
    upstream OrdinaryPolling{
       ip_hash;
       server 192.168.111.111:8088 weight=5;
       server 192.168.111.112:8080 weight=10;
    }
    
       
    server {
        listen       88;
        server_name  localhost;

        location ^~ /tomcat/ {
               proxy_pass http://OrdinaryPolling/;
        }
               
        root   /usr/local/nginx/html/dist;
         
        location / {
            try_files  $uri   $uri/  @router;
            index  index.html;
        }

        location @router{
             rewrite ^.*$ /index.html last;
        }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

　　注意：我们在 upstream 指令块中增加了 ip_hash 指令。该指令就是告诉 nginx 服务器，同一个 IP 地址客户端发送的请求都将分发到同一个 Tomcat 服务器进行处理。

## 4、基于服务器响应时间负载分配

　　根据服务器处理请求的时间来进行负载，处理请求越快，也就是响应时间越短的优先分配。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 upstream OrdinaryPolling{
       server 192.168.111.111:8088 weight=5;
       server 192.168.111.112:8080 weight=10;
       fair;
    }
    
       
    server {
        listen       88;
        server_name  localhost;

        location ^~ /tomcat/ {
               proxy_pass http://OrdinaryPolling/;
        }
               
        root   /usr/local/nginx/html/dist;
         
        location / {
            try_files  $uri   $uri/  @router;
            index  index.html;
        }

        location @router{
             rewrite ^.*$ /index.html last;
        }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

　　通过增加了 fair 指令。