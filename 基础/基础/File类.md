# 一、 File类

## 1.1 概述

`java.io.File`类是文件和目录路径名的抽象表示，主要用于文件和目录的创建，查找和删除等操作



## 1.2 构造方法

![1570006314832](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570006314832.png)

````java
package File;

import java.io.File;

/**
 * @author TJ
 * @dept 上海软件研发中心
 * @description TODO
 * @date 2019/10/2 16:52
 **/
public class File01 {

    public static void main(String[] args) {
        /*
        static String	pathSeparator   与系统相关的路径分隔符字符，为方便起见，表示为字符串。
        static char	pathSeparatorChar  与系统相关的路径分隔符。
        static String	separator  与系统相关的默认名称 - 分隔符字符，以方便的方式表示为字符串。
        static char	separatorChar  与系统相关的默认名称分隔符。

        操作路径：路径蹦年写死了
        C:\tj\a\b.text windows
        C:/tj/a/b.text  Linux
        要如下这样写
       "C:"+File.separator+"tj"+File.separator+"a"+File.separator+"b.text"
         */

        String pathSeparator = File.pathSeparator;

        char pathSeparatorChar = File.pathSeparatorChar;

        String separator = File.separator;

        char separatorChar = File.separatorChar;

        System.out.println("pathSeparator"+pathSeparator); // 路径分隔符 windows ; Linux :

        System.out.println("pathSeparatorChar"+pathSeparatorChar); // 文件名称分隔符 windows：反斜杠 \  Linux:正斜杠 /

        System.out.println("separator"+separator);

        System.out.println("separatorChar"+separatorChar);

        /*
           绝对路径 和 相对路径
           绝对路径：是一个完整的路径
              以盘符（C:,D:）开始的路径
              C:\\毕设项目框架\\moxi-master\\a.text
           相对路径：是一个简化的路径
              相对于当前项目的根目录
              C:\\毕设项目框架\\moxi-master\\a.text -> a.text
         */


    }
}
````

**构造方法案列**：

```java
package File;

import java.io.File;

/**
 *
 **/
public class File02 {

    public static void main(String[] args) {
        /*
           File类的构造方法 File(String pathName)
         */
        show1();

        System.out.println("========================");
          /*
           File类的构造方法 File(String parent,String child) 根据parent路径字符串和child路径名字符串创建一个新File实列
           好处：
               父路径和子路径，可以单独书写，使用起来非常灵活；父路径和子路径都可以变化
         */
        show2("c:","a.text");
        show2("d:","a.text"); //使用起来非常灵活

        System.out.println("========================");

        /*
            File(File parent,String child)
            好处：
               父路径和子路径，可以单独书写，使用起来非常灵活；父路径和子路径都可以变化
               父路径是File类型，可以使用File类的方法，对路径进行一些操作，在使用路径创建对象
         */
        show3();
    }

    /*
       File(String pathName):通过将给定路径名字字符串转换为抽象路径名来创建一个新File实列。
       参数
          String pathName :字符串的路径名称
          路径可以是文件结尾，也可以是文件夹结尾
          路径可以是相对路径，也可以是绝对路径
          路径可以是存在，也可以是不存在
          创建File对象，只是把字符串的路径封装为File对象，不考虑路径的真假情况
     */
    private static void show1(){
        // 相对路径
        File f1 = new File("D:\\A-Tj学习之路\\1.基础班\\1-8 File类与IO流\\第1节 File类\\a.text ");
        System.out.println(f1); // 重写了Object的toString方法

        File f2= new File("D:\\A-Tj学习之路\\1.基础班\\1-8 File类与IO流\\第1节 File类 ");
        System.out.println(f2); // 重写了Object的toString方法

        // 绝对路径
        File f3= new File("b.text ");
        System.out.println(f3); // 重写了Object的toString方法
    }

    private static  void show2(String parent,String child){
        File f1 = new File(parent,child);
        System.out.println(f1);
    }

    private static  void show3() {
        File parent = new File("C:\\");
        File file = new File(parent,"hello.java");
        System.out.println(file);

    }
}

```

## 1.3 常用方法

### 获取功能的方法

```java
public String getAbsolutePath():返回File的绝对路径名字符串 （无论是传递的相对或者绝对路径，都返回绝对）
public String getPath():将此File转换为路径名字符串
public String getName():返回由File表示的文件或目录名称 （结尾部分）
public long length():返回由此File表示的文件长度
```



### 判断功能的方法

