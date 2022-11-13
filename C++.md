# 基础复习

### Static、#define、const、mutable、typedef：

代码存储区域：常量区、代码区、静态区（全局区）、堆区、栈区

栈区向下增长，堆区向上增长。**栈由系统管理，没有内存碎片，每个元素之间都是连续的，大小比较小，8k，可以修改系统参数**，堆区存储动态开辟的变量。

还有一个内核空间，但是它不与用户直接交互（内核区）。

<img src="D:\A文档\笔记\C++.assets\image-20221022094243664.png" alt="image-20221022094243664" style="zoom:33%;" />

#### static

修饰局部变量：局部变量的存储区域改变、变为静态区，生命周期改为程序结束才销毁。

修饰全局变量：

修饰成员变量：静态成员变量不属于实体的类对象，要在类外初始化

修饰成员函数：静态函数**属于类不属于类对象** 需要通过类作用域调用 **函数无this指针**(静态成员函数仅能访问静态的数据成员，不能访问非静态的数据成员，也不能访问非静态的成员函数)

#### \#define

带参数的宏定义可以减少函数调用的开销，在运行时只是简单的展开

#### const

在运行期间可以进行类型检查，

#### mutable

mutable为可变的，易变的跟C++中的const是反义词。被mutable修饰的变量(mutable智能用于修饰类的非静态数据成员)**,将永远处于可变的状态**, 即使在一个const函数中

#### typedef

用途：1、为名称复杂的变量创建别名，2、创建与平台无关的变量。

```cpp
// 在vs中，short 2个字节，int 4个，long也是4个，long long 8个
typedef short int16_t;
typedef int int32_t;
typedef long long int64_t;
// 在Linux下，short 2个，int 4 个，long 8个，long long 也是8个
typedef short int16_t;
typedef int int32_t;
typedef long int64_t;
```

所以在程序源码中，只使用这些在头文件中声明的别名。

### CPP指针

#### 空指针

在C和C++中，用0和`NULL`都可以表示空指针

空指针误操作的后果：如果对空指针解引用，程序会崩溃，**如果对空指针使用 delete 运算符，程序会忽略这个操作，不会出现异常，所以，在内存被释放之后也应该，把指针置为空指针**。

**1、为什么空指针会出现访问异常？**

NULL空指针被分配的区域是一个空闲的区域，没有对应的物理空间与之对应，所以对这段空间来说，任何读操作都是非法的，并且**无论何时**都要保证这个区域都没有与之对应的地址（空指针区域）。

**2、C++ 11 的 nullptr**

由于 使用 0 和 `NULL` 表示空指针会出现歧义，C++ 11 推荐使用 `nullptr` ，也就是 `(void *)0`。

**注意：**在 Linux 平台下，要使用`nullptr `编译需要加上 `-std=C++11`参数

#### 野指针

指针只申明，但未初始化，就会出现，`int* p`，此时p就是野指针。

**规避方法：**
1）指针在定义的时候，如果没地方指，就初始化为 `nullptr` 。
2）动态分配的内存被释放后，将其置为 `nullptr` 。
3）数不要返回局部变品的地址。
**注意：**野指针的危害比空指针要大很多，在程序中，如果访问野指针，可能会造成程序的崩溃。是
**可能，不是一定（因为你也不知道它初始化到底指向了哪里）**，程序的表现是不稳定，增加了调试程序的难度。

#### 指针与数组名的区别

指针是一个地址占4个字节，而数组名是一种数据结构，可以通过 `sizeof` 体现出来

```c++
#include<iostream>
using namespace std;
int main(){
  int a[10];
  int *a1;
  cout<<sizeof(a)<<endl;//40
  cout<<sizeof(a1)<<endl;//4
}
```

但是有一种特殊情况，当数组名作为**函数的参数**来传递的时候，他的高贵的数组结构特性已经失去了，成了一个地地道道的只拥有4个字节的平民。

```cpp
#include <iostream>
using namespace std;
void func1(int arr[]){
    cout<<"sizeof(arr): "<<sizeof(arr)<<endl;
    for(int i=0; i<sizeof(arr)/sizeof(int); i++){
        cout<<arr[i]<<endl;
    }
}
void func2(int arr[], int len){//所以一定要加上数组的长度。
    cout<<"sizeof(arr): "<<sizeof(arr)<<endl;
    for(int i=0; i<len; i++){
        cout<<arr[i]<<endl;
    }
}
int main(){
    int arr[]={1, 2, 3, 4, 5};
    cout<<sizeof(arr)<<endl;
    func1(arr);
    func2(arr, 5);
    return 0;
}
//20
//sizeof(arr): 4
//1
//sizeof(arr): 4
//1
//2
//3
//4
//5
```

