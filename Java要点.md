```java
/*
 * @Author: hzy
 * @Date: 2022-11-13 11.08
 * @LastEditTime: 2022-11-13 16.28
 * @LastEditors: hzy
 */
```

# 第1章 Java概述

重要特性：

**Write Once Run Anyway**

简单性：相比C++移除指针、运算符重载、多重继承等，垃圾自动回收。

平台无关性：Java引进虚拟机（JVM，Java Virtual Machine）概念。

安全性：没有指针，内存由系统分配。

动态转载：类运行时是动态装载的。

编译过程：

![image-20221113111159344](D:\A文档\笔记\Study\Java要点.assets\image-20221113111159344.png)



JRE： Java Runtime Environment
JDK：Java Development Kit

JRE和JDK的区别：https://www.cnblogs.com/mark5/p/11063047.html

# 第2章 Java语言基础

| 数据类型 | 名称    | 位长 |
| -------- | ------- | :--- |
| 字节型   | byte    | 8    |
| 短整型   | short   | 16   |
| 整型     | int     | 32   |
| 长整型   | long    | 64   |
| 单精度型 | float   | 32   |
| 双精度型 | double  | 64   |
| 字符型   | char    | 16   |
| 布尔型   | boolean | 1    |

数据输入：类`Scanner`，`import java.util.Scanner`;

| 方法          |
| ------------- |
| nextBoolean() |
| nextDouble()  |
| nextFloat()   |
| nextInt()     |
| nextLine()    |
| next()        |

# 第3章 程序流程控制

声明多维数组：int array\[][]，int\[][] array

**和其他高级编程语言不同，Java多维数组不必须是规则矩阵形式。**

```java
int[][] a = new int[2][];
a[0] = new int[2];
a[0][0]=12;
a[0][1]=2;
a[1] = new int[1];
a[1][0]=111;
for(int i=0; i<a.length; i++){
    for(int j=0; j<a[i].length; j++){
        System.out.print(a[i][j] + "\t");
    }
    System.out.println();
}
```

数组复制：`System.arraycopy(src, srcPos, dest, destPos, length);`；

```java
int array[] = new int[10];
for(int i=0; i<10; i++){
    array[i]=i;
}
int array1[] = new int[5];
System.arraycopy(array, 0, array1, 0, 5);
for(int i:array1){
    System.out.print(i + "\t");
}
```

数组排序：`import java.util.Random; import static java.util.Arrays.sort;`；

Random的使用：http://c.biancheng.net/view/867.html

```java
Random r = new Random();
int array[] = new int[10];
for(int i=0; i<10; i++){
array[i]=r.nextInt()%100;
}
sort(array);//Arrays类中的sort()使用的是“经过调优的快速排序法”
for(int i=0; i<array.length; i++){
System.out.print(array[i] + "\t");
}
```

# 第4章.类与对象

用关键字**static修饰**的方法称为类方法，又称为静态方法。不用static修饰的方法称为实例方法，又称为对象方法。 

**类方法只能操作类变量，不能操作实例变量。**

Java的值传递还是引用传递：https://www.jianshu.com/p/457bfc91df79，https://www.javadude.com/articles/passbyvalue.htm

“primitives are passed by value, objects are passed by reference”.



java常用的包：**看看就行了**

| 序号 |      包名      | 功能描述                                                     |
| :--: | :------------: | ------------------------------------------------------------ |
|  1   |   java.lang    | java的核心类库，包含了运行java程序必不可少的系统类，如基本数据类型、基本数学函数、字符串处理、线程、异常处理类等，系统缺省加载这个包。 |
|  2   |    java.io     | java语言的标准输入/输出类库，如基本输入/输出流、文件输入/输出、过滤输入/输出流等等。 |
|  3   |   java.util    | Java的实用工具类库Java.util包。在这个包中，Java提供了一些实用的方法和数据结构。例如，Java提供日期(Data)类、日历(Calendar)类 |
|  4   | java.awt.image | 处理和操纵来自于网上的图片的java工具类库。                   |
|  5   |    java.net    | 实现网络功能的类库有Socket类、ServerSocket类。               |
|  6   |    java.awt    | 构建图形用户界面(GUI)的类库，低级绘图操作Graphics类，图形界面组件和布局管理 |
|  7   | java.awt.event | GUI事件处理包 。                                             |
|  8   |    java.sql    | 实现JDBC的类库。                                             |

import的使用：当定义包后，**在同一个包中的类是默认隐式导入的。**但如果一个类访问来自另一个包中的类，则前者必须显示通过import语句导入后者后才能使用。

基本类型的封装类：**使用范型数据的时候只能使用封装的类。**

