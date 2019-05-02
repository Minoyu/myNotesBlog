---
title: Core Java Day4--基本编程结构4(输入与输出、控制流)
date: 2018-10-25 16:48:12
tags: [《Java核心技术》,学习笔记]
categories: [学习笔记,Java]
---
## 1.6输入与输出
### 读取输入
调用`System.out.println`时，输出发送到“标准输出流”，然后在窗口上显示出来。

但从“标准输入流”读取不是那么简单，因为相应的`System.in`对象只有读取单个字节的方法。要想读取字符串和数字，需要构造一个Scanner对象依附到`System.in`

`Scanner`类位于`java.util`包中，使用前要在程序的开始位置`import`.

``` java
import java.util.Scanner;

Scanner in = new Scanner(System.in);
//nextLine方法从输入读取一行
System.out.println("What's your name?");
String name = in.nextLine();

//这里nextLine是合理的，因为名字可能会有姓名分割。
//如果要读取用空格分割的单个单词，使用next
String firstName = in.next();

//整数和浮点数　可以使用
String age = in.nextInt();
String score = in.nextDouble();

//还有一些判断方法
in.hasNextLine();
in.hasNext();
in.hasNextInt();
in.hasNextDouble();
//可以在读取前判断一下，例如
if(in.hasNextInt()){
    int age = in.nextInt();
}

```

#### 在终端读取密码
读取密码时可以使用`Console`类来代替因为`Scanner`是可见的。
``` java
Console terminal = System.console();
String username = terminal.readLIne("User name: ");
char[] passwd = terminal.readPassword("Password:")

```
密码以一组字符串数组的形式返回。与将密码存在字符串中相比更加的安全，因为在完成必要的工作后，数组可以重写而字符串不能更改。

### 格式化输出
- `println`新起一行，所有数字都会显示
- `print`不会新起一行，所有数字都会显示
- `printf`格式输出方法
``` java
System.out.print(1000.0/3.0);
//333.333333333333
System.out.printf("%8.2f",1000.0/3.0);
//浮点数宽度为８（包括小数点），精度为小数点后两位
//□□333.33
```
以%开头的每一个格式说明符都会被相应的参数替代，转换字符说明了要被格式化的值所属的类型

printf的使用方法和Ｃ很相似。
```java
System.out.printf("%s,%d",name,age)
```

![格式化输出的转换字符](/img/core_java_post/day4-1.jpg)

此外，还可以指定一些标记符控制格式化输出的显示形式

![格式化输出的标记符](/img/core_java_post/day4-2.jpg)

例如，为一个浮点数添加分组标记符和正负标记符
```java
System.out.printf("%f",100000.0/3.0);
//+33,333.33
```

你也可以使用`String.format`方法来创建不打印输出的格式化字符串

```java
String message = String.format("%s,%d",name,age)
```

## 1.7控制流
Java的控制流声明与其他语言非常的相似，这里我可能会略过一些相似的内容。

### 分支
像`if`/`else`/`switch`等的分支结构我们已经非常熟悉

需要注意的是不要忘记在想要结束的分支末尾添加break.

在java中，case可以使用以下任何类型的类型值
- char,byte,short或int类型的常量表达式(或者这些类型的包装类)
- 文字串
- 枚举类型值（第四章）

### 循环
`while`/`do while`/`for`

在for语句中如果省略了条件，则默认为总是真。
``` java
for(;;)
//无限循环
```

Java中有一种增强的for循环，使针对数组或集合的for循环的写法更加方便，之后将在数组中讲到。

#### 跳出循环和继续循环
- break 退出循环
- continue 跳到下一次循环

可以在应该退出的声明前加上标签，为break提供标签，例如：
``` java
outer:
while(){
    ...
    while(){
        ...
        if(...) break outer;
        ...
    }
    ...
}
//带标签的break跳转到这里
```
注意：你在声明的顶部加标签，但是break声明跳到尾部。
普通的break只能用来退出循环或switch，但是带标签的break可以将控制转移到任何声明的结束，甚至是一个声明块。
``` java
exit:{
    ...
    if(...) break exit;
    ...
}
//带标签的break跳转到这里
```
continue声明也可以带标签，它可以跳转到标签循环的下一个迭代。

!!!过多的break和continue会让程序变得难以阅读，晕头转向，你完全可以选择没有他们的编程方式

