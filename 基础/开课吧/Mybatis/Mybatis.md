# 第1章 课前准备工作

## 1.1 开发工具:Eclipse neon1版(绿色免安装)

## 1.2 jar管理工具:Maven-3.3.9(绿色免安装)

## 1.3 数据库: MySql 5.x

## 1.4 项目类型:MAVEN工程

 

# 第2章 MyBatis框架

## 2.1 MyBatis框架简介

#### （1）   MyBatis框架就是对JDBC的封装.主要目的简化JDBC开发流程,实现事务松耦合管理,将实体类与SQL命令进行动态对应.

#### （2）   起源于Apache的Ibatis项目,2010年迁移到Google.被正式命名为MyBatis.最后在2013年迁移到Github

#### （3）   MyBatis框架使用简单,同时由于提供了中文官方文档,一般在一天左右即可以掌握.

## 2.2 MyBatis下载

MyBatis的官网：   https://github.com/mybatis/mybatis-3

 

# 第3章 MyBatis开发流程

## 3.1 添加MyBatis依赖jar

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image002.gif)

## 3.2 开发一个实体映射类

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image004.gif)

## 3.3 开发一个SQL映射文件

在src/main/resource下创建与当前表对应的SQL映射文件用于声明SQL语句

 

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image006.gif)

## 3.4 开发MyBatis核心配置文件

   在src/main/resources下创建MyBatis-config.xml作为核心配置文件

   ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image008.gif)

## 3.5 开发MyBatis基本调用流程

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image010.gif)

 

 

 

 

# 第4章 MyBatis工作原理与工作流程

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image012.gif)

 

 

 

 

# 第5章 MyBatis单表增删改查操作

## 5.1 API介绍

​      在SqlSession接口中提供了四个方法,实现简单的增删改查操作,分别是:

#### （1）    insert方法:实现插入

#### （2）    delete方法:实现删除

#### （3）    update方法:实现更新

#### （4）   select方法:实现查询   

## 5.2 配置测试环境

### 5.2.1        添加Junit依赖jar

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image014.gif)

### 5.2.2         编写测试类

#### （1）   @Test关联的方法,是测试方法.方法声明时只能 public void.方法名可以随意定义

#### （2）   @Before关联的方法,会在测试方法之前执行

#### （3）   @After 关联的方法,会在测试方法之后执行   

## 5.3 插入操作主键值获取

### 5.3.1        当前表支持主键自动增长

在JDBC技术中,可以通过Statement接口中getGeneratedKeys()方法获得本次插入后得到自动增长主键值.MyBatis框架也采用这个技术.因此MyBatis在插入完毕后也可以获得本次插入数据id.做法如下

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image016.gif)

 

 

 

 

 

 

### 5.3.2        当前表不支持主键自动增长

在Mysql数据库中,可以通过max函数获得当前表中最后一条插入数据id.

在MyBatis中,也可以通过这种方式来获得主键值

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image018.gif)

## 5.4 查询操作

### 5.4.1        将查询结果封装为Map集合或则List集合

在SqlSession接口中,可以分别使用selectList方法和selectMap方法将查询结果分别封装为List集合和Map集合

### 5.4.2        查询单个记录

在SqlSession接口中,可以使用selectOne方法获得一个数据行并将数据行封装为一个实体类对象

### 5.4.3        模糊查询

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image020.gif)

# 第6章 MyBatis框架配置文件详解

## 6.1 在Mybatis配置文件获得帮助提升

我们在Mybatis的jar中发现如下两个约束文件mybatis-3-config.dtd和mybatis-3-mapper.dtd

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image022.gif)

 mybatis-3-config.dtd是MyBatis核心配置文件约束

 mybatis-3-mapper.data是MyBatis的SQL映射文件约束

 只要将这两个约束文件引入到Eclipse中,就可以在配置文件开发时获得帮助提示.

 操作步骤如下: 以引入mybatis-3-config.dtd为例

 

  第一步:打开工程中MyBatis核心配置文件,复制约束文件地址

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image024.gif)

 第二步:在window->Preferences下定位XML约束引入向导

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image026.gif)

 

 

​     第三步:指定添加约束文件位置

​     ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image028.gif)

 

 至此,我们就开在MyBatis核心配置文件获得提示帮助了

