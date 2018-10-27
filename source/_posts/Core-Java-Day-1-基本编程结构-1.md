---
title: Core Java Day 1--基本编程结构1(Hello World)
date: 2018-10-19 17:02:06
tags: [《Java核心技术》,学习笔记]
categories: [学习笔记,Java]
---
## 关于这个笔记
最近开始想系统性的啃一啃Java，觉得以前自己学的都太乱太杂，反而学艺不精。现在准备开始照着《Java核心技术》开始怼下去，希望自己可以坚持下去吧，这次不弃坑不弃坑 T.T

# 1.基本编程结构

## 一些要点

1. Java中所有的方法必须在类中声明。非静态方法只能在该方法所属类的对象上调用
2. 静态方法不是通过对象调用。程序从静态的main方法开始执行
3. Java有8个基本数据类型： 5个整型 2个浮点型和1个布尔型
4. Java操作符和控制结构与C/Javascript非常相似
5. Math类提供常见的数学函数
6. 字符串对象是字符序列，或者更精确的说 是UTF-16编码中Unicode编码点的序列
7. Java中使用System.out对象 可在终端窗口显示输出 绑定System.in的Scanner可以读取终端输入
8. 数组和集合用来收集相同类型的元素

## 1.1 Hello World
![一个简单的Hello World程序](/img/core_java_post/day1-1.jpg)

**一些解析**

1. Java是面向对象的一门语言，绝大多数情况下通过操作_对象_来进行工作。对象属于某个类，类定义了对象能做什么。在Java中，所有的代码都必须在类中定义好。例如在这个HelloWorld程序由一个HelloWorld类组成。
2. main是方法，也就是在类中声明的函数。main方法是程序运行时调用的第一个方法。它被声明为静态static，表示该方法不是依赖于对象运行的。方法声明为void，指出该方法不会有返回值。
3. 在Java中可以声明一些特性（可见性） 例如public或private
4. package（包） 是一组相关类的集合。每个类都放入一个包中，这样就可以区分相同类名的多个类。
5. 回到main方法的方法体中 它由一行语句组成，作用是讲消息打印到讲讲System.out对象 该对象在Java中代表标准输出

所以，Java被明确定义为可以从组织有序的类和包中获益的大型程序的编程语言

在Java中 所有的一切都必须在类中声明相当简单和一致

### 1.1.2 Java程序的编译与运行

安装好JDK之后，我们可以运行用终端打开，输入以下的命令，以刚才的Hello World为例

``` bash
$ javac ch01/sec01/HelloWorld.java
//javac命令将java源代码编译为中间代码，也称为字节码，并将他们保存到类文件中。

$ java ch01.sec01.HelloWorld
//Java 命令启动虚拟机，虚拟机加载类文件并执行字节码。
```
所以，一旦编译完成，字节码可以在任何Java虚拟机上运行 &quot; 编写一次，到处运行 &quot;

### 1.1.3 方法调用
``` java
System.out.println("Hello World!");
```
System.out是对象。这是PrintStream类的实例。PrintStream类有println、print等方法。

类似这样的方法被称为实例方法，他们运行在对象，或者类的实例上。

调用对象上的实例方法使用点符号
``` java
object.methodName(arguments)
```
另一个例子
```java
"Hello world".length();
```
此处的 "Hello world" 是String类的实例，String 有一个length方法 用于返回字符串对象的长度。

在Java中 你需要构造绝大多数的对象（而不是像此处的System.out和"Hello World"对象直接使用就好，因为他们已经存在）

```java
new 类名 ( 构造函数的参数列表 );
```
举个例子Random类的对象可以产生随机数，你可以使用new操作符构建一个Random对象
```java
new Random();
```
此处的构造函数参数为空

你可以直接在构造对象上调用方法，例如

``` java
new Random().nextInt();
```
新构造的随机数产生器会提供一个整数

如果想要在一个对象上调用多个方法，可以先将该对象用变量存下来，例如接下来我们打印两个随机数：

```java
Random generator = new Random();

System.out.println(generator.nextInt());

System.out.println(generator.nextInt());
```
注意 Random类声明在java.util包中。要在程序中使用该类，需要添加import声明，如：

``` java
import java.util.Random;
```