### malloc的实现原理

当开辟的空间小于128k时，使用`brk()`，只是将指针`_edata`增加；当开辟的空间大于128k的时候，使用`mmap()`，在空闲区找一块地址去分配。

malloc采用的是内存池的管理方式，以减少内存碎片。

https://zhuanlan.zhihu.com/p/311527161

### C++接口

纯虚函数：virtual int gerArea()=0;

```c++
#include <iostream>
using namespace std;
// 基类
class Shape
{
public:
    // 提供接口框架的纯虚函数
    virtual int getArea() = 0;
    void setWidth(int w)
    {
        width = w;
    }
    void setHeight(int h)
    {
        height = h;
    }
protected:
    int width;
    int height;
};

class Rectangle : public Shape{
public:
    int getArea(){
        return (width* height);
    }
};
class Triangle : public Shape
{
public:
    int getArea()
    {
        return (width * height) / 2;
    }
};

int main(void)
{
    // Shape s=new Shape();//纯虚函数无法创建实例
    Rectangle Rect;
    Triangle Tri;

    Rect.setWidth(5);
    Rect.setHeight(7);
    cout<<"Length of Rect:"<<sizeof(Rect)<<endl;
    
    // 输出对象的面积
    cout << "Total Rectangle area: " << Rect.getArea() << endl;

    Tri.setWidth(5);
    Tri.setHeight(7);
    cout<<"Length of Tri:"<<sizeof(Tri)<<endl;
    // 输出对象的面积
    cout << "Total Triangle area: " << Tri.getArea() << endl;

    return 0;
}
```

### 虚函数实现机制

作用：实现多态，动态绑定。

条件：基类是一个纯虚函数，子类覆写基类的方法，概念：虚函数表（vtbl， virtual table），vptr（函数指针）

虚函数表是：存放复写方法的函数指针，类型识别是有编译器在编译器生成的特殊类型信息。每次在子类中，复写一个函数指针就增加一个。

多重继承的**优点**很明显，就是对象可以调用多个基类中的接口
**缺点**：如果派生类所继承的多个基类有相同的基类，而派生类对象需要调用这个祖先类的接口方法，就会容易出现二义性。 

设计原则之一，**开放封闭原则：对扩展开放,对修改关闭**

```c++
#include <iostream>
using namespace std;
class Point{
    int x, y;
public:
    Point(int x=0, int y=0):x(x), y(y){}
    virtual double area(){
        return 0.0;
    }
    double primeter(){
        return 0.0;
    }
};
class Circle:public Point{
    double radius;
public:
    Circle(int x, int y, int r):Point(x, y), radius(r){};
    double area(){
        return 3.14*radius*radius;
    }
    double primeter(){
        return 3.14*2*radius;
    }

};
int main(){
    Point *p;
    cout<<"hahaha"<<endl;
    // cout<<p->area()<<endl;
    Circle c(1, 2, 3);
    p=&c;
    cout<<p->area()<<endl;
    cout<<c.area()<<endl;

}
```

### Effective c++条款37

绝不重新定义继承而来的缺省参数值，原因：**virtual 函数是动态绑定，而缺省参数是静态的。**

### 运算符重载

重载=运算符的详细介绍：https://www.cnblogs.com/zpcdbky/p/5027481.html

```c++
#include <iostream>
using namespace std;
class Interger{
    int a;
public:
    Interger(int a):a(a){}
    int get() const{
        return this->a;
    }
    Interger& operator++(){
        this->a++;
        return *this;
    }
    Interger operator++(int){
        Interger temp= *this;
        this->a++;
        return temp;
    }
	Integer operator+(const Integer& an){
        Integer tmp;
        tmp.a=this.a+an.a;
        return tmp;
    }
    friend ostream& operator<<(ostream& out, const Integer& i){
        out<<"a="<<i.a<<endl;
        return out;
    }
    friend isstream& operator>>(isstream& in, const Integer& i){
        in>>i.a;
        return in;
    }
    Integer operator-{
        int _a=-this.a;
        return Integer(_a);
    }
    bool operator<(const Integer& an){
        if(this.a<an.a){
            return true;
        }
        return false;
    }
    Integer& operator=(const Integer& an){
        this.a=an.a;
        return *this;
    }
    Integer operator()(int a){
        this.a = this.a+a;
        return *this;
    }
    //类成员访问运算符 -> 重载,用于实现"智能指针"的功能
    
};
int main(){
    Interger i(0);
    int a=0;
    cout<<a++<<endl;
    cout<<i.get()<<endl;
    // i++;
    cout<<i.get()<<endl;
    i++++;
    // ++i;
    cout<<i.get()<<endl;
    return 0;
}
```