## 6.2 properties

properties标签可以引入外部属性文件内容

操作如下

第一步:在工程下创建一个config.properties文件存储数据库访问信息

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image030.gif)

第二步:在核心配置文件引入config.propertites并修改数据源参数读取方式

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image032.gif)

 

 

 

在默认情况下, SqlSessionFactoryBuilder会将properties标签指定的属性文件作为默认文件在开发环境(<environments>)使用.当然在一个项目中,也可以有多个properties属性文件,此时可以通过SqlSessionFactoryBuilder来动态指定开发环境(<environments>)依赖的properties属性文件

 

开发环境默认依赖的属性文件

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image034.gif)

在工程中新增的另一个属性文件

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image036.gif)

此时,如果我们需要开发环境依赖config2.properties,可以做如下操作

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image038.gif)

 

 

 

 

 

 

 

 

 

 

 

## 6.3 settings

mybatis全局配置参数，全局参数将会影响mybatis的运行行为。比如：开启二级缓存、开启延迟加载。具体可配置情况如下：

![http://img.blog.csdn.net/20170327105703763](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image040.gif)

## 6.4 typeAliases

​    在mapper.xml中，定义很多的statement，而statement需要parameterType指定输入参数的类型、需要resultType指定输出结果的映射类型。如果在指定类型时输入类型全路径，不方便进行开发，可以针对parameterType或resultType指定的类型定义一些别名，在mapper.xml中通过别名定义，方便开发。

typeAliases是MyBaits框架中提供别名转换器,可以对使用实体类名称设置一个简短的别名,从而简化开发负担.使用方式如下

 

方式一:为每一个实体类都设置一个别名

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image042.gif)

 此时,在SQL映射文件就可以使用这个”Dept”别名了

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image044.gif)

 如果工程中实体类个数比较多,那么用第一种方式就不会很方便.

 

  方式二:为某个包下所有的类设置默认别名,此时别名就是当前类的简单名称

  ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image046.gif)

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

## 6.5 environments 环境

MyBatis 可以配置多种环境。这会帮助你将 SQL 映射应用于多种数据库之中。但是要记得一个很重要的问题：你可以配置多种环境，但每个数据库对应一个 SqlSessionFactory。

所以，如果你想连接两个数据库，你需要创建两个 SqlSessionFactory 实例，每个数据库对应一个。而如果是三个数据库，你就需要三个实例，以此类推。

为了明确创建哪种环境，你可以将它作为可选的参数传递给 SqlSessionFactoryBuilder。

可以接受环境配置的两个方法签名是：

SqlSessionFactory factory = sqlSessionFactoryBuilder.build(reader, environment);

SqlSessionFactory factory = sqlSessionFactoryBuilder.build(reader,environment,properties);

如果环境被忽略，那么默认环境将会被加载，按照如下方式进行：

SqlSessionFactory factory = sqlSessionFactoryBuilder.build(reader);

SqlSessionFactory factory = sqlSessionFactoryBuilder.build(reader,properties);

 

如下图所示,我们配置了两个开发环境,[development1]和[development2].默认的开发环境是[development1].

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image048.gif)

 

 

 

 

 

 

在开发时,如果需要使用[development2],此时可以通过

SqlSessionFactory factory = sqlSessionFactoryBuilder.build(reader, environment);

来制定

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image050.gif)

 

 

## 6.6 databaseIdProvider 数据库厂商标识

## 6.7 mappers 映射器

mappers标签是MyBatis框架提供的SQL映射文件的加载器,用于指定项目中SQL映射文件的位置.用法如下

### 6.7.1        第一种（常用）  <mapper resource=" " />  resource指向的是相对于类路径下的目录  如：<mapper resource="sqlmap/User.xml" />

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image052.gif)

 

### 6.7.2        第二种  <mapper url=" " />  使用完全限定路径  如：<mapper url="file:///D:\workspace\mybatis1\config\sqlmap\User.xml" />

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image054.gif)

### 6.7.3        第三种  <mapper class=" " />  使用mapper接口类路径  如：<mapper class="cn.kang.mapper.UserMapper"/>  注意：此种方法要求mapper接口名称和mapper映射文件名称相同，且放在同一个目录中。

操作如下

 

