# 一、 常用API

  ## 1. Scanner

功能：实现键盘输入的数据，到程序当中

引用类型的一般使用步骤：

 ```java
1.导包
import 包路径.类名称；
如果需要使用的目标类，和当前类位于同一个包下，则可以省略导包语句不写。
只有java.lang包下的内容不需要导包，其他的包都需要import语句

2.创建
类名称 对象名 = new 类名称();

3.使用
对象名.成员方法();
 ```

工具类API:

```java
// 创建  System.in 代表从键盘进行输入
Scanner sc = new Scanner(System.in);
// 获取到数据 int类型
int sc = sc.nextInt(); 
// 字符串 
String str = sc.next();
//其实，手动输入的都是字符串类型，next(),它后面的数据类型是进行转型
```

## 2. Random

功能：产生随机数

```java
// 创建
Random  r = new Random();
// 产生随机数
int num = r.nextInt();
// 产生固定区间随机数 [0-3)
int num = r.nextInt(3);
// 产生固定区间随机数[100,200]
int num = r.nextInt(101)+100
```

## 3. Arrays

功能：数组有关的方法

```java
// public static  String toString(数组) 将数组变成字符串格式
int intAry = {10,20,30};
String intStr = Arrays.toString(intAry); // [10,20,30]
// public static void sort(数组) 排序 数组：升序  字母：升序
Arrays.sort(intAry);


```

##  4. Math

功能：数学相关的方法

````java
// 获取绝对值
Math.abs(-1);  // 1
// 向上取整
Math.ceil(12.1) // 13
// 向下取整
Math.floor(12.9) // 12   
// 四舍五入
Math.round(12.45) // 12
// 最大值
Math.max(a,b)
// 最小值
Math.min(a,b)
````

## 5. Object类

功能：所有类的父类/超类

```java
// String toString() 返回该对象的字符串表示
Person p = new Person();
print(p.toString()); // 需重写，不然打印该对象的地址 print(p)

// boolean equals(Object obj) 比较对象
// 源码 ：  return  this == obj; 
// == 比较运算符 返回的是一个布尔值 true false
// 基本数据类型：比较的是值
// 引用数据类型：比较的是两个对象的地址值
// 
```

## 6. Date时间日期类

功能：表示特定的时间（一个时间点，一刹那时间），精确到毫秒

- 毫秒值的作用：可以对时间和日期进行计算

- 2099-01-03 到 2088-01-01中间一共有多少天

- 可以把日期转换为毫秒进行计算，计算完后再把毫秒转换成日期

![1569326381052](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1569326381052.png)

````java
// 构造无参 Date() 获取当前系统的日期和时间
Date date = new Date(); 
// 构造有参 Date(long date) 把毫秒值转换为对应日期 0L-->原点
Date date = new Date(0);
// getTime() 把日期转换成毫秒值 相当于 System.currentTimeMillis方法
long time = date.getTime();


// DateFormat:日期/时间格式化子类的抽象类
// 作用： 日期 --> 文本  文本 --> 日期
// 成员方法：
// String format(Date date) 按照指定的格式，把日期-->符合格式的字符串 
// Date parse(String source) 符合模式的字符串-->Date日期
````

## 7. Calendar日历类

功能：同Date类，但它是个抽象类，无法直接实例化，里面有一个静态方法，getInstance () ,返回的就是Calendar子类对象，提供了很多操作日历字段的方法（YEAR,MONTH,DAY_OF_MONTH,HOUR）

```java
// 获得Calendar
Calendar c = Calendar.getInstance();//多态
// int get(int field); 返回给指定日历字段的值
// void set(int field,int value); 将个定的日历字段设置为给定值
// abstract void add(int field,int amount); 根据日历的规则，为给定的日历字段添加或减去指定的时间量。
// Date getTime(); 返回一个表示此Calendar时间值（从厉元到现在的毫秒偏移量）的Date对象

/*
  int get(int field);
*/
int year = c.get(Calendar.YEAR); // 2019
int month = c.get(Calendar.MONTH) + 1 ; //东西方问题，东方1-12 西方0-11

/*
  void set(int field,int value);
*/
c.set(Calendar.YEAR,9999); //设置年为9999
c.set(8888,8,8); //同时设置年月日，重载方法


/*
  abstract void add(int field,int amount);
*/
c.add(Calendar.YEAR,1); // 增加一年，10000 + 增加 - 减少

/*
   Date getTime();
*/
Date date = Calendar.getTime();
```

## 8. System类

````java
// public static long currentTimeMilllis(); 返回当前系统时间，毫秒级 测试程序的效率
// public static void arraycopy(Object src,int srcpos, Object dest,int destPos,int length); 将数组中指定的数据拷贝到另外一个数组中

/*
 public static long currentTimeMilllis();
*/
long time = System.currentTimeMilllis();

/*
 public static void arraycopy(Object src,int srcpos, Object dest,int destPos,int length);
*/
int [] srcAry = {1,2,3,4,5};
int [] destAry = {6,7,8,9,10}
System.arraycopy(srcAry,0,destAry,0,3);// {1,2,3,9,10}
````

## 9.  StringBuilder类

![1569330333652](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1569330333652.png)

````java
// 构造方法 StringBuilder() 构造一个不带任何字符的字符串生成器，其初始容量为16个字符
// 构造方法 StringBuilder(String str) 构造一个字符串生成器，并初始化指定的字符串内容
StringBuilder sb = new StringBuilder();
print(sb);//

StringBuilder sb = new StringBuilder("abc");
print(sb);//abc

/*
  public StringBuilder append(...) 添加任意类型数据的字符串形式，并返回当前对象自身
*/
sb.append("def");// abcdef

/*
  String toString() StringBuilder --> String
*/
String str = sb.toString();
````

## 10. 包装类

| 基本类型 | 对应的包装类 |
| -------- | ------------ |
| byte     | Byte         |
| short    | Short        |
| int      | Integer      |
| long     | Long         |
| float    | Float        |
| double   | Double       |
| char     | Character    |
| boolean  | Boolean      |

![1569332573135](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1569332573135.png)

![1569332599561](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1569332599561.png)

