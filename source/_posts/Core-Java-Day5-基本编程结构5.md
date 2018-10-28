---
title: Core Java Day5--基本编程结构5(数组与数组列表、方法)
date: 2018-10-28 21:52:45
tags: [《Java核心技术》,学习笔记]
categories: [学习笔记,Java]
---
## 1.8 数组与数组列表
数组是收集相同类型多个元素的基本编程结构。

Java内置了数组类型，并且也提供根据需求随时增长或缩减的ArrayList类。

ArrayList是集合框架中的一部分，在这本书的后面会有讲到

### 使用数组
对所有的类型都有相应的数组类型，例如`int[]`,`String[]`.

和Ｃ语言的数组不太相同，java中的声明方式：
``` java
//数组变量的声明
String[] names;
//变量的初始化，我们用一个新的数组去初始化它。因此我们用到new 操作符
names = new String[100];
//声明和初始化可以放在一起
String[] names　= new String[100];
//现在names引用一个有100个元素的数组了，合法下标范围0~99
```
使用C语言中的语法来声明数组变量(int nums[])也是合法的，但不够清晰，Java中一般不用这种方式。

- 如果下标越界，会抛出`ArrayIndexOutOfBoundException`异常

可以用 数组名.length 获得数组的长度(个数，如上面的100)
``` java
//用空字符串填充数组
for(int i = 0;i<names.length;i++){
    names[i]="";
}
```

### 构造数组
当你使用操作符new构造数组时，会用默认值填充数组
- 数字类型 (包括char类型)的数组用0填充
- Boolean类型的数组用false填充
- 对象数组用空引用填充

警告：任何时候，当构造对象数组时，需要使用对象填充数组。考虑这种声明
``` java
BigInteger[] numbers = new BigInteger[100];
//此刻 你还没有任何BigInteger 仅是一个100个空引用的数组。
//你需要用BigInteger对象的引用代替他们
```

你可以写个循环用值填充数组，你也可以将他们列在括号中
``` java
int[] primes = {2,3,5,7,11,13}
//不用使用new操作符，不用指定数组长度
//如果你不想给数组名称，则可以使用类似的语法，给已存在的数组变量赋值
primes = new int[]{2,3,5,7,11,13}
```

注意，允许数组长度为0，但长度为0的数组与null不同，与之前的空字符串与null不同类似。

### 数组列表
构造数组时，你需要知道数组的长度。一旦构造后，数组的长度不能改变。

解决这种问题的方法就是使用java,util包中的ArrayList类。ArrayList对象内部管理数组。

当数组太小或者不够使用时，自动创建好另外一个内部数组，并且将元素移动到该数组。这个过程对使用数组列表的程序员是不可见。

数组和数组列表的语法完全不同，在数组中使用特殊的语法————[]操作符访问元素，使用Type[]语法获取数组类型，使用new Type[n]语法构造数组。相比之下，数组列表类，你可以使用构造实例和调用的语法。

ArraryList类是泛型类，这是一种带有类型参数的类。在这本书的后面会讲到。

使用泛型类语法，在尖括号中指定类型，声明一个数组列表变量：
``` java
ArrayList<String> friends;
//上面只是声明了变量。你现在需要构建数组列表：
friends = new ArrayList<>;
    //或者new ArrayList<String>()
```
注意空的<> ，编译器会从变量的声明推断出类型参数（被称为钻石语法）

该调用中没有构造参数，但依然必须在末尾提供()。
结果为一个大小为0的数组列表。

``` java
//可以通过add方法给末尾添加元素。
friends.add("Peter");
friends.add("Mino");

//可以在列表的任何位置添加或删除元素
freinds.remove(1);
friends.add(0,"Paul")

//需要使用方法调用访问元素，get方法读取元素，set方法将元素
//用另外一个代替：
String first = friends.get(0);
friends.set(1,"Mary");

//size方法返回列表当前的大小。
//可以使用下面的循环遍历数组列表中的所有元素。
for (int i=0;i<friends.size();i++){
    System.out.println(friends.get(i));
}

```

### 基本类型包装类
！你不能使用基本类型作为类型参数，例如ArrayList<int>是非法的。你必须使用包装类。

每一种基本类型，都有对应的包装类，Integer、Byte、Short、Long、Character、Float、Double和Boolean。

所以刚才那种要收集整数的数组列表可以写成`ArrayList<Integer>`.

``` java
ArrayList<Integer> numbers = new ArrayList<>();
numbers.add(42);
int first = numbers.get(0);
```
基本类型和他们对应的包装类之间的转换是自动进行的。
- 在调用add方法时，一个值为42的Integer类对象在一种被称为自动装箱的过程中自动构造了。
- 在代码的最后一行，get调用返回一个Integer对象，在变量赋值之前自动拆箱，内部产生一个int值

像字符串一样，比较两个包装类是否相等时不可使用==和!= 记得调用包装类的equals方法

### 增强for循环
你可能想经常遍历数组的所有元素，例如：
``` java
int sum = 0;
for(int i = 0;i<numbers.length;i++){
    sum += numbers[i];
}

//还有种更普遍的方式，成为增强循环

int sum = 0;
for (int n :numbers){
    sum +=n;
}
//变量n被复制为每次的循环项
```

### 数组与数组列表的复制
你可以将数组变量复制到另外一个数组变量，但是那样两个变量引用同一个数组（共享的数组）,可以理解为数组变量保存的是一个地址，例如：
``` java
int[] numbers = primes;
numbers[5] = 42;
//数组列表引用的也是同样的工作方式
ArrayList<String> people = friends;
people.set(0,"Merry");
//于是现在friends中的第一个元素也是Merry
```

