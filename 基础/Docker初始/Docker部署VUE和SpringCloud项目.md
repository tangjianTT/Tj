## 部署VUE项目

- **先将vue项目(sc-vue)传入到服务器中**

把你的项目打开，ls一下啦。

![img](https://upload-images.jianshu.io/upload_images/6411222-c2cc98fb55540a7b.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/831/format/webp)

- 项目怎么来的就不说了，你会发现你项目里少了两个东西，一个是Dockerfile,另一个是dist。
  Dockerfile没有的话好办，直接创建一个

```undefined
    vi Dockerfile
```

- 进去过后填入下面的内容

```jsx
FROM nginx:latest
MAINTAINER xx
COPY dist/ /usr/share/nginx/html/                                             
```

第一行写的是设置基础镜像，也就是我们刚刚pull下来的nginx镜像，
第二行是写一个作者，写上自己的邮箱就好滴啦，
第三行的意思就是将dist文件夹下面的内容拷贝到/usr/share/nginx/html/这个目录下。
这个目录是不是很眼熟？这个路径就是nginx一般的项目地址路径。还记得nginx的测试页面在哪儿吗,就是这个路径下的index.html啦。

没有dist文件夹怎么办？更简单啦，vue项目下npm run build一下下啦。一般来说，项目成熟了部署的时候就不带源码了，直接带这个文件夹到地方部署就好了嘛。

## 好了 准备开始创建自己的镜像了

- **在Dockerfile的目录下执行**

```undefined
    docker build -t xxx .
    docker build -t sc-vue.
```

- **xxx是你镜像的名字。 特别注意后页面那个点不能省略**

![img](https://upload-images.jianshu.io/upload_images/6411222-d3daa9e49e1ef28d.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/433/format/webp)

- **然后在docker images 一下，你就能看到自己创建的镜像了。**
  **然后执行命令创建容器**

```css
    docker run -d --name xx -p 8848:80 xxx
    docker run -d --name pc -p 8848:80 sc-vue
```

-d：代表后台启动
--name xx：这是创建的容器名称
-p 8848:80: 是将nginx的80映射到你服务器的8848端口(注意你服务器的端口是否开放8848，其他端口也可以)
xxx：是刚刚创建的镜像名称

- **然后执行docker ps**

![1571046193002](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1571046193002.png)

**然后就能看到你创建的容器了。**
**最后打开浏览器输入你的服务器ip端口号就行了**

![1571046211483](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1571046211483.png)





## 部署SpringCloud项目

- 将各个服务进行打成jar包

- 目录格式如下

  ```java
  -- sc-shelectric
      -- eureka-server
      	-- Dockerfile
          -- sc-eureka-server-0.0.1-SNAPSHOT.jar
      -- api-gateway
      	-- Dockerfile
      	-- sc-eureka-server-0.0.1-SNAPSHOT.jar
      -- config-server
      	-- Dockerfile
      	-- sc-config-server-0.0.1-SNAPSHOT.jar
      -- web-auth
      	-- Dockerfile
      	-- sc-web-auth-0.0.1-SNAPSHOT.jar
      -- security-uaa
      	-- Dockerfile
      	-- sc-security-uaa-0.0.1-SNAPSHOT.jar
      -- electric-pc
      	-- Dockerfile
      	-- sc-electric-pc-0.0.1-SNAPSHOT.jar
  ```

- 以eureka-server为案列，以下都是如此操作

  ```java
  // 1.进入该目录下
  [root@tj sc-shelectric]# cd eureka-server/
      
  // 2.查看该目录下的目录结构
  [root@tj eureka-server]# ll
  总用量 48904
  -rw-r--r--. 1 root root      171 10月 15 17:37 Dockerfile
  -rw-r--r--. 1 root root 50069783 10月 11 13:37 sc-eureka-server-0.0.1-SNAPSHOT.jar
      
  // 3. 查看Dockerfile文件 ps：其余服务大同小异
  [root@tj eureka-server]# cat Dockerfile
  FROM java:8  // 获取镜像
  VOLUME /tmp // 挂载数据卷
  ADD /sc-eureka-server-0.0.1-SNAPSHOT.jar eureka.jar // 添加jar包 并改名
  ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/eureka.jar"] // 执行解压命令 
  EXPOSE 8761 // 对外暴露的端口号
  
  // 4.进行创建镜像
  [root@tj eureka-server]# docker build -t eureka-server:1.0 .
     
  // 5.查看镜像
  [root@tj eureka-server]# docker images
  
  // 6.运行容器
  [root@tj eureka-server]# docker run -d -p 8761:8761 eureka-server:1.0
      
  // 7.查看正在运行的容器
  [root@tj eureka-server]# docker ps
      
  // 8.若不在运行，则查看所有容器
  [root@tj eureka-server]# docker container ls -a
  
  // 9.查看该容器的日志
  [root@tj eureka-server]# docker logs -f [容器id]
  
  // 10.访问eureka-server
  [root@tj eureka-server]# curl http://192.168.111.111:8761
  ```

  ![1571210066089](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1571210066089.png)

- 其余服务大同小异



**注意事项**

      ```java
1. 当eureka-server注册中心服务启动后，主机访问的地址是192.168.111.111:8761。因此在其他服务想要注册上去的话，配置文件【defaultZone: http://127.0.0.1:8761/eureka/】 
指向注册中心的地址应该发生改变变成【defaultZone: http://192.168.111.111:8761/eureka/】 
因为每个容器都相当于一台主机，有着自己的网卡，目前自身还为学好荣期间的通信问题
只有更改这个指向注册服务的中心地址，方可注册
2. 还有一种方式解决，就是将配置文件的端口号进行灵活配置 【defaultZone: http://${eureka-ip:192.168.111.111}:8761/eureka/】
在进行java -jar时进行传递参数，有两种方式 一种是运行参数 java -jar --port=8080 一种是jvm参数 java -jar -Dport=8080

      ```

**解决方式如下**
     A.  编写Dockerfile文件

```java
FROM java:8
COPY ./sc-eureka-server-0.0.1-SNAPSHOT.jar /sc-shelectric/sc-eureka-server-0.0.1-SNAPSHOT.jar
COPY ./app-entrypoint.sh /
RUN chmod +x /app-entrypoint.sh

ENTRYPOINT ["/app-entrypoint.sh"]
```

   B.  编写app-entrypoint.sh文件

```java
java
-jar

-Dport=$PORT
-DeurekaServerUrl=$EUREKA_SERVER_URL // 注册地址 这是全局变量
-DipAddress=$IP_ADDRESS

/sc-shelectric/sc-eureka-server-0.0.1-SNAPSHOT.jar

```

​    C.  运行容器

```java
docker create --name config-server -t -p 6869:6869 -e PORT=6869 -e EUREKA_SERVER_URL=http://192.168.111.111:8761/eureka/  
eureka-serveg:1.0.0

docker start eureka-serveg && docker logs -f eureka-server

```

