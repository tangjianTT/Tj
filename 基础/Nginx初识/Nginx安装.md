### **1、安装nginx依赖得PCRE/SSL/Zlib库**

```java
yum install pcre pcre-devel
yum install openssl openssl-devel
yum install zlib zlib-devel
```

### **2.安装/解压/编译**

```java
// 1.下载
wget  http://nginx.org/download/nginx-1.16.1.tar.gz 
// 2.解压
tar zxvf nginx-1.2.7.tar.gz
// 3.安装/编译
cd nginx-1.16.1
    
./configure --prefix=/usr/local/nginx

make && make install


```

### **3.运行nginx**

```java
// 启动
sudo ./nginx

// 查询nginx.conf是否正确
/usr/local/nginx/sbin/nginx -t


```

###  **4.关闭nginx**

- **从容停止**

　　1、查看进程号

```
~]# ps -ef|grep nginx
```

![img](https://img2018.cnblogs.com/blog/1421392/201811/1421392-20181101175933490-60221212.png)

2、杀死进程

```
[root@LinuxServer ~]# kill -QUIT 2072
```

- **快速停止**

1、查看进程号

```
 ~]# ps -ef|grep nginx
```

2、杀死进程

```
[root@LinuxServer ~]# kill -TERM 2132

或 [root@LinuxServer ~]# kill -INT 2132
```

- **强行停止**

```
[root@LinuxServer ~]# pkill -9 nginx
```

2、重启Nginx服务

 

### 

####  方法一：进入nginx可执行目录sbin下，输入命令**./nginx -s reload** 即可

 ![img](https://img2018.cnblogs.com/blog/1421392/201811/1421392-20181101180210337-169781091.png)

 

#### 方法二：查找当前nginx进程号，然后输入命令：kill -HUP 进程号 实现重启nginx服务

![img](https://img2018.cnblogs.com/blog/1421392/201811/1421392-20181101180155223-1874121346.png)

**5.部署vue前端**

````java
将dist包放入到/usr/local/nginx/html/dist

修改配置文件nginx.conf
        root   /usr/local/nginx/html/dist;
         
        location / {
            try_files  $uri   $uri/  @router;
            index  index.html;
        }

        location @router{
             rewrite ^.*$ /index.html last;
        }

````