| 原始类型 | 封装类    | 静态的常用方法                                               |
| -------- | --------- | ------------------------------------------------------------ |
| boolean  | Boolean   | Boolean.parseBoolean(String str)                             |
| char     | Character | isDigit(char ch);isLetter(char ch);isLowerCase(char ch);toLowerCase(char ch) |
| byte     | Byte      | Byte.parseByte(String str)                                   |
| short    | Short     | Short.parseShort(String str)                                 |
| int      | Integer   | Integer.parseInt(String str)                                 |
| long     | Long      | Long.parseLong(String str)                                   |
| float    | Float     | Float.parseFloat(String str)                                 |
| double   | Double    | Double.parseDouble(String str)                               |

# 第5章面向对象高级特性

实操：https://www.liaoxuefeng.com/wiki/1252599548343744/1255943520012800

下面都是概念

需要掌握的知识点：

- 继承（extends）
- 关键字this和super
- final关键字
- 转型和多态
- 抽象类（abstract class ）和接口（interface）
- 内部类
- 匿名对象和类
- 异常类
- 泛型类

**转型与多态：**

* 上转型：子类对象到父类对象的类型转换，即把创建的子类对象放到父类的对象变量中，该过程是自动完成的，有些像基本类型的自动类型转换。

* 下转型：父类对象到子类对象的转换，必须使用强制转换。

* 多态：是指同一个操作被不同类型对象调用时可能产生不同的行为。

**final关键字：**

final的本义是“最终”，final可以修饰变量、一般方法和类 。

final修饰变量，表示变量一旦获取了初始值就不能被修改；

final修饰方法，表示该方法在派生子类中不能被重写，只能引用；

final修饰类，表示该类不能派生出子类。

**抽象类（abstract class ）和接口（interface）：**

相同点有：

* 都包含抽象方法，这些方法在继承抽象类或实现接口的类中都要具体实现，**如果有一个不实现，该类就是抽象类，还是不能创建实例对象。**

* 抽象类和接口都包含抽象方法，不能用new创建对象实例，两者都可以通过上转型对象或接口回调方式实现多态机制。

不同点有：

* 声明方式不同，接口使用interface关键字，而抽象类使用abstract class关键字。

* 成员变量不同，接口中只能有静态常量，而抽象类中不受限制。

* 成员方法不同，接口中的方法均是public abstract；而抽象类中抽象方法必须加上修饰符abstract。另外，接口中不能定义静态方法，而抽象类可以。

**内部类：**

如果一个类A的内部定义了一个类B，那么类A称为外部类或外嵌类，而类B称为内部类或内嵌类 。

**匿名对象和类：**

new 类名（[实参列表]）{//；类体

 继承“类名”的子类

 }

对象只能使用一次。 

**异常：**

try catch final（**final块一定会执行**）

常用异常类：

空指针异常类：NullPointerException

类型强制转换异常：ClassCastException

数组负下标异常：NegativeArrayException

数组下标越界异常：ArrayIndexOutOfBoundsException

# 第6章.OOP程序设计的基本原则

知识点：了解就行

- 对象的抽象
- 单一职责原则
- 迪米特原则
- 接口隔离原则
- 开放-封闭原则
- 里氏替换原则
- 合成/聚合复用原则

# 第7章 常用类

常用的方法在java.md里面有。

知识点：

- String类
- StringBuffer类
- String类与StringBuffer类比较
- StringTokenizer类
- 日期类（Date、Calendar）

**String类和StringBuffer类：**

String类定义字符串**常量**对象，可以直接定义，也可以用构造方法定义。StringBuffer类对象必须使用构造方法定义。

String的内容**一旦声明不可改变**，如果要改变，改变的是String的引用地址。

用StringBuffer创建的字符串对象**可以修改**。并且所有的修改都直接发生在包含该字符串的缓冲区上。

**日期类（Date、Calendar）：**

Date类可以得到一个完整的日期，但是日期格式不符合平常看到的格式，时间也不能精确到毫秒，要想按照用户自己的格式显示时间可以使用Calender类完成操作。Date类取得的时间是一个正确的时间，但显示格式不符合习惯，可以利用DateFormat类进行格式化。

Calender可以将取得的时间精确到毫秒，但是此类是抽象类，要想使用抽象类，必须依靠对象的多态性，通过子类进行父类得实例化操作，其子类是GregorianCalender，在Calender中提供了部分常量，分别表示日期的各个数字。

# 第8章 图形界面设计

不考，java swing

# 第9章 Java输入和输出

知识点：代码见java.md

- 文件操作：File
- 字节流：InputStream和OutputStream
- 字符流：Reader和Writer

# 第10章多线程

见Java.md：Java并发编程、wait、notify、notifyAll，JDBC