```java
public boolean exists():此File表示的文件或目录是否实际存在
public boolean isDirectory():此File表示的是否为目录
public boolean isFile():此File表示的是否为文件
```



### 创建和删除功能的方法

````java
public boolean createNewFile():当且仅当具有该名称的文件尚不存在时
public boolean delete():删除由此File表示的文件或目录
public boolean mkdir():创建由此File表示的目录 （单级文件夹）
public boolean mkdirs():创建由此File表示的目录，包括任务必需但不存在的父目录 （多级文件夹）
````



### 目录遍历的方法

```java
public String[] list():返回一个String数组，表示该File目录中的所有子文件或目录
public File[] listFiles():返回一个File数组，表示该File目录中的所有子文件或目录
```



## 1.4 递归

### 1...n 的相加

![1570425200981](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570425200981.png)

### 递归显示文件

![1570425461665](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570425461665.png)



## 1.5 文件过滤器（FileFilter）

![1570426416653](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570426416653.png)



# 二、 IO流

![1570442666744](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570442666744.png)

|            | 输入流                         | 输出流                          |
| ---------- | ------------------------------ | ------------------------------- |
| **字节流** | 字节输入流<br/>**InputStream** | 字节输出流<br/>**OutputStream** |
| **字符流** | 字符输入流<br/>**Reader**      | 字符输出流<br/>**Writer**       |

## 2.1 字节流

 ### 2.1.1 字节输出流（OutputStream）

![1570442994786](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570442994786.png)

**代码：**

```java
package io;

import java.io.*;

/**
 * OutputStream:字节输出流
   FileOutputStream 文件字节输出流
   作用：把内存中数据写入到硬盘的文件中

   构造方法：
       FileOutputStream(String  name)创建一个向具有指定名称的文件中写入数据的输出文件流
       FileOutputStream（File file)创建一个向指定File对象表示的文件中写入数据的文件输出流
   参数： 写入数据的目的
       String name:目的地是一个文件的路径
       File  file：目的地是一个文件
   构造方法的作用：
        1.创建一个FileOutputStream对象
        2.会根据构造方法中传递的文件/文件路径，创建一个空的文件
        3.会把FileOutputStream对象指向创建好的文件
   写入数据的原理（内存-->硬盘）
        java程序-->JVM(java虚拟机)-->os(操作系统)-->os调用写数据的方法-->把数据写入到文件中
   字节输出流的作用步骤（重点）:
        1.创建一个FileOutputStream对象,构造方法中传递写入数据的目的地
        2.调用FileOutputStream对象中的方法write，把数据写入到文件中
        3.释放资源(流使用会占用一定的内存，使用完毕要把内存清空，提高程序的效率)
 **/
public class OutputStreamDemo1 {
    public static void main(String[] args) throws IOException {
        // 1.创建一个FileOutputStream对象,构造方法中传递写入数据的目的地
        FileOutputStream fos = new FileOutputStream("D:\\A-Tj学习之路\\1.基础班\\1-8 File类与IO流\\第4节 IO字节流\\a.text");
        // 2.调用FileOutputStream对象中的方法write，把数据写入到文件中
        fos.write(97);
        fos.close();

    }
}

```

**原理**：

![1570448890731](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570448890731.png)

**一次写入多个字节：**

````java
package io;

import java.io.FileOutputStream;
import java.io.IOException;

/**
 *  一次写多个字节的方法：
 *     public void write(byte[] b)将b.length字节从指定的字符数组写入此输出流
 *     public void write(byte[] b ,int off,int len)从指定的字符数组写入len字节，从偏移量off开始输出到此输出流
 *
 * **/
public class OutputStreamDemo2 {
    public static void main(String[] args) throws IOException {
        // 1.创建一个FileOutputStream对象,构造方法中传递写入数据的目的地
        FileOutputStream fos = new FileOutputStream("D:\\A-Tj学习之路\\1.基础班\\1-8 File类与IO流\\第4节 IO字节流\\b.text");
        // 2.调用FileOutputStream对象中的方法write，把数据写入到文件中
        // 在文件中写入100，三个字写
        byte[] b ={49,48,48};
        fos.write(b);
        fos.write(b,0,1);
        byte[] b1 = "你好".getBytes();
        fos.write(b1);
        fos.close();

    }
}

````

**续写和换行：**

