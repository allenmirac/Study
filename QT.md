# QT初识

## 第一个QT程序

https://www.xinbaoku.com/archive/dec4s2cz.html

## 项目的目录结构

在项目名称节点下面，分组管理着项目内的各种源文件，几个文件及分组分别为以下几项：

- Demo.pro 是项目管理文件，包括一些对项目的设置项。
- Headers 分组，该节点下是项目内的所有头文件（.h），图 5 中所示项目有一个头文件 mainwindow.h，是主窗口类的头文件。
- Sources 分组：该节点下是项目内的所有 C++源文件（.cpp），图 5 中所示项目有两个 C++ 源文件，mainwindow.cpp 是主窗口类的实现文件，与 mainwindow.h 文件对应。main.cpp 是主函数文件，也是应用程序的入口。
- Forms 分组：该节点下是项目内的所有界面文件（.ui）。图 5 中所示项目有一个界面文件mainwindow.ui，是主窗口的界面文件。界面文件是文本文件，使用 XML 语言描述界面的组成。

### .pro文件详解

https://www.xinbaoku.com/archive/2AcqsKcv.html

**存储项目的基本配置**

```properties
QT       += core gui
```

在项目中添加适当的类库模块支持，core gui 是 Qt 用于 GUI 设计的类库模块

如果要使用`sql`模块：`QT += sql`；

第 2 行是：

```properties
greaterThan(Qt_MAJOR_VERSION, 4): Qt += widgets
```

这是个条件执行语句，表示当 Qt 主版本大于 4 时，才加入 widgets 模块。

### .ui 文件详解

https://www.xinbaoku.com/archive/8yc8sMF5.html

**窗体界面定义文件，利用XML的语法来配置窗口上面的组件。**

但是在设计界面时，不用管 widget.ui文件是怎么生成的，由QT自己解析。

属性编辑器（Property Editor）：窗口右下方是属性编辑器，是界面设计时最常用到的编辑器。属性编辑器的内容分为两列，分别为属性的名称和属性的值。属性又分为多个组，实际上表示了类的**继承关系**。

![信号与槽编辑器中设计信号与槽的关联](https://www.xinbaoku.com/uploads/allimg/181229/2-1Q229103100196.gif)

**信号与槽编辑器的工具栏**：信号与槽编辑器的工具栏上单击“Add”按钮，在出现的条目中，Sender 选择 btnClose，Signal 选择 clicked()，Receiver 选择窗体 Widget，Slot 选择 close()。这样设置表示当按钮 btnClose 被单击时，**就执行 Widget 的 close() 函数，实现关闭窗口的功能。**

## main主函数及其作用

https://www.xinbaoku.com/archive/B6cBs0F0.html

```c++
#include "widget.h"
#include <QApplication>
int main(int argc, char *argv[])
{
    QApplication a(argc, argv); //定义并创建应用程序
    Widget w; //定义并创建窗口
    w.show(); //显示窗口
    return a.exec(); //应用程序运行
}
```

利用`a.exec()`来开始应用程序的消息循环和事件处理。

## 信号与槽机制

https://www.xinbaoku.com/archive/zbcNsMF8.html

https://www.cnblogs.com/QG-whz/p/4995938.html

**信号与槽机制实际上都是函数，当一个事件触发之后，会发送一个信号，与这个信号连接的槽函数接受这个信号之后，作出反应。**

**没有使用C++的回调函数的原因是C++这种方法不是类型安全的，所以QT采用了信号与槽这种类型安全的方式。**

信号与槽关联是用 QObject::connect() 函数实现的，其基本格式是：

```c++
QObject::connect(sender, SIGNAL(signal()), receiver, SLOT(slot()));
```

connect() 是 QObject 类的一个静态函数，而 QObject 是所有 Qt 类的基类，在实际调用时可以忽略前面的限定符，所以可以直接写为：

```c++
connect(sender, SIGNAL(signal()), receiver, SLOT(slot()));
connect(ui->pushButton, SIGNAL(clicked()), this, SLOT(close()));
```

其中，sender 是发射信号的对象的名称，signal() 是信号名称。信号可以看做是特殊的函数，需要带括号，有参数时还需要指明参数。receiver 是接收信号的对象名称，slot() 是槽函数的名称，需要带括号，有参数时还需要指明参数。

信号与槽的规则：**一对多，多对一**

* 一个信号可以连接多个槽，这个时候连接connect函数里面要指定参数的类型

* * ```C++
        connect(spinNum, SIGNAL(valueChanged(int)), this, SLOT(addFun(int))
    ```

* 多个信号可以连接同一个槽。

* 一个信号可以连接另外一个信号。

* 在使用信号与槽的类中，必须在类的定义中加入宏 Q_OBJECT。

## Qt纯代码设计UI实例分析

https://www.xinbaoku.com/archive/K6cKs9FE.html

使用起来比较繁琐。

## Qt Creator使用技巧

https://www.xinbaoku.com/archive/w9cws6Fx.html

快捷键。

# QTNetwork

## QTcpSocket

https://blog.csdn.net/gongjianbo1992/article/details/107743780

## QUdpSocket

https://hyw-zero.github.io/2019/02/11/Qt%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0%E4%B9%8Bsocket%E9%80%9A%E4%BF%A1/#1-2-UDP%E9%80%9A%E4%BF%A1%E6%B5%81%E7%A8%8B%E5%9B%BE