第一步:在[src/main/java]创建接口[com.kaikeba.dao.IEmployeeDao]

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image056.gif)

第二步:在[src/main/resources]下的[com.kaikeba.dao]文件夹下创建[IemployeeDao.xml]

的SQL映射文件

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image058.gif)

 这两个文件在MAVEN工程位置如下

 ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image060.gif)

### 6.7.4        第四种（推荐）  <package name=""/>  注册指定包下的所有mapper接口  如：<package name="cn.kang.mapper"/>  注意：此种方法要求mapper接口名称和mapper映射文件名称相同，且放在同一个目录中。

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image062.gif)

### 6.7.5        MyBatis中接口和对应mapper文件配置深入解析

首先要说明的问题是，`Mybatis`中接口和对应的mapper文件不一定要放在同一个包下，放在一起的目的是为了`Mybatis`进行**自动扫描**，并且要注意此时**java****接口的名称和****mapper****文件的名称要相同**，否则会报异常，由于此时Mybatis会自动解析对应的接口和相应的配置文件，所以就不需要配置mapper文件的位置了。

#### （1）   默认MAVEN构建

如果在工程中使用了maven构建工具，那么就会出现一个问题：我们知道在典型的maven工程中，目录结构有：`src/main/java`和`src/main/resources`，前者是用来存放java源代码的，后者则是存放一些资源文件，比如配置文件等，**在默认的情况下****maven****打包的时候，对于**`**src/main/java**`**目录只打包源代码，而不会打包其他文件**。所以此时如果把对应的mapper文件放到`src/main/java`目录下时，不会打包到最终的jar文件夹中，也不会输出到`target`文件夹中，由于在进行单元测试的时候执行的是`/target`目录下`/test-classes`下的代码，所以在测试的时候也不会成功。

 

如下情况

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image064.gif)

 此时我们在target中并不会发现有IemployeeDao.xml文件存在

 ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image066.gif)

 

 

 

 

 此时运行程序会抛出如下异常

 ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image068.gif)

**为了实现在****maven****默认环境下打包时，****Mybatis****的接口和****mapper****文件在同一包中，可以通过将接口文件放在**`**src/main/java**`**某个包中，而在**`**src/main/resources**`**目录中建立同样的包**，这是一种约定优于配置的方式，这样在maven打包的时候就会将`src/main/java`和`src/main/resources`相同包下的文件合并到同一包中。

 

如下图所示

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image060.gif)

此时在target下,我们是可以看到IemployeeDao.xml文件的

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image070.gif)

 此时程序可以正常运行

#### （2）    更改MAVEN配置

如果不想将接口和mapper文件分别放到`src/main/java`和`src/main/resources`中，而是全部放到`src/main/java`，那么在构建的时候需要指定maven打包需要包括xml文件，具体配置如下：

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image072.gif)

## 6.8    typeHandlers 类型转换器

每当MyBatis 设置参数到PreparedStatement 或者从ResultSet 结果集中取得值时，就会使用TypeHandler 来处理数据库类型与java 类型之间转换。下表描述了默认TypeHandlers

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image074.jpg)

 

 

# 第7章 MyBatis框架Mapper配置文件详解

## 7.1 Mapper配置文件标签介绍

#### （1）     `insert` – 映射插入语句

#### （2）     `update` – 映射更新语句

#### （3）   `  delete` – 映射删除语句

#### （4）     `select` – 映射查询语句

#### （5）   `  sql` – 可被其他语句引用的可重用语句块

#### （6）     resultMap-确定实体类属性与表中字段对应关系

## 7.2 mapper中nameSpace

​     <mapper>标签是SQL映射文件中根目录标签.在这个标签中只有输一个属性

​     <mapper namesapce=””>

### 7.2.1        namespace属性有什么作用呢?

​     在MyBatis中，Mapper中的namespace用于绑定Dao接口的，即面向接口编程。

它的好处在于当使用了namespace之后就可以不用写接口实现类，业务逻辑会直接通过这个绑定寻找到相对应的SQL语句进行对应的数据处理

 

 

 

 

 

### 7.2.2        namespace属性赋值规则

#### （1）    规则1: 短名称（比如“selectAllThings”）如果全局唯一也可以作为一个单独的引用

nameSpace的值可以是一个简短的唯一字符串

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image076.gif)