```java
package io;

import java.io.FileOutputStream;
import java.io.IOException;

/**
 *  追加写/续写：使用两个参数的构造方法
 *      FileOutputStream(String name,boolean append)创建一个向具有指定name的文件中写入数据的输出文件流
 *      FileOutputStream(File file,boolean append)创建一个向指定File对象表示的文件中写入数据的文件输出流
 *      参数：
 *          String name,File file:写入数据的目的地
 *          boolean append：追加写开关
 *              true:创建对象不会覆盖原文件，继续在文件的末尾追加写数据
 *              false：创建一个新文件，覆盖原文件
 *     写换行：写换行符号
 *          windows:\r\n
 *          Linux:/n
 *          Mmac:/r
 * **/
public class OutputStreamDemo3 {
    public static void main(String[] args) throws IOException {
        // 1.创建一个FileOutputStream对象,构造方法中传递写入数据的目的地
        FileOutputStream fos = new FileOutputStream("D:\\A-Tj学习之路\\1.基础班\\1-8 File类与IO流\\第4节 IO字节流\\c.text",true );
        // 2.调用FileOutputStream对象中的方法write，把数据写入到文件中
        for (int i=0;i<10;i++){
            // 在文件中写入100，三个字写
            fos.write("你好".getBytes());
            fos.write("\r\n".getBytes());
        }
        fos.close();

    }
}

```

### 2.1.2 字节输入流（InputStream）

**读取字符：**

````java
package io;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

/**
 *   java.io.InputStream:字节输入流
 *   这个抽象类是表示输入字节流的所有类的超类。
 *
 *   定义了所有子类公用的方法
 *       int read() 从输入流读取数据的下一个字节。
 *       int  read(byte[] b)  从输入流读取一些字节数，并将它们存储到缓冲区 b 。
 *       close() 关闭此输入流并释放与流相关联的任何系统资源。
 *
 *   java.io.FileInputStream extends InputStream
 *   FileInputStream:文件字节输入流
 *   作用：把硬盘文件中的数据，读取到内存中使用
 *
 *   构造方法：
 *       FileInputStream(File file)  通过打开与实际文件的连接创建一个 FileInputStream ，该文件由文件系统中的 File对象 file命名。
 *       FileInputStream(String name)  通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的路径名 name命名。
 *   参数：
 *       String name :文件的路径
 *       File file:文件
 *
 *   读取数据的原理（硬盘-->内存)
 *
 *   字节输入流的使用步骤（重点）：
 *      1.创建FileInputStream对象，构造方法中绑定要读取的数据源
 *      2.使用FileInputStream对象中的方法read，读取文件
 *      3.释放资源
 *
 *  **/
public class InputStreamDemo1 {
    public static void main(String[] args) throws IOException {

        // 创建FileInputStream对象，构造方法中绑定要读取的数据源
        FileInputStream fs = new FileInputStream("D:\\A-Tj学习之路\\1.基础班\\1-8 File类与IO流\\第4节 IO字节流\\c.text");
        // 2.使用FileInputStream对象中的方法read，读取文件
        int len = 0;
        while ((len = fs.read()) != -1){
            // 读取文件一个字节返回，读取到文件的末尾返回-1
            System.out.println(len);
        }
        fs.close();
    }
}

````

**原理：**

![1570451204573](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570451204573.png)

**一次读取多个字节：**

```java
package io;

import java.io.FileInputStream;
import java.io.IOException;
import java.util.Arrays;

/**
 *   字节输入流一次读取多个字节的方法：
 *   int  read(byte[] b)  从输入流读取一些字节数，并将它们存储到缓冲区 b 。
 *
 *   明确两件事：
 *      1.方法的参数byte[]的作用？
 *           起缓冲作用，存储每次读取到多个字节
 *           数组的长度一般定义为1024（1kb）或者1024整数倍
 *      2.方法的返回值int是什么？
 *           每次读取的有效字节个数
 *
 *  **/
public class InputStreamDemo2 {
    public static void main(String[] args) throws IOException {

        // 创建FileInputStream对象，构造方法中绑定要读取的数据源
        FileInputStream fs = new FileInputStream("D:\\A-Tj学习之路\\1.基础班\\1-8 File类与IO流\\第4节 IO字节流\\c.text");
        // 2.使用FileInputStream对象中的方法read[]，读取文件
        byte[] bytes = new byte[1024];
        int len = 0;
        while( (len = fs.read(bytes)) != -1){
            System.out.println(len+" : "+new String(bytes));
        }
        fs.close();
    }
}

```

**原理：**

![1570452244774](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570452244774.png)

### 2.1.3  文件的复制

**原理：**

![1570452707499](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570452707499.png)

**代码：**

