# Redis从入门到崩溃

## 一、应用场景

- 内存数据库（登陆信息、购物车信息、用户浏览记录等）
- 缓存服务器（商品数据、广告数据等）（最多使用）
- 解决分布式集群架构中得session分离问题（session共享）
- 任务队列（秒杀、抢购、12306等）（单线程）
- 支付发布订阅得消息模式（RabbitMQ)
- 应用排行榜
- 网站访问统计
- 数据过期处理（可以精确到毫秒）



##  二、Linux(Centos7 安装)

````java
// 安装
wget http://download.redis.io/releases/redis-5.0.5.tar.gz
// 解压
tar xzf redis-5.0.5.tar.gz
// make 编译c 若make命令不认识 则需要 yum install -y gcc g++
cd redis-5.0.5
make
// 进行编译安装
make install PREFIX=/root/redis
// 后端启动
cp /root/redis-5.0.5/redis.conf ./
// 修改配置文件 daemonize on ==> daemonize yes :/dae搜索
vim redis.conf
// 启动
./redis-server redis.conf
// 查看进程
ps -ef | grep redis
// 正常关闭
./redis-cli shutdown
// 前端启动
./redis-server
````

## 三、常用命令

- **redis-server：启动redis服务**
- **reids-cli：进入redis命令客户端**
- redis-benchmark：性能测试的工具
- redis-check-aof：aof文件进行检查的工具
- redis-check-dump：rdb文件进行检查的工具
- redis-sentinel：启动哨兵监控服务

## 四、运行主机访问

````java
// 打开redis 配置文件
vim /etc/redis.conf
      #bind 127.0.0.1
      bind 0.0.0.0
      protected-mode no

````

## 五、Redis常用命令

## **String类型**

![1570794153643](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570794153643.png)

````java
- set key value // 设置key指和value值
    
- get key // 通过key值获取到对应得value值

- incr key // 递增该key值对应得value值（value必须是整数，若不存在value，默认从0开始）

- incrby key increment // 递增该key值对的value值，increment 递增多少个数

- decr key// 递减该key值对应得value值（value必须是整数，若不存在value，默认从0开始）

- decrby key decrement // 递减该key值对的value值，increment 递增多少个数

- keys * // 查看所有key值

- setnx key value // 实现分布式锁 仅当不存在得时候赋值 0代表成功，1代表失败，只能赋值使用一次

- strlen key // 获取字符串得长度
````



## **Hash类型**

![1570794131050](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570794131050.png)

````java
- hset key field value // 设置key值 field值 value值

- hget key field // 获取key对应得field得value值
    
- hdel key field // 删除某一个字段

- del key // 五种基本数据类型都能删除

- hexits key field // 判断字段是否存在

- hincrby key field increment // 递增字段值

- hexists key field // 判断某个字段是否存在

- hvals key // 获取keu值下得所有value值

- hgetall key // 获取key值下所有的字段和value值
````

**注意事项：**

​        **存储那些对象数据，特别是对象属性经常发生增删改查操作得数据**



##  **List类型**     **（有序重复）**

````java
- lpush key value[value...] // 从左端push多个值
    
- lrange key start end // 输出key值对应得value值，最先进的最后打印 从左入 从左出 end = -1 标识最后一个元素
    
- lpop l1 // 将列表左边得元素从列表移除并返回被移除得元素

// lpush rpop 从左入 从右取 保证顺序

- lrem key count value // 从左边开始删除这个value值得count个

- lindex key index // 获取key值索引对应得value值

````

**应用场景：**

​        **商品得评论表： 用户针对于一商品发布评论。一个商品会被不同得用户进行评论，存储商品评论时，要按时间顺序排序。 用户子啊前端页面查询该商品得评论，需要按照时间顺序降序排序**

​                                      

## **Set类型 （无序不能重复）**

````java
- sadd key memeber // 设置一个key值对应得memeber值
    
- smembers s1 // 查看该key值对应得memebers值

- srem key memeber // 删除该key值下得memeber值

- sismember key member // 判断key值下是否存在该member值

- scrad key // 获取元素个数

- spop key [count] // 随机弹出（count）一个元素

// 集合运算
A-B
- sadd s1 1 2 3 4 5
- sadd s2 2 3 6 7
- sdiff s1 s2 // 1 4 5 减去相同的 留下s1得值
    
A∩B
- sinter s1 s2 // 2 3

A∪B
- sunion s1 s2 // 1 2 3 4 5 6 7

````



## **SortedSet类型zset**   **(有序)** : 存在score使得set集合变得有序

````java
- zadd key score member [score member] // zadd z1 10 a 20 b 30 c
    
- zrange key start end // zrange z1 0 -1 从小到大
    
- zrevrange key start end // zrevrange z1 0 -1 从大到小