### 红黑树

**红黑树的特性**:
**（1）每个节点或者是黑色，或者是红色。**
**（2）根节点是黑色。**
**（3）每个叶子节点（NIL）是黑色。 [注意：这里叶子节点，是指为空(NIL或NULL)的叶子节点！]**
**（4）如果一个节点是红色的，则它的子节点必须是黑色的。**
**（5）从一个节点到该节点的子孙节点的所有路径上包含相同数目的黑节点。**

**红黑树的应用**

红黑树的应用比较广泛，主要是用它来存储有序的数据，它的时间复杂度是O(lg2n)，效率非常之高。
**例如，Java集合中的[TreeSet](http://www.cnblogs.com/skywang12345/p/3311268.html)和[TreeMap](http://www.cnblogs.com/skywang12345/p/3310928.html)，C++ STL中的set、map，以及Linux虚拟内存的管理，都是通过红黑树去实现的。**

GitHub项目：有四百多行代码，[红黑树的实现](https://github.com/william-zk/RB_Tree)

### static关键字

总的来说

- （1）修饰变量，static 修饰的静态局部变量只执行初始化一次，延长了局部变量的生命周期，直到程序运行结束以后才释放。
- （2）修饰全局变量：全局变量只能在本文件中访问，不能在其它文件中访问， **extern 外部声明**也不可以。
- （3）修饰一个函数，则这个函数的只能在本文件中调用，不能被其他文件调用。static 修饰的变量存放在全局数据区的静态变量区，包括全局静态变量和局部静态变量，都在全局数据区分配内存。初始化的时候自动初始化为 0。
- （4）不想被释放的时候，可以使用static修饰。比如修饰函数中存放在栈空间的数组。如果不想让这个数组在函数调用结束释放可以使用 static 修饰。
- （5）考虑到数据安全性（当程序想要使用全局变量的时候应该先考虑使用 static）。

### 排序算法

#### 冒泡排序

```c++
template<typename T>
void BubbleSort(T a[], int len){
    for(int i=0; i<len; i++){
        for(int j=0; j<len-i-1; j++){
            if(a[j]>a[j+1])
            {
                swap(a[j], a[j+1]);
            }
        }
    }
}
```

#### 选择排序

```c++
template<typename T>
void SelectSort(T arr[], int len){
    for(int i=0; i<len; i++){
        int min=i;
        for(int j=i+1; j<len; j++){
            if(arr[j]<arr[min]){
                min=j;
            }
        }
        swap(arr[i], arr[min]);
    }
}
```

#### 插入排序

```C++
template<typename T>
void InsertSort(T arr[], int len){
    for(int i=1; i<len; i++){
        int key=arr[i];
        int j=i-1;
        while(j>=0 && key<arr[j]){
            arr[j+1]=arr[j];
            j--;
        }
        arr[j+1]=key;
    }
}
```

#### 希尔排序

```c++
template<typename T>
void ShellSort(T arr[], int len){
    int gap=1;
    while(gap<len/3){
        gap=gap*3+1;
    }
    while(gap>=1){
        for(int i=gap; i<len; i++){
            for(int j=i; j>gap && arr[j]<arr[j-gap]; j-=gap){
                swap(arr[j], arr[j-gap]);
            }
        }
        gap/=3;
    }
}
```

#### 快速排序

```c++
void quickSort(int a[], int l, int r){

    if (l < r) {
        int i,j,x;

        i = l;
        j = r;
        x = a[i];
        while (i < j) {
            while(i < j && a[j] > x)
                j--; // 从右向左找第一个小于x的数
            if(i < j)
                a[i++] = a[j];
            while(i < j && a[i] < x)
                i++; // 从左向右找第一个大于x的数
            if(i < j)
                a[j--] = a[i];
        }
        a[i] = x;//没有理解这一行
        quickSort(a, l, i-1); /* 递归调用 */
        quickSort(a, i+1, r); /* 递归调用 */
    }
}
```

#### 堆排序



```c++
void sift_down(int arr[], int start, int end){
    int parent = start;
    int child = 2*start+1;
    while(child<=end){
        if(child+1<=end && arr[child]<arr[child+1])
            child++;
        if(arr[parent]>arr[child])
            return ;
        else{
            swap(arr[parent], arr[child]);
            //往下调整
            parent=child;
            child=2*parent+1;
        }
    }
}
void heap_sort(int arr[], int len){
    //调整为大根堆，从最后一个父元素开始(len-1-1)/2
    for(int i=(len-1-1)/2; i>=0; i--) sift_down(arr, i, len-1);
    //将堆的根（最大值）掉换到最后一个元素（从小到大排序），在重新调整为大根堆
    for(int i=len-1; i>=0; i--){
        swap(arr[0], arr[i]);
        sift_down(arr, 0, i-1);
    }
}
```

#### 归并排序

一半一半砍开来，砍成只有两个元素之后，归并。

```cpp
template<typename T>
void merge_sort1(T arr[], int len) {
    T* a = arr;
    T* b = new T[len];
    for (int seg = 1; seg < len; seg += seg) {//区间长度
        for (int start = 0; start < len; start += seg + seg) {
            int low = start, mid = min(start + seg, len), high = min(start + seg + seg, len);
            int k = low;
            int start1 = low, end1 = mid;
            int start2 = mid, end2 = high;
            while (start1 < end1 && start2 < end2)
                b[k++] = a[start1] < a[start2] ? a[start1++] : a[start2++];
            while (start1 < end1)
                b[k++] = a[start1++];
            while (start2 < end2)
                b[k++] = a[start2++];
        }
        T* temp = a;
        a = b;
        b = temp;
    }

    if (a != arr) {//这里没有看懂
        for (int i = 0; i < len; i++)
            b[i] = a[i];
        b = a;
    }

    delete[] b;
}
```

## explicit关键字

禁止隐式转换类型，可以在类的构造函数前面加上。

```cpp
#include <iostream>
#include <string>
using namespace std;
class Person {
private:
	string name;
	int age;
public:
	Person() :name("nullptr"), age(0) {}
	Person(int _age) :name("nullptr"), age(_age) {}
    explicit Person(string _name):name(_name), age(0){}//无法进行隐式转换
	void display() {
		cout << "name: " << name << " age:" << age << endl;
	}
};
int main()
{
	Person p = 9;//自动转换数据类型
	p.display();
    string s="qwe";
    Person p1=s;//自动转换数据类型，会报错：error: conversion from 'std::string' {aka 'std::__cxx11::basic_string<char>'} to non-scalar type 'Person' requested
    p1.display();
    
	return 0;
}
```

## 原子操作atomic

https://juejin.cn/post/7086226046931959838

有两个线程，一个要写数据，一个读数据，如果不加锁，可能会造成读写值混乱，使用`std::mutex`程序执行不会导致混乱，**但是每一次循环都要加锁解锁是的程序开销很大。** 为了提高性能，C++11提供了原子类型(`std::atomic<T>`)，它提供了多线程间的原子操作，可以把原子操作理解成一种：**不需要用到互斥量加锁（无锁）技术的多线程并发编程方式。**它定义在`<atomic>`头文件中，原子类型是封装了一个值的类型，它的访问保证不会导致数据的竞争，并且可以用于在不同的线程之间同步内存访问。从效率上来说，原子操作要比互斥量的方式效率要高。

**atomic类型原子操作宣告C++11来到了多线程和并行编程的时代。相对于偏于底层的`pthread`库，C++通过定义原子类型的方式，轻松地化解了互斥访问共享数据的难题。**

atomic的两个方法：load()//读取数据    store()//存储数据  fetch_add(val)//加法  fetch_sub(val)//减法   exchange()

可以用在指针对象上，但是只表示指针是原子类型，指针指向的数据不一定是，atomic<int *> ptr;

## inline关键字

https://baike.baidu.com/item/inline/10566989

在函数声明和定义加上，使之称为内联函数。如果一些简单的函数直接在类中声明并且定义，编译器可以将其优化为内联函数。

注：内联函数可以减少函数的执行时间：原因是它可以在使用时直接进行替换（像宏一样展开），从而避免出现参数压栈、代码生成等操作，但是如果函数体过大，编译器就不会将其作为内联函数，即使加了关键字inline，同时内联函数不能递归。

inline关键字声明的函数依旧是函数，所以会有类型检查，可以消除C语言宏的一些缺点。

## typeid运算符和type_info类

`typeid`获取数据类型的信息，返回值是`type_info`类（头文件是typeinfo）对象的引用。

`type_info`类中有一个name()函数，该函数返回一个类名的字符串，同时重载了==运算符。

`typeid`可以用于多态的场景，在运行阶段判断对象的数据类型。

## auto关键字

auto变量必须在定义时初始化，初始化的时候的右值可以是表达式或者是函数的返回值。

合理用法：

* 代替冗长的变量声明
* 在模板中，代替用于代替声明模板参数的变量
* 用于lambda表达式

```
#include <iostream>
using namespace std;
double func(int a, double b, int c, const char*d, float e, short f){
    cout<<"a="<<a<<" b="<<b<<" c="<<c<<" d="<<d<<" e="<<e<<" f="<<f<<endl;
    return b;
}
int main(){
    //声明函数指针
    double (*pf)(int a, double b, int c, const char*d, float e, short f);
    pf=func;
    pf(1, 2, 3, "123", 4, 5);
    //声明函数指针
    auto pf1=func;//明显更加方便
    pf1(1, 2, 3, "123", 4, 5);
    return 0;
}
```

## 类型转换-static_cast

新增四个关键字用于代替C语言不够安全的类型转换，static_cast、const_cast、reinterpret_cast、dynamic_cast。

## namespace关键字

与Java的Package类似，用于区分两个人之间定义了相同名字的类、变量、函数等。

```C++
#include <iostream>
using namespace std;
namespace A
{
    int a = 100;
    namespace B            //嵌套一个命名空间B
    {
        int a =20;
    }
}

int a = 200;//定义一个全局变量


int main(int argc, char *argv[])
{
    cout <<"A::a ="<< A::a << endl;
    cout <<"A::B::a ="<<A::B::a << endl;
    cout <<"a ="<<a << endl;
    cout <<"::a ="<<::a << endl;

    int a = 30;
    cout <<"a ="<<a << endl;
    cout <<"::a ="<<::a << endl;

    return 0;
}
```

## 右值引用

http://c.biancheng.net/view/7829.html

C++98中

```c++
int num = 10;
int &b = num; //正确
int &c = 10; //错误
```

因为10只能作为右值，所以C++98中的引用又叫做**左值引用**，右值是不可以修改的。

```c++
const int &c=10;//正确
```

所以这样又是正确的。

C++11中为了在移动语义时可以修改右值，使用&&来表示右值引用。

```c++
int &&a=10;//zheng'que
```

## 完美转发

http://c.biancheng.net/view/7868.html

完美转发是指在传参的时候，将自己的参数完美的转发给其内部调用的其他函数。所谓完美指的是不仅可以传值，还可以保证转发参数的**左右值属性不变**。

## gcc/g++ 链接库的编译与链接

https://blog.csdn.net/surgewong/article/details/39236707

 在widows平台下，静态链接库是.lib文件，动态库文件是.dll文件。在linux平台下，静态链接库是.a文件，动态链接库是.so文件。

计算机那些事(5)——链接、静态链接、动态链接：http://chuquan.me/2018/06/03/linking-static-linking-dynamic-linking/

## 回调函数

c语言式子的回调函数：https://www.cnblogs.com/zzw19940404/p/14152151.html，使用的是基本的函数指针

使用std::function：https://en.cppreference.com/w/cpp/utility/functional/function

```c++
#include <iostream>
#include <functional>
using namespace std;
void print(int a){
	cout<<"a:"<<a<<endl;
}
struct Add{
    void print(int q) const{//一定要加上
        cout<<"q: "<<q<<endl;
    }
};
int main(){
	std::function<void(int )> f1 = print;
	f1(100);
    //std::function<void(int)> f2= &Add::print;
    //f2(101);
    const Add add;
    std::function<void(const Add&, int)> f3 = &Add::print;
    f3(add, 103);
	return 0;

}
```

不在Add的print函数后加上`const`：报错

```c++
error: conversion from ‘void (Add::*)(int)’ to non-scalar type ‘std::function<void(const Add&, int)>’ requested
```



