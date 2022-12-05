## CMake

##### 参考：https://www.hahack.com/codes/cmake/#USE-MYMATH-%E4%B8%BA-ON

重点编写CMake.txt，保存在与源文件.c、.cpp在同一级目录下，

```
# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

# 项目信息
project (Demo1)

# 指定生成目标
add_executable(Demo main.cc)
```

CMakeLists.txt 的语法比较简单，由**命令、注释和空格**组成，其中命令是不区分大小写的。符号 `#` 后面的内容被认为是注释。命令由命令名称、小括号和参数组成，参数之间使用空格进行间隔。

对于上面的 CMakeLists.txt 文件，依次出现了几个命令：

1. `cmake_minimum_required`：指定运行此配置文件所需的 CMake 的最低版本；
2. `project`：参数值是 `Demo1`，该命令表示**项目的名称**是 `Demo1` 。
3. `add_executable`： 将名为 main.cc的源文件**编译**成一个名称为 Demo 的可执行文件。

## 编译项目

之后，在当前目录执行 `cmake .` ，得到 Makefile 后再使用 `make` 命令编译得到 Demo1 可执行文件。

## 语法特性介绍

基本语法格式：指令(参数1 参数2)

* 参数之间使用**空格或者分号**。

* 指令是大小写无关的，参数和变量是大小写相关的。

* 变量使用${}取值，但是再if语句中是直接使用变量名字

## 重要指令

* cmake_mininmum_required(VERSION 2.8.3)

* project - 工程名字 

    ```cmake
    project(HELLOWORLD)
    ```

    

* set - 显式定义的变量 

    ```cmake
    set(SRC sayHello.cpp hello.cpp)
    ```

    

* include_directories -- 添加头文件搜索路径，相当于g++的-l指令

    ```cmake
    include_directories(/usr/include/... )
    ```

    `

*  link_directories -- 向工程添加多个特定的库文件搜索路径，相当于指定g++的-L参数，

    ```cmake
    link_directories(/usr/lib/...  ./lib)
    ```

    

* add_library -- 生成库文件，

    ```cmake
    # 生成libhello.so的共享库(动态库)文件 STATIC是静态库
    add_library(hello SHARED ${SRC})
    ```

    

* add_compile_options -- 添加编译参数

    ```cmake
    # 编译添加参数
    add_compile_options(-wall -std=c++11 -o2)
    ```

    

* add_executable -- 生成可执行文件

    ```cmake
    add_executable(main main.cpp)
    ```

* target_link_libraries -- 为target添加需要链接的共享库
