---
title: Cmake学习笔记（一）
excerpt: Cmake的安装与Cmake命令的基本介绍
date: 2024-01-20 17:29:19
index_img: /img/Cmake学习笔记（一）/title.png
banner_img: /img/Cmake学习笔记（一）/title.png
tags: [Cmake, C++]
categories: [cmake]
---

# Cmake

C/C++的编译过程中，通过命令处理多个文件的编译是及其复杂。如下所示：
```powershell
├─.idea
├─cmake-build-debug
│  ├─.cmake
│  │  └─api
│  │      └─v1
│  │          ├─query
│  │          └─reply
│  ├─archive
│  ├─CMakeFiles
│  │  ├─3.27.8
│  │  │  ├─CompilerIdC
│  │  │  │  └─tmp
│  │  │  └─CompilerIdCXX
│  │  │      └─tmp
│  │  ├─demo.dir
│  │  │  └─src
│  │  └─pkgRedirects
│  └─Testing
│      └─Temporary
├─include
└─src
```

在这个文件夹中有很多文件需要编译，如果使用命令就进行编译，那么就需要输入很多命令，这样就会很麻烦。所以就需要一个工具来进行管理，这个工具就是**cmake**。

### 1.1 Cmake的安装

在[cmake官网](https://cmake.org/download/)下载对应的安装包，然后进行安装即可。

可通过
    
```powershell
cmake --version
```
检测**cmake**是否安装成功。

### 2.1使用cmake编译c++文件

在这个文件夹中，我们首先创建一个**CMakeLists.txt**文件，这个文件就是**cmake**的配置文件，利用这个文件我们就可以进行多文件的管理.

比如在上面这个文件夹中，我们创建一个**CMakeLists.txt**文件，内容如下：

```cmake
#cmake标准
cmake_minimum_required(VERSION 3.27)
#项目名称
project(demo)

#设置编译器
set(CMAKE_CXX_STANDARD 17)

#设置源文件
aux_source_directory(${CMAKE_SOURCE_DIR}/src SRC_LIST)

#设置头文件
include_directories(${CMAKE_SOURCE_DIR}/include)

add_executable(demo ${SRC_LIST} main.cpp)
```

之后在powershell中输入

```powershell
mkdir build
cd build
cmake -G "MinGW Makefiles" ..
```

然后文件夹中就会出现build文件夹，里面就有**makefile**文件。之后在powershell中输入

```powershell
cmake --build .
```
在build文件夹中就会出现可执行文件。

### 2.2cmake 命令

在上面的例子中，我们使用了**cmake**的一些命令，下面就对这些命令进行介绍。

####  cmake_minimum_required

`cmake_minimum_required(VERSION 3.27)`是设置**cmake**的最低版本。

{% note warning %}
**注意**
如果版本过低，而我们的**cmake**文件中使用了高版本的命令，那么就会报错。
{% endnote %}

#### project（）

`project(demo)`是设置项目名称。一般来说，这个名称和文件夹的名称是一样的。

#### set（）

`set(<variable> <value> [CACHE <type> <docstring> [FORCE]])`这个命令相当于一个变量的定义，其中`<variable>`是变量的名称，`<value>`是变量的值，`[CACHE <type> <docstring> [FORCE]]`是可选的，其中`CACHE`是缓存，`<type>`是类型，`<docstring>`是文档字符串，`[FORCE]`是强制。

{% note info %}

在变量赋值中的`<value>`是可以是多个值的，比如`set(SRC_LIST main.cpp test.cpp)`，这样就可以将多个文件赋值给变量`SRC_LIST`，不同的值之间用空格或者 **;** 隔开。

{% endnote %}

#### aux_source_directory（）

`aux_source_directory(<dir> <variable>)`是一个所搜的指令，这个命令是将**dir**目录下的所有源文件名称赋值给变量`<variable>`。

#### include_directories（）

`include_directories([AFTER|BEFORE] [SYSTEM] dir1 [dir2 ...])`是一个包含目录的指令，这个命令是将编译时头文件指向**dir1** [dir2 ...]。

#### add_executable（）

`add_executable(<name> [WIN32] [MACOSX_BUNDLE] [EXCLUDE_FROM_ALL] source1 [source2 ...])`是一个可执行文件的指令，这个命令是将源文件`source`编译成可执行文件`name`。

### cmake的宏

在**cmake**中，有一些宏是可以直接使用的，这些宏可以直接使用，不需要进行定义。在这里，我将介绍我在上面的例子中使用的宏。

#### CMAKE_SOURCE_DIR

`CMAKE_SOURCE_DIR`是**cmake**的源文件目录，这个目录就是**CMakeLists.txt**所在的目录。

#### CMAKE_CURRENT_SOURCE_DIR

`CMAKE_CXX_STANDARD`是编译器的标准，这个标准是**c++**的标准，这里使用的是**c++17**的标准。