- zscore key member // zscore z1 a 

- zrem key member // zrem z1 a 

````

**应用场景：**

​       **商品排行榜:根据商品销售量对商品进行排序显示**



## **通用命令**

````java
- keys pattern // 返回满足pattern得所有key
   
- del key // 删除一个可以

- expire key seconds // 设置key值得生存时间 秒

- ttl key // 查看key值得剩余生存时间

- persist key // 清楚该key得生存时间

- pexpire key milliseconds // 生存时间按设置单位：毫秒

- exists key // 判断key值是否存在

- type key // 判断key值类型

- rename key newKey // 重命名
````



## 六、 事务

- Redis得事务搜索通过**MULTI**,**EXEC**,**DISCARD**和**WATCH**这四个命令来完成得

- Redis得单个命令都是**原子性**的，所以在这里确保事务性得对象是**命令集合**

- Redis将命令集合序列化并确保处于同一事务得**命令集合连续且不被打断**得执行

- Redis不支持事务回滚？

  ​    1.大多数事务失败是因为语法错误或者类型错误，这两种错误，在开发阶段是可预见得

  ​    2.redis为了性能方面忽略了事务回滚

   **通俗时刻：客户调用redis使用命令时，WATCH是在开启事务之前，监控数据变化，MULTI开启事务后，命令会进入一个queue命令集合中，进行编译，DISCARD命令可清除命令集合中得命令，当编译完成后，EXEC执行queue中得命令**

![1570797855154](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570797855154.png)



### 相关命令

- **MULTI**

  **用于标记事务块得开始。** Reids会将后续得命令逐个放入队列中，然后才能试用EXEC命令原子化地执行这个命令序列

  ```java
  语法：multi
  
  127.0.0.1:6379> multi 
  OK
  127.0.0.1:6379> set s1 v1 
  QUEUED
  127.0.0.1:6379> set s2 v2
  QUEUED
  127.0.0.1:6379> get s1
  QUEUED
  127.0.0.1:6379> get s2
  QUEUED
  
  ```

  

- **EXEC**

  **在一个事务中执行所有先前放入队列得命令**，然后恢复正常得连接状态

  ```java
  语法：exec
  
  127.0.0.1:6379> exec
  1) OK
  2) OK
  3) "v1"
  4) "v2"
  ```

  

- **DISCARD** 

  **清楚所有先前在一个事务中放入队列得命令**，然后恢复正常得连接状态

  ````java
  语法：discard
  127.0.0.1:6379> multi
  OK
  127.0.0.1:6379> set s1 v11
  QUEUED
  127.0.0.1:6379> set s2 v22
  QUEUED
  127.0.0.1:6379> discard 
  OK
  127.0.0.1:6379> set s1 v11
  OK
  
  ````

  

- **WATCH**

  当某个事务**需要按条件执行**时，就要使用这个命令将给定得**键设置为受监控**得状态

  ````java
  语法：watch key[key...]
  注意事项：使用该命令可以实现redis得乐观锁
  
  主机1 ：
  127.0.0.1:6379>set s1 v1
  OK
  127.0.0.1:6379>set s2 v2
  OK
  127.0.0.1:6379>watch s1 //带有条件得事务，当s1更改时，事务不允许执行 所以不会产生ABA问题
  OK
  127.0.0.1:6379>multi
  OK
  127.0.0.1:6379>set s2 v2222
  QUEUED
  
  主机2：
  127.0.0.1:6379>set s1 v1111
  OK
  
  主机1：
  127.0.0.1:6379>exec
  (nil)
  127.0.0.1:6379>get s2
  'v2'
  
  ````

  

- **UNWATCH**

  清楚所有先前为一个事务监控得键

  ````java
  语法：unwatch
  ````



## 七、事务失败处理

- Redis语法错误（编译期错误)

  ```java
  127.0.0.1:6379> multi
  OK
  127.0.0.1:6379> sets s1 1111
  (error) ERR unknown command `sets`, with args beginning with: `s1`, `1111`, 
  127.0.0.1:6379> set s1
  (error) ERR wrong number of arguments for 'set' command
  127.0.0.1:6379> set s4 444
  QUEUED
  127.0.0.1:6379> exec
  (error) EXECABORT Transaction discarded because of previous errors.
  127.0.0.1:6379> get s4
  (nil)
  
  ```

  

- Redis类型错误（运行时错误） 

  ````java
  127.0.0.1:6379> multi
  OK
  127.0.0.1:6379> set s4 444
  QUEUED
  127.0.0.1:6379> lpush s4 111 222 
  QUEUED
  127.0.0.1:6379> exec
  1) OK
  2) (error) WRONGTYPE Operation against a key holding the wrong kind of value
  127.0.0.1:6379> get s4
  "444"
  ````