#### （2）   规则2:接口完全限定名

 

 也可以是当前工程中一个接口完整路径

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image078.gif)

## 7.3 parameterType属性

在<insert>,<update>,<select>,<delete>标签中,可以通过parameterType指定输入参数的类型，类型可以是简单类型、hashmap、pojo的包装类型。

parameterType属性是可以省略的.MyBatis框架可以根据SqlSession接口中方法的参数

来判断输入参数的实际数据类型.

## 7.4 参数(#{参数名})

​     \#{}实现的是向prepareStatement中的预处理语句中设置参数值，sql语句中#{}表示一个占位符即?

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image080.gif)

​    使用#{参数名},将参数的内容添加到sql语句中指定位置.

 

​    如果当前sql语句中只有一个参数,此时参数名称可以随意定义

​    但是,如果当前sql语句有多个参数,此时参数名称应该是与当前表关联[实体类的属性名]或则[Map集合关键字]

​    ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image082.gif)

​     上述SQL语句在调用时,我们可以分别采用如下两种方式输入参数

​     ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image084.gif)

​     使用#{}读取实体类对象属性内容

 

​    ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image086.gif)

​     使用#{}读取map集合中关键字的值

## 7.5 #{}和${}区别

​    在MyBatis中提供了两种方式读取参数的内容到SQL语句中,分别是

\#{参数名} :实体类对象或则Map集合读取内容

 ${参数名} :实体类对象或则Map集合读取内容

 

为了能够看到两种方式的区别,需要看到MyBatis执行时输送的SQL情况.因此

需要借助Log4J日志进行观察

 

第一步: 加载Log4j日志工具包到项目

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image088.gif)

第二步:将Log4j配置文件添加到src/main/resources下

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image090.gif)

 接下来,我们可以查看

 ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image092.gif)

输出结果

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image094.gif)

 从这里我们可以看出两者的区别:

 \#{} : 采用预编译方式,可以防止SQL注入

 ${}:  采用直接赋值方式,无法阻止SQL注入攻击

 

 在大多数情况下,我们都是采用#{}读取参数内容.但是在一些特殊的情况下,我们还是需要使用${}读取参数的.

 

比如 有两张表,分别是emp_2017 和 emp_2018 .如果需要在查询语句中动态指定表名,就只能使用${}

<select>

​      select *  from emp_${year}

<select>

 

再比如.需要动态的指定查询中的排序字段,此时也只能使用${}

<select>

​       select  *  from dept order by ${name}

</select>

 

简单来说,在JDBC不支持使用占位符的地方,都可以使用${}

## 7.6 resultType属性

#### （1）   resultType属性存在<select>标签.负责将查询结果进行映射.

#### （2）   resultType属性可以指定一个基本类型也可以是一个实体类类型

#### （3）   使用resultType属性为实体类类型时，只有查询出来的列名和实体类中的属性名一致，才可以映射成功. 如果查询出来的列名和pojo中的属性名全部不一致，就不会创建实体类对象.但是只要查询出来的列名和实体类中的属性有一个一致，就会创建实体类对象

#### （4）   resultType属性无法与resultMap属性同时出现.

 

 

 

 

 

 

 

 

 

 

 

 

 

## 7.7  resultMap

​     MyBatis框架中是根据表中字段名到实体类定位同名属性的.如果出现了实体类属性名与表中字段名不一致的情况,则无法自动进行对应.此时可以使用resultMap来重新建立实体类与字段名之间对应关系.

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image096.gif)

 

## 7.8  sql标签

  首先,我们如下两条SQL映射

​     ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image098.gif)

 这两条查询映射中要查询的表以及查询的字段是完全一致的.因此可以<sql>标签

 将[select  *  from dept]进行抽取.

 ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image100.gif)

  在需要使用到这个查询的地方,通过<include>标签进行引用

 

 

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image102.gif)

# 第8章 MyBatis接口编程

## 8.1 传统的MyBatis开发流程问题

在讲Mybatis接口式编程之前，我们先回忆一下前面是如何调用映射文件中的SQL代码的。通常情况下，都是使用SqlSession实例的selectXXX(selectOne, selectList, selectMap)方法来执行映射文件中相应的SQL语句的，这些方法都有一个共同的特征，那就是第一个参数都是String类型的，我们需要使用这个参数明确告之Mybatis我们是需要执行映射文件的哪一个元素下的SQL语句，所以这个参数内容应该是映射文件的名称空间加上相应元素的id值，如：

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image104.gif)

 

 

 

