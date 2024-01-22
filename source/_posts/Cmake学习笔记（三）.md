---
title: Cmake学习笔记（三）
excerpt: 学习使用使用Cmake制作库以及链接库
date: 2024-01-22 19:31:56
index_img: /img/Cmake学习笔记/（三）/title.png
banner_img: /img/Cmake学习笔记/（三）/title.png
tags: [ Cmake, C++ ]
categories: [ cmake ]
---

### 前言

我们在使用C++的时候，经常会使用到一些库，比如说`iostream`，`vector`
等等，这些库都是在编译器中自带的，我们只需要在代码中`#include`
就可以使用了，而有些库时是第三方库，所以需要我们自己调用，而这个过程由及其复杂，所以我们需要使用Cmake来帮助我们来实现这个过程。

### 1.制作静态库与动态库

#### 1.1.制作静态库

在**cmake**中，制作静态库需要使用的命令如下：

```cmake       
add_library(库名 STATIC 源文件1 [源文件2...])
```

在windows中，静态库的名称为`库名.lib`，在linux中，静态库的名称为`lib库名.a`。

所以在一下文件夹中

```powershell
│  CMakeLists.txt
│  main.cpp
│
├─bin
│      demo.exe
│
├─build
├─include
│      head.h
│
└─src
        add.cpp
        div.cpp
        mult.cpp
        sub.cpp
```

我们需要将这个文件的内容输出为一个静态库，我们需要在`CMakeLists.txt`中输入

```cmake
#cmake标准
cmake_minimum_required(VERSION 3.27)
#项目名称
project(demo)

#设置编译器
set(CMAKE_CXX_STANDARD 17)

#添加头文件
include_directories(include)

file(GLOB SRC_LIST ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)

#输出静态库
add_library(demo STATIC ${SRC_LIST} )
``` 

此时在build文件夹中就生成一个libdemo.a文件，这就是我们的静态库。

#### 1.2.制作动态库

在**cmake**中，制作动态库仅仅只需要将`add_library()`函数中的`STATIC`改为`SHARED`即可。

```cmake
add_library(库名 SHARED 源文件1 [源文件2...])
```

在windows中，动态库的名称为`库名.dll`，在linux中，动态库的名称为`lib库名.so`。

#### 1.3修改输出路径

在上面的例子中，我们的静态库和动态库都是输出在build文件夹中，但是我们希望将其输出到我们想要的文件夹中，但cmake中可以使用CMAKE_LIBRARY_OUTPUT_DIRECTORY与CMAKE_ARCHIVE_OUTPUT_DIRECTORY这两个宏来实现。

```cmake
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/lib) #动态库输出路径

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/lib) #静态库输出路径
```

在CMakeLists.txt中加入这两句后运行cmake，就可以将库输出到lib文件夹中了。

{% note secondary %}
注意：
在使用这两个宏时，需要在`add_library()`函数之前使用，否则无法生效。
若没有lib文件夹，系统会自动创建。
{% endnote %}

### 2.链接库

在上面的例子中，我们已经成功的制作了静态库和动态库，但在日常生活中，我们如果使用这些库，所以我们就需要链接这些库。

#### 2.1.链接静态库

在**cmake**中，链接静态库需要使用`target_link_libraries()`函数，函数的第一个参数为需要链接的库，第二个参数为需要链接的库的名称。

```cmake    
target_link_libraries(可执行文件名 库名)
```

在上面的例子中，我们需要链接静态库，所以我们需要在`CMakeLists.txt`中输入

```cmake
#cmake标准
cmake_minimum_required(VERSION 3.27)
#项目名称
project(demo)

#设置编译器
set(CMAKE_CXX_STANDARD 17)


#添加头文件
include_directories(include)

add_executable(demo main.cpp)

#链接静态库
target_link_libraries(demo demo)
```

但此时，发现在编译时会报错:

```powershell
cannot find -lcalc
collect2.exe: error: ld returned 1 exit status
mingw32-make.exe[2]: *** [CMakeFiles\demo.dir\build.make:99: demo.exe] Error 1
mingw32-make.exe[1]: *** [CMakeFiles\Makefile2:82: CMakeFiles/demo.dir/all] Error 2
mingw32-make.exe: *** [Makefile:90: all] Error 2
```

这是因为我们生成的库的目录不在系统的环境变量里，所以编译器不知道我们的静态库在哪里，所以我们需要在`CMakeLists.txt`中加入

```cmake
#设置静态库路径
link_directories(${CMAKE_CURRENT_SOURCE_DIR}/lib)
```

此再在powershell中运行`cmake`,就不会报错了。

#### 2.2.链接动态库

动态库的链接和静态库一样，也是使用`target_link_libraries()`函数，只是函数的第二个参数为动态库的名字。

代码如下：

```cmake
#cmake标准
cmake_minimum_required(VERSION 3.15)
#项目名称
project(demo)

#设置编译器
set(CMAKE_CXX_STANDARD 17)

#添加头文件
include_directories(include)

#链接静态库地址
link_directories(${CMAKE_CURRENT_SOURCE_DIR}/lib)

add_executable(demo main.cpp)

target_link_libraries(demo  calc)
```

{% note secondary %}
注意：
无论是静态库还是动态库，都需要在`target_link_libraries()`函数之前使用`link_directories()`函数，且需要在`add_executable()`
函数之后使用。
{% endnote %}

