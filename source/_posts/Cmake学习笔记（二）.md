---
title: Cmake学习笔记（二）
excerpt: 使用set()函数与aux_source_directory()函数舒服的编译c++代码
date: 2024-01-20 18:34:12
index_img: /img/Cmake学习笔记/（二）/title.png
banner_img: /img/Cmake学习笔记/（二）/title.png
tags: [ Cmake, C++ ]
categories: [ cmake ]
---

### 前言

在这样一个文件夹中

```powershell
    目录: D:\Code\demo
    
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         2024/1/20      9:55             85 add.cpp
-a----         2024/1/20      9:55             99 div.cpp
-a----         2024/1/20      9:55            237 head.h
-a----         2024/1/20      9:56            247 main.cpp
-a----         2024/1/20      9:56             85 mult.cpp
-a----         2024/1/20      9:55             87 sub.cpp
```

我们要编译一个`main.cpp`文件要同时编译`add.cpp div.cpp mult.cpp sub.cpp`多个文件，就需要写一个~~又臭又长~~的命令

```powershell
g++ -o a.exe main.cpp add.cpp sub.cpp mult.cpp div.cpp
```

但我们要编辑的文件更多是，这个命令就不是一个普通程序员之力可以写出来的，
所以，我们就需要**cmake**来帮助我们来实现这个编译过程。

### 1.创建CMakeLists文件

首先在powershell中输入 ~~(其实直接右键 新建一个夹**CMakeLists**的文本文件就信了)~~

```powershell
echo CMakeLists.txt
```

{% note warning %}
**注意**
文件名必须拼写正确，大小写区分
{% endnote %}

在**CMakeLists**中输入

```cmake
#cmake标准
cmake_minimum_required(VERSION 3.27)
#项目名称
project(demo)

#设置编译器
set(CMAKE_CXX_STANDARD 17)
```

确定cmake最低标准、项目名称、编译器标准。

### 2.利用add_executable ()创建可执行文件

add_executable()函数可以创建一个可执行文件，这个函数有两个参数，第一个参数是可执行文件的名称 `name`
，第二个参数是需要编译的文件 `sourse`。

在这里我们需要编译的文件是`main.cpp add.cpp sub.cpp mult.cpp div.cpp`，所以我们在**CMakeLists**中输入

```cmake
add_executable(demo main.cpp add.cpp sub.cpp mult.cpp div.cpp)
```

之后在powershell中输入

```powershell
mkdir build                   #创建一个build文件夹
cd build                      #进入build文件夹
 #生成makefile文件
```

{% note info %}
在这命令中，`cmake -G "MinGW Makefiles" ..`是cmake的命令，去构建一个makefile文件，`-G`
参数是指定生成的makefile文件的类型，这里我们使用的是`MinGW Makefiles`，因为我们使用的是`MinGW`编译器。 `..`是指定*
*CMakeLists**文件的路径，这里我们使用的是`..`因为我们在build文件夹中，而**CMakeLists**
文件在上一级文件夹中，而cmake命令需要在`CMakelists.txt`文件的同级文件夹中执行。
{% endnote %}

在build文件夹中就会出现**makefile**文件。之后在powershell中输入

```powershell
cmake --build .
```

在build文件夹中就会出现可执行文件 demo.exe。

### 3.利用set()函数与aux_source_directory()函数编译多个文件

在上面的例子中，我们使用了`add_executable()`函数来创建可执行文件，但是如果我们要编译的文件更多，那么这个命令就会变得很长，再本质上并没有改变我们的工作
~~甚至要打的内容更多了~~。所以我们就需要一个变量去存储我们要编译的文件，而创建这个变量就需要`set()`函数。

在我们的**CMakeLists**中输入

```cmake
#设置源文件
set(SRC_LIST main.cpp add.cpp sub.cpp mult.cpp div.cpp)
```

在这里我们将`main.cpp add.cpp sub.cpp mult.cpp div.cpp`存储在变量`SRC_LIST`中。

{% note info %}
我们可以使用空格来分隔文件

```cmake
set(SRC_LIST main.cpp add.cpp sub.cpp mult.cpp div.cpp)
```

也可以使用分号`;`来分隔文件

```cmake
set(SRC_LIST main.cpp;add.cpp;sub.cpp;mult.cpp;div.cpp)
```

{% endnote %}

之后我们还要修改`add_executable()`函数中的值，改为

```cmake
add_executable(demo ${SRC_LIST})
```

{% note info %}
我们使用${变量名}来获得变量的值
{% endnote %}

此时`CMakeLists.txt`内容如下

```cmake
#cmake标准
cmake_minimum_required(VERSION 3.27)
#项目名称
project(demo)

#设置编译器
set(CMAKE_CXX_STANDARD 17)

set(SRC_LIST main.cpp;add.cpp;sub.cpp;mult.cpp;div.cpp)

add_executable(demo ${SRC_LIST})
```

### 4.利用aux_source_directory()函数查找文件

使用`set()`后，我们的工作量实际上还是没有变化，因此，我们还需要使用`aux_source_directory()`函数实现查找功能。

`aux_source_directory()`函数需要一个参数，即源文件所在的路径，在这里我们可以使用`CMAKE_SOURCE_DIR`（指向`CMakeLists.txt`
文件所在的路径）这个宏来实现。此时，我们只需要在`CMakeList.txt`文件中写入

```cmake
aux_source_directory( #{CMAKE_SOURCE_DIR} SRC_LIST)
```

此时修改后的`CMakeLists.txt`的内容为

```cmake
#cmake标准
cmake_minimum_required(VERSION 3.27)
#项目名称
project(demo)

#设置编译器
set(CMAKE_CXX_STANDARD 17)

aux_source_directory( #{CMAKE_SOURCE_DIR} SRC_LIST)

add_executable(demo ${SRC_LIST})
```

### 5.一些其它内容

在实际中为使文件夹整洁，我们会对文件进行分类。此时我们的文件夹目录如下

```powershell
D:.
├─bin
├─build
├─include
└─src
```

`src`文件夹存储源文件，`include`文件夹存储头文件，`bin`文件夹存储可执行文件

因此，我们还需要设置头文件路径以及设置输出路径

#### 头文件路径

我们可以使用`include_directories()`函数来设置头文件路径，函数的参数就是头文件所在的路径

```cmake
include_directories(${CMAKE_SOURCE_DIR}/include)
```

#### 输出路径

可执行文件的输出路径由宏`EXECUTABLE_OUTPUT_PATH`决定，所以我们可以使用`SET()`函数来设置输出路径

```cmake
SET(EXECUTABLE_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/bin)
```

### 6.代码

```cmake
#cmake标准
cmake_minimum_required(VERSION 3.27)
#项目名称
project(demo)

#设置编译器
set(CMAKE_CXX_STANDARD 17)

#设置头文件路径
include_directories(${CMAKE_SOURCE_DIR}/include)

#设置源文件
aux_source_directory(${CMAKE_SOURCE_DIR}/src SRC_LIST)

#设置输出路径
SET(EXECUTABLE_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/bin)

add_executable(demo ${SRC_LIST})
```