- Redis不支持事务回滚？

  ​    1.大多数事务失败是因为**语法错误或者类型错误**，这两种错误，在开发阶段是可预见得

  ​    2.Redis为了性能方面忽略了事务回滚
  
  

## 八、Redis实现分布式锁

- ### **8.1 锁的处理**

  - 单应用中使用锁：单进程 多线程

    synchronize  lock

  - 分布式应用中使用锁：多进程

- ### 8.2  分布式锁的实现方式

  - 数据库的乐观锁
  - 基于zookeeper的分布式锁
  - **基于redis的分布式锁**

- ### 8.3 分布式锁的注意事项

  - **互斥性：**在任意时刻，只有一个客户端能持有锁
  - **同一性：**加锁和解锁必须是同一个客户端，客户端不能把别人加的锁给解了
  - **避免死锁**：即使有一个客户端在持有锁的期间崩溃而没有主动解锁，也能保证后续其他客户端能加锁

- ### 8.4 实现分布式锁

- #### 8.4.1 获取锁 类似于lock锁

  ````java
  set命令实现 //原子性操作
     set(lockKey,requestId,"NX","EX",timeout) 
      
  /**
  	 * 使用redis的set命令实现获取分布式锁
  	 * @param lockKey   	可以就是锁
  	 * @param requestId		请求ID，保证同一性
  	 * @param expireTime	过期时间，避免死锁
  	 * @return
  	 */
  	public static boolean getLock(String lockKey,String requestId,int expireTime) {
  		//NX:保证互斥性
  		String result = jedis.set(lockKey, requestId, "NX", "EX", expireTime);
  		if("OK".equals(result)) {
  			return true;
  		}
  		
  		return false;
  	}
      
  setnx命令实现
     方法外需要加同步synchronized 解决在设置有效期破坏了原子性问题
      setnx(lockKey,requestId)
          setnx(lockKey,timeout) // 设置有效期 为了满足避免死锁 破坏了原子性
      
      public static synchronized boolean getLock(String lockKey,String requestId,int expireTime) {
  		Long result = jedis.setnx(lockKey, requestId);
  		if(result == 1) {
  			jedis.expire(lockKey, expireTime);
  			return true;
  		}
  		
  		return false;
  	}
  
  ````

  

- #### 8.4.2 释放锁

  ````java
  public static void releaseLock(String lockKey,String requestId) {
  	    if (requestId.equals(jedis.get(lockKey))) {
  	        jedis.del(lockKey);
  	    }
  	}
  ````

  

## 九、Redis的持续化方案 

- #### 9.1 RDB

  ![1571224854091](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1571224854091.png)

  - RDB是Redis**默认**采用的持久化方式

  - RDB方式是通过**快照**完成的，当**符合一定条件**时，Redis会自动将内存中数据进行快照并持久化到硬盘

  - Redis会在指定情况下出发快照

    - **1. 符合自定义配置的快照规则**
    - **2.执行save或者bgsave命令**
    - **3.执行flushall命令**
    - **4.执行主从复制操作**

  - 在redis.conf中设置自定义快照规则

    - 1.RDB持久化条件

      ```
      格式：save <seconds> <changes>
      
      示例：save 900 1 :表示15分钟(900秒)内至少1个键被更改则进行快照
      	 save 300 10 :表示5分钟(300秒)内至少10个键被更改则进行快照
      	 
      可以配置多个条件(每行配置一个条件)，每个条件之间是“或”的关系
      ```

    - 2.配置dir指定rdb快照文件的位置

      ````
      # redis.conf文件中指定文件位置
      dir ./
      ````

    - 3.配置dbfilename指定rdb快照文件的名称

      ````
      # 默认RDB文件名
      dbfilename dump.rdb
      ````

    - **特别说明**

      - Redis启动后会读取RDB快照文件，将数据从硬盘载入到内存
      - 根据数据量大小于结构和服务器性能不同，这个时间也不同。通常将记录一千万个字符串类型键、大小为1GB的快照文件载入到内存中需要花费20~30分钟

- **实现快照过程**

  - redis使用fork函数**复制**一份当前进程的**副本**(子进程)

  - **父进程**继续接收并处理客户端发来的命令，而**子进程**开始将内存中的数据写入硬盘中的临时文件

  - 当子进程写入完所有数据后会**用该临时文件替换旧的RDB文件**，至此，一次快照操作完成

    

![1571232262702](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1571232262702.png)

- **注意事项**
  - redis在进行**快照的过程中不会修改RDB文件**，只有快照 结束后才会将旧的文件替换成新的，也就是说任何时候RDB文件都是完整的
  - 这就使得我们可以通过定时备份RDB文件来实现redis数据库的备份，**RDB文件是经过压缩的二进制文件**，占用空间会小于内存中的数据，更加利于传输

