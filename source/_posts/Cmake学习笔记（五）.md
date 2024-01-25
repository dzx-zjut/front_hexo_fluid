---
title: Cmake学习笔记（五）
date: 2024-01-24 21:33:45
excerpt: cmake中字符串的基本操作
index_img: /img/Cmake学习笔记/（五）/title.png
banner_img: /img/Cmake学习笔记/（五）/title.png
tags: [ Cmake, C++ ]
categories: [ cmake ]
---

### 字符串操作

字符串作为一种极其常见的数据类型，**cmake**也提供了一些字符串操作的函数，这里主要介绍一下**cmake**中常用字符串的操作。

#### 1.字符串插入

在**cmake*中，由许多函数可以实现字符串的插入操作，这里主要介绍`string()``set()` `list()`这三个函数。

##### 1.1`set()`函数

`set()`函数的使用方法如下：

```cmake
set(<变量名> [<值1> <值2> ... ])
```

`set()`函数的作用是将值1到值n经行拼接，将其存储到变量中，如果变量的值已经存在，则会覆盖原来的值。

##### 1.2`list()`函数

`list()`函数的使用方法如下：

```cmake
list(<APPEND> <变量名> [<值1> <值2> ... ])
```

在`list()`函数中，`APPEND`说明此时list()函数的作用；链接，是将值1给添加到变量的值后面，如果变量有值，这个操作是不会覆盖原来的值的。


{% note info %}
`list()`在底层的实现并非直接的字符串拼接，而是将一个个值以`;`分隔，存储到变量中。
{% endnote %}

##### 1.3`string()`函数

`string()`函数的使用方法如下：

```cmake
string(<APPEND> <变量名> [<值1> <值2> ... ])
```

`string()`函数的用法大致和`list()`相同。

##### 

set()函数 list()函数 string()函数在**cmake**中的使用情况如下：

```cmake

set(STR "hello")
message(${STR})
set(STR "world")
message(${STR})
list(APPEND STR "world")
message(${STR})
string(APPEND STR "world")
message(${STR})
```

在powershell中的输出结果如下：

```powershell
hello
world
worldworld
worldworldworld
```
#### 2.字符串的删除

在**cmake**中，我们可以使用`string()`函数与`list()`函数来删除字符串中的某些字符，具体内容如下

##### 2.1`list()`函数

`list()`函数的使用方法如下：

```cmake
list(REMOVE_ITEM <变量名> <值1> [<值2> ... <值n>])
```
`list()`函数在**REMOVE_ITEM**关键字的作用是在变量中找到并删除值为值1的元素。

以上就是**cmake**中字符串的基本操作，更多的字符串操作可以参考[cmake官方文档](https://cmake.org/cmake/help/latest/manual/cmake-generator-expressions.7.html#manual:cmake-generator-expressions(7))。