```java
package io;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyFile {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream("D:\\A-Tj学习之路\\1.基础班\\1-8 File类与IO流\\第4节 IO字节流\\c.text");
        FileOutputStream fos = new FileOutputStream("D:\\A-Tj学习之路\\1.基础班\\1-8 File类与IO流\\第4节 IO字节流\\copy.text");

        int len = 0;
        // 当读取一个图片时，将导致循环过多
      /*  while ((len = fis.read()) != -1){
            fos.write(len);
        }*/

      // 优化：使用数组缓冲读取多个字节，写入多个字节
        byte[] bytes = new byte[1024];
        while ((len = fis.read(bytes)) != -1){
            fos.write(bytes,0,len);
        }
        fos.close();
        fis.close();
    }
}
```



### 2.1.4 字节流读取中文的问题

![1570453130497](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570453130497.png)



-----



## 2.2 字符流

### 2.2.1 字符输入流Reader&FileReader类

````java
package io;

import java.io.FileInputStream;
import java.io.FileReader;
import java.io.IOException;

/**
 *   java.io.Reader:字符输入流
 *   这个抽象类是表示输入字节流的所有类的超类。
 *
 *   定义了所有子类公用的方法
 *       int read() 读取单个字符
 *       int  read(byte[] b)  一次读取多个字符，字符放入数组
 *       close() 关闭此输入流并释放与流相关联的任何系统资源。
 *
 *   java.io.FileReader extends InputStreamReader extends Reader
 *   FileReader:读取字符的便捷类
 *
 *
 *   构造方法：
 *       FileReader(File file)
 *       FileReader(String name)
 *   参数：
 *       String name :文件的路径
 *       File file:文件
 *
 *   读取数据的原理（硬盘-->内存)
 *
 *   字符输入流的使用步骤（重点）：
 *      1.创建FileReader对象，构造方法中绑定要读取的数据源
 *      2.使用FileReader对象中的方法read，读取文件
 *      3.释放资源
 *
 *  **/
public class ReaderDemo1 {
    public static void main(String[] args) throws IOException {
        FileReader fr = new FileReader("D:\\A-Tj学习之路\\1.基础班\\1-8 File类与IO流\\第5节 IO字符流\\a.text");
        int len = 0;
        /*while ((len = fr.read()) != -1){
            System.out.print((char)len);
        }*/

        char[] cs = new char[1024];
        while ((len=fr.read(cs))!= -1){
            System.out.print(new String(cs));
        }
        fr.close();
    }
}
````

### 2.2.2 字符输出流Write类&FileWriter类

````java
package io;

import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

/**
 *   java.io.Writer:字符输出流
 *   这个抽象类是表示输出字符流的所有类的超类。
 *
 *   定义了所有子类公用的方法
 *       void writer(int c) 写入单个字符
 *       void  writer(char[] cbuf)  写入字符数组
 *       void writer(String str) 写入字符串
 *       close() 关闭此输入流并释放与流相关联的任何系统资源。
 *
 *   java.io.FileWriter extends OutputStreamWriter extends Writer
 *   FileWriter:读取字符的便捷类
 *
 *
 *   构造方法：
 *       FileWriter(File file)
 *       FileWriter(String name)
 *   参数：
 *       String name :文件的路径
 *       File file:文件
 *
 *   读取数据的原理（硬盘-->内存)
 *
 *   字符输入流的使用步骤（重点）：
 *        1.创建一个FileWriter对象,构造方法中传递写入数据的目的地
 *        2.调用FileWriter对象中的方法write，把数据写入到文件中
 *        3.释放资源(流使用会占用一定的内存，使用完毕要把内存清空，提高程序的效率)
 *
 *  **/
public class WriterDemo1 {
    public static void main(String[] args) throws IOException {
        FileWriter fr = new FileWriter("D:\\A-Tj学习之路\\1.基础班\\1-8 File类与IO流\\第5节 IO字符流\\b.text");
        char[] c = {48,48,48};
        fr.write("我爱你");
        fr.write(97);
        fr.write("ABCDEFG",2,2);
        fr.write(c);
        fr.flush();
        fr.close();
    }
}

````

### 2.2.3 字符输出流的续写与换行

原理同字节输出流的续写与换行一样

```java
FileWriter(String fileName,boolean append);
FileWriter(File fiel,bookean append)
```

![1570536370462](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570536370462.png)



# 三、 文件上传

**原理**：

![1570536735624](C:\Users\TJ\AppData\Roaming\Typora\typora-user-images\1570536735624.png)