如果你不想要这种共享数组，则需要复制数组，可以使用静态方法Array.copyOf(被复制的数组，被复制的数组长度)。
``` java
int[] copiedPrimes = Arrays.copyOf(primes,primes.length);
//该方法会构造一个新数组，并将原数组的元素复制进去

//对于数组列表，可直接从已存在的数组列表构造一个新的数组列表
ArrayList<String> copiedFriends = new ArrayList<>(friends);
```
- 构造函数也可以将数组复制到数组列表，首先使用Arrays.asList方法，将数组包装到列表中。
``` java
ArrayList<String> copiedFriends = new ArrayList<>(Arrays.asList(names));
```
- Arrays.asList可以接受一个数组或任意数量的参数。
- 使用任意变量的参数时可以将它当做数组初始化语法的一种替代
``` java
Arrays.asList(names);
Arrays.asList("Mino","Paul","Mary");
```

也可以将数组列表复制到数组中，必须提供正确类型的数组
``` java
String[] names = friends.toArray(new String[0]);
```

没有一种容易的方式可以完成基本类型数组和对应的包装类数组列表之间转换。例如：要在`int[]`和`ArrayList<Integer>`之间转换，你需要循环或者IntStream(这本书之后会讲到)。


### 数组方法
Arrays（数组）类和Collection（集合）类实现了数组和数组列表的常用方法。
``` java
//fill方法，填充数组或数组列表
Arrays.fill(numbers,0);//int[]数组
Collections.fill(friends,"")//ArrayList<String>

//sort 方法 排个序
Arrays.sort(numbers);
Collections.sort(friends);

//Array.toString方法产生一个数组的字符串表示。
//调试时要输出数组该方法特别管用
System.out.pintln(Arrays.toString(nums));
//数组列表对象也有个toString方法
friends.toString();
//PS. println方法输出时会自动调用toString()方法
System.out.println(friends);

//数组列表还有一对有用的方法，数组没有对应的方法
Collections.reverse(names);//反转元素
Collections.shuffle(names);//随机打乱元素
```

### 命令行参数
所有java程序的main方法都有一个字符串数组的参数：
```java
public static void main(String[] args){
    ...
}
```
``` bash
//当程序执行时 在命令行中指定的参数就复制给args
//如果程序在命令行中以如下方式调用
java Greeting hello world
//Greeting是类名 而后面的参数以空格分割开。
//所以args[0]为"hello" args[1]为"world"
```

### 多维数组
Java没有真实的多维数组，对维数组是通过数组的数组来实现的。例如 声明一个对维数组：
``` java
int[][] square = {
    {1,2,3,4},
    {2,3,4,5},
    {3,4,5,6},
    {4,5,6,7}
};
```
从技术上来看 这是一个一维的int[]数组，要访问元素，使用两个中括号。
``` java
int element = square[1][2];
//第一个索引选择行数组square[1]
//第二个索引从该行数组获得元素
```
可以互相换行
``` java
int[] temp = square[0];
square[0]=square[1];
square[1]=temp;
```

如果你不提供初始值，则必须使用new操作符并指定行数和列数
``` java
int[][] square =new int[4][4];//行在前列在后。
```

- 二维数组的实际实现就是，数组的每一行是用数组填充的。并不要求数组的每一行长度相同。
- 遍历二位数组需要使用两个循环，一个行循环，一个列循环。
- 对于二维数组，有方法`Arrays.deepToString(array)`.
- 没有二维的数组列表，但是可以声明ArrayList<ArrayList<Integer>>类型的变量并自己构建行。


## 1.9 功能分解
如果你的main方法太长了，则可以将程序分解到多个类中。对于一个简单的程序，可以将程序代码分解到同一个类的方法中。这些方法都必须以static修饰符声明，如同main一样。本书的第二章会介绍。

### 静态方法的声明与调用
声明方法时，在方法头提供返回值类型、方法名称以及参数名称。然后在方法体提供方法的实现。return返回结果。

例如如下的例子：
``` java
public static double average(double x,double y){
    double sum = x + y;
    return sum/2;
}

//该方法与main在同一个类中，可以直接调用
//java中与出现的前后顺序没有关系
public static void main(String[] args){
    double a = ...;
    double b = ...;
    double average = average(a,b);
    ...
}
```

### 数组参数与返回值
可以将数组传递给方法，方法只是收到数组的引用，通过引用可以修改原数组。

例如下面这个例子交换数组的两个元素
``` java
public static void swap(int[] values,int i,int j){
    int temp = values[i];
    values[i] = values[j];
    values[j] = temp
}
//此方法直接对数组进行修改 不需要返回值
```

方法可以返回数组。下面这个方法返回一个数组，返回的数组是生产的一个新数组。

``` java
public static int[] firstLast(int[] values){
    if (values.length == 0) return new int[0];
    else return new int[]{values[0],values[values.length-1]}
}
```

### 可变参数
如printf这样的方法，允许调用者提供数量可变的参数
例如
``` java
System.out.printf("%d", n);
//与
System.out.printf("%d %s",n,"Hello");
```
虽然他们调用同样的方法，但是参数个数不同。

我们定义一个average方法，在类型之后加上`...`声明变长参数，这样我们就可以以任意参数长度调用average。
``` java
public static double average(double... values)
```
参数实际上是double类型的数组。当方法被调用时，创建了一个数组并且以参数填充该数组。在方法体中，可以像数组一样使用。
``` java
public static double average(double... values){
    double sum = 0;
    for (double v : values) sum += v;
    return values.length == 0 ? 0 :sum / values.length;
}
```

- 注意 可变参数必须是方法的最后一个参数，例如
``` java
public static double max(double first,double... rest)
```