- **RDB优缺点**
  - **缺点**：使用RDB方式实现持久化，一旦Redis异常退出，就会**丢失最后一次快照以后更改的数据**。解决方式就是更改自动快照条件的出发。如果数据相对来说比较重要，希望将损失降到最小，则可以使用AOF方式进行持久化。
  - **优点**：RDB可以最大化Redis的性能：父进程在保存RDB文件时唯一要做的就是fork出一个子进程，然后这个子进程就会处理接下来的所有保存工作，父进程无需执行任何磁盘I/O操作，父子进程是异步处理。同时这个也是一个缺点，如果数据集比较大的时候，fork性能比较耗时，造成服务器在一段时间内停止客户端的请求



- ### 9.2 AOF

  - 默认不开启的持久化方式

  - 开启AOF持久化后每执行一条会**更改Redis中的数据的命令**，Redis就会将该命令写入硬盘的AOF文件，这一过层显然会**降低Redis的性能**，但大部分情况下这个影响是能够接收的，另外**使用较快的硬盘可以提高AOF的性能**

  - 可以通过修改redis.conf配置文件中的appendonly参数开启

    ````
    appendonly yes
    ````

  - AOF文件的保存和RDB文件的位置相同，都是通过dir参数设置的

    ````
    appendfilename appendonly.aof
    
    appendonly.aof文件内容
    $1
    0   // 默认选择数据库0
    *3
    $3
    set
    $2
    tj
    $2
    tj
    *3
    $3
    set
    $2
    tt
    $2
    tt
    ````

- **AOF重写原理(优化AOF文件）**
  - Redis可以在AOF**文件体积变得过大时（可进行配置文件体积限值）**，自动地在后台对AOF进行重写
  - 重写后的新AOF文件包含了恢复当前数据集所需对的**最小命令集合**
  - 整个重写操作是绝对安全的，因为Redis在创建**新的AOF**文件的过程中，会继续将命令追加到**现有的AOF文件**里面，即使重写过程中发生停机，现有的AOF文件也不会丢失。而一旦**新AOF文件**创建完毕，Redis就会从**旧AOF文件切换到新AOF文件**，并开始对新AOF文件进行追加操作
  - AOF文件有序地保存了对数据库执行的所有写入操作，这些写入操作以Redis协议的格式保存，因此AOF文件的内容非常容易被人读懂，对文件进行分析(parse)也很轻松

![1571234678913](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1571234678913.png)

- **redis.conf参数说明**
  - **auto-aof-rewrite-percentage 100** 表示当前aof文件超过上一次文件大小的百分之多少的时候会进行重写。如果之前没有重写过，以启动时aof文件大小为准
  - **auto-aof-rewrite-min-size 64mb**  限制允许重写最小aof文件大小，也就是文件大小小于64mb的时候，不需要进行优化 



- ### 9.3  同步磁盘数据

  Redis每次更新数据的时候，aof机制都会将命令记录到aof文件，但是实际上**由于操作系统的缓存机制**，数据并没有实时写入到磁盘，而是**进入硬盘缓存，在通过硬盘缓存机制去刷新到保存到的文件**

- **参数说明**

  - **appendfsync always**  每次执行写入都会进行同步，这个是最安全但是效率比较低的方式

  - **appendfsync everysec(默认开启）** 每一秒执行 

  - **appendfsync no** 不主动进行同步操作，由操作系统去执行，这个是最快但是最不安全的方式

    

- ### 9.4 AOF文件损坏以后如何修复

  ​      服务器可能在程序正在对AOF文件进行写入时停机，如果停机造成了AOF文件出错(corrupt),那么Redis在重启时会拒绝载入这个AOF文件，从而确保数据的一致性不会被破坏

- **当发生这种请款时，可以用一下方式来修复出错的AOF文件**

  - 为现有的AOF文件创建一个**备份**

  - 使用Redis附带的**redis-check-aof**程序，对原来的AOF文件进行修复。 **redis-check-aof  --fix**

  - 重启Redis服务器，等待服务器载入修复后的AOF文件，并进行数据恢复

    

- ### 9.5 如何选择RDB和AOF

  ````
  #1. 一般来说，如果对数据的安全性要求性非常高的化，应该同时使用两种持久化功能
  
  #2. 如果可以承受数分钟以内的数据丢失，那么可以只使用RDB持久化。有很多用户都只使用AOF持久化，但不推荐这种方式。因为定时生成RDB快照非常便于数据库备份，并且【RDB恢复数据集的速度也要比AOF恢复的速度要快】
  
  #3. 两种持久化策略可以同时使用，也可以使用其中一种。如果同时使用的化，那么Redis重启，会优先【使用AOF文件来还原数据】
  ````



## 十、Reids的主从复制

