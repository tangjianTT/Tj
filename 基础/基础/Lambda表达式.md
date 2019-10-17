# 一、 Lambda表达式

## 1.1函数式编程思想概述

![1570003184592](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570003184592.png)



## 2.2 冗余的Runnable代码

```java
/**

 **/
public class RunnableImpl implements Runnable {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }

    public static void main(String[] args) {
        RunnableImpl ri = new RunnableImpl();
        new Thread(ri).start();

        // 简化代码 使用匿名内部类

        Runnable r2 = new Runnable(){
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName());
            }
        };
        new Thread(r2).start();

        // 简化代码
        new Thread(new Runnable(){
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName());
            }
        }).start();        
    }
}
```

![1570004036968](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570004036968.png)



## 1.3 编程思想转换

![1570004092933](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570004092933.png)

![1570004123554](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570004123554.png)

## 1.4 体验Lambda表达式

````java
public class Lamdba {

    public static void main(String[] args) {
        // 使用Lamdba表达式
        new Thread(()->{
            System.out.println(Thread.currentThread().getName());
        }).start();
    }
}
````

![1570004351936](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570004351936.png)

## 1.5 Lamdba标准格式

Lambda省去面向 对象的条条框框。格式由**3个部分**组成：

- 一些参数

- 一个箭头

- 一段代码

  Lambda表达式的标准格式为：

  ````java
  (参数类型 参数名称) -> {代码语句}
  ````

解释说明格式：

​        ():接口中抽象方法的参数列表，没有参数，就空着；有参数就写出参数，多个参数使用逗号分隔

​        ->:传递的意思，把参数传递给方法体{}

​         {}:重写接口的抽象方法的方法体

## 1.6 练习：使用Lambda标准格式（无参无返回）

**题目**

给一个厨子`Cook`接口，内涵唯一的抽象方法`makeFood`，且无参无返回值，代码如下

```java
package lambda;

/**
 **/
public interface Cook {

    void makeFood();
}

```

在下面的代码中，使用Lambda的标准格式调用`invokeCook`方法

```java
package lambda;

/**
 * @author TJ
 * @dept 上海软件研发中心
 * @description TODO
 * @date 2019/10/2 16:28
 **/
public class CookTest {

    public static void main(String[] args) {
        invoke(new Cook() {
            @Override
            public void makeFood() {
                System.out.println("我吃饭了");
            }
        });

        // 使用Lambda表达式
        invoke(()->{
            System.out.println("我吃饭了");
        });
    }

    public static void invoke(Cook cook){
        cook.makeFood();
    }
}

```

## 1.7 Lambda的参数和返回值

![1570005069826](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570005069826.png)

````java
package lambda;


import java.util.Comparator;

/**
 * @author TJ
 * @dept 上海软件研发中心
 * @description TODO
 * @date 2019/10/2 16:32
 **/
public class Arrays {
    public static void main(String[] args) {
        Person[] per = {
                new Person("tt",123),
                new Person("jj",12),
                new Person("zz",1)
        };

        java.util.Arrays.sort(per, new Comparator<Person>() {
            @Override
            public int compare(Person o1, Person o2) {
                return o1.getAge() - o2.getAge();
            }
        });

        for (Person person : per) {
            System.out.println(person);
        }

        // 使用Lambda表达式
        java.util.Arrays.sort(per,(Person o1 , Person o2)->{
            return o1.getAge() - o2.getAge();
        });


        for (Person person : per) {
            System.out.println(person);
        }

    }
}

````

## 1.8 Lambda表达式可推导可省略

````java
package lambda;

/**
   Lambda表达式：是可推导，可省略
   凡是根据上下文推导出来的内容，都可以省略不写
   可以省略的内容：
       1.（参数列表）：括号中参数列表的数据类型，可以省略不写
       2.（参数列表）：括号中的参数如果只有一个，那么类型和（）都可以省略
       3.{一些代码}：如果{}中的代码只有一行，无论是否有返回值，都可以省略（{}，return，分号）
             注意：要省略{}，return，分号必须一起省略
 **/
public class ArrayList {

    public static void main(String[] args) {
        // 要省略{}，return，分号必须一起省略
        new Thread(()-> System.out.println("xxxx")).start();
        
     Arrays.sort(per,(o1,o2)->o1.getAge() - o2.getAge());


    }
}

````

![1570005972023](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570005972023.png)