这里存在一些潜在的问题：

·        为了确保名称空间的唯一性，通常会使用相对较长的、且有一定含义的字符串来作为其值，这样就很难保证我们在代码不出现拼写错误的情况，即使是直接从映射文件拷贝过来的，也存在不经意间被修改的可能性；

·        从selectXXX方法的签名可以看到，她的第二个参数是Object类型，那么如果我们传入的参数类型与映射文件中由parameterType属性指定的类型不一致时，将会出现不可预知的错误

·        同样，selectXXX方法返回值使用了泛型，我们须确保用于接收其返回值的变量类型与映射文件中属性resultType指定的类型相一致

Mybatis为了规避上述风险,提供了接口编程

## 8.2 什么是接口编程

接口式编程，我们可以简单的理解为Mybatis为映射文件定义了一个代理接口，以后全部通过这个接口来和映射文件交互，而不再是使用以前方法。

## 8.3 接口编程实现

#### （1）    声明一个接口

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image106.gif)

 

 

 

 

 

 

 

 

#### （2）   修改Mapper文件命名空间

 

命名空间应该是接口完整名称   

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image108.gif)   

#### （3）   修改Mapper文件SQL语句ID ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image110.gif)

#### （4）   测试调用

#### ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image112.gif)    

 

 

 

 

 

 

 

 

# 第9章 MyBatis动态SQL

## 9.1 什么是MyBatis动态SQL

​     根据用户提供的参数,动态决定查询语句依赖的查询条件或则SQL语句的内容

## 9.2 动态SQL依赖标签

### 9.2.1        if

### 9.2.2        choose、when、otherwise

### 9.2.3        trim、where、set

### 9.2.4        foreach

## 9.3  if使用

​     ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image114.gif)

 

 

 

 

 

 

## 9.4 **choose****、****when****、****otherwise**

类似于Java中的switch case default. 只有一个条件生效,也就是只执行满足的条件when,没有满足的条件就执行otherwise,表示默认条件

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image116.gif)

## 9.5 where

<where>可以自动的将第一个条件前面的逻辑运算符(or ,and)去掉

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image118.gif)

 

 

 

 

 

 

 

 

 

## 9.6 set

会在成功拼接的条件前加上SET单词且最后一个”,”号会被无视掉

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image120.gif)

## 9.7 trim

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image122.gif)

## 9.8 foreach

foreach标签用于对集合内容进行遍历,将得到内容作为SQL语句的一部分.

在实际开发过程中主要用于in语句的构建和批量添加操作

 

foreach元素的属性主要有 item，index，collection，open，separator，close。

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image124.gif)

 

 案例1.使用foreach实现批处理添加

 ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image126.gif)

 

 案例2.使用foreach遍历list集合作为查询条件

 ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image128.gif)

 案例3.使用foreach遍历数组作为查询条件

  ![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image130.gif)

 

 

 

案例4.使用foreach遍历Map作为查询条件

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image132.gif)

# 第10章        MyBatis级联操作

在实际开发中,我们操作的表往往不是一个独立个体.它们往往根据业务依赖关系,形成一对一,一对多,多对多关联关系.为了保证业务数据的完整性.我们在操作某一张表的时候也要对与这张表关联其它表进行操作.这样的操作就成为级联操作

## 10.1        级联操作分类

### 10.1.1    级联查询

### 10.1.2    级联删除

### 10.1.3    级联更新

### 10.1.4    级联添加

 

## 10.2        级联查询

### 10.2.1    一对多查询

### 10.2.2    多对一查询

### 10.2.3    多对多查询

 

 

# 第11章        MyBatis注解开发

## 11.1        CRUD操作

### 11.1.1    @Insert

### 11.1.2    @Update

### 11.1.3    @Delete

### 11.1.4    @Select

![img](file:///C:/Users/TJ/AppData/Local/Temp/msohtmlclip1/01/clip_image134.gif)

### 11.1.5    @SelectProvider

### 11.1.6    @InsertProvider

### 11.1.7    @DeleteProvider

### 11.1.8    @UpdateProvider        