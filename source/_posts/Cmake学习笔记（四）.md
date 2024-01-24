---
title: Cmake学习笔记（四）
date: 2024-01-24 16:30:51
excerpt: cmake中message()函数的使用
index_img: /img/Cmake学习笔记/（四）/title.png
banner_img: /img/Cmake学习笔记/（四）/title.png
tags: [ Cmake, C++ ]
categories: [ cmake ]
---

### message()函数

在**cmake**中，我们可以使用`message()`函数来为用户打印一条日志信息，使用方法如下：

```cmake
message([<mode>] "message to display" ...)
```

message()
函数的参数有两个，第一个参数是这条信息的重要程度，是可选的，第二个参数是我们打算打印的内容，是必选的。当我们使用`message()`
函数时，正**cmake**生成**Makefile**的过程中，会将这条信息打印出来。比如下面这个例子：

```cmake
#cmake标准
cmake_minimum_required(VERSION 3.15)
#项目名称
project(demo)

#设置编译器
set(CMAKE_CXX_STANDARD 17)

message("hello world")

#添加头文件
include_directories(include)

SET(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_SOURCE_DIR}/bin)

aux_source_directory( ${CMAKE_SOURCE_DIR}/src SRC_LIST)

add_executable(demo ${SRC_LIST} main.cpp)

```

在**cmake**生成**Makefile**的过程中，`powershell`中,在的13行打印出`hello world`这条信息。
```poweshell
-- The C compiler identification is GNU 8.1.0
-- The CXX compiler identification is GNU 8.1.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: D:/codeproject/mingw64/bin/gcc.exe - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: D:/codeproject/mingw64/bin/c++.exe - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
hello world
-- Configuring done (2.0s)
-- Generating done (0.0s)
-- Build files have been written to: D:/Code/demo/build
```

### message()函数的重要程度

在`message()`函数中，我们可以设置这条信息的重要程度，这个重要程度常用的有六个等级，分别是：

-（无）：重要消息
- `STATUS`：非重要消息
- `WARNING`：cmake警告，但生成过程继续
- `AUTHOR_WARNING`：cmake警告(dev)，但生成过程继续
- `SEND_ERROR`：cmake错误，生成过程被跳过
- `FATAL_ERROR`：cmake错误，终止所有的处理过程

比如下面这个例子：

```cmake
#cmake标准
cmake_minimum_required(VERSION 3.15)
#项目名称
project(demo)

#设置编译器
set(CMAKE_CXX_STANDARD 17)


#添加头文件
include_directories(include)

SET(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_SOURCE_DIR}/bin)

aux_source_directory( ${CMAKE_SOURCE_DIR}/src SRC_LIST)

message(WARNING "warning")

message(AUTHOR_WARNING "author_warning")

message(SEND_ERROR  "send_error")

message(FATAL_ERROR "fatal_error")

message(STATUS "status")

add_executable(demo ${SRC_LIST} main.cpp)
```

在运行**cmake**后，`powershell`中输出如下：
```powershell
CMake Warning at CMakeLists.txt:17 (message):
  warning


CMake Warning (dev) at CMakeLists.txt:19 (message):
  author_warning
This warning is for project developers.  Use -Wno-dev to suppress it.

CMake Error at CMakeLists.txt:21 (message):
  send_error


CMake Error at CMakeLists.txt:23 (message):
  fatal_error
```
{% note info %}
在这个程序中，只有在运行`message(FATAL_ERROR "fatal_error")`后，程序才会直接停止运行。
{% endnote %}