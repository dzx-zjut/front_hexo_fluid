---
title: Cmake学习笔记（六）
date: 2024-01-25 19:19:19
excerpt: list()函数与string()函数的使用
index_img: /img/Cmake学习笔记/（六）/title.png
banner_img: /img/Cmake学习笔记/（六）/title.png
tags: [ Cmake, C++ ]
categories: [ cmake ]
---

### 1.list()函数

list()函数除了用于字符串的拼接外，还有其他的用法

#### 1.1 获得list()的长度

```cmake
list(LENGTH <list> <output variable>)
```

- `LENGTH`:子命令，用于获取list()的长度
- `<list>`:list()的名称
- `<output variable>`:输出变量，用于存储list()的长度

#### 1.2 获得列表指定索引的元素

```cmake
list(GET <list> <element index> [<element index> ...])
```

- `GET`:子命令，用于获取list()中指定索引的元素
- `<list>`:list()的名称
- `<element index>`:元素的索引
    - 索引从0开始
    - 可以指定多个索引，用于获取多个元素
    - 如果索引超出范围，则运行时会报错
    - -1表示获取最后一个元素，-2表示获取倒数第二个元素，以此类推

#### 1.3 在列表末尾插入元素

```cmake
list(APPEND <list> [<element> ...])
```

- `APPEND`:子命令，用于在列表末尾插入元素
- `<list>`:list()的名称
- `<element>`:要插入的元素
    - 可以指定多个元素，用于一次性插入多个元素

#### 1.4 在列表指定位置插入元素

```cmake
list(INSERT <list> <insert-index> [<element> ...])
```

- `INSERT`:子命令，用于在列表指定位置插入元素
- `<list>`:list()的名称
- `<insert-index>`:插入的位置
    - 索引从0开始
    - 如果索引超出范围，则运行时会报错
    - -1表示插入到最后一个元素的后面，-2表示插入到倒数第二个元素的后面，以此类推
- `<element>`:要插入的元素

#### 1.5 将元素插入到列表头部

```cmake
list(PREPEND <list> [<element> ...])
```

- `PREPEND`:子命令，用于将元素插入到列表头部
- `<list>`:list()的名称
- `<element>`:要插入的元素

#### 1.6 删除列表指定位置的元素

```cmake
list(REMOVE_AT <list> <element index> [<element index> ...])
```

- `REMOVE_AT`:子命令，用于删除列表指定位置的元素
- `<list>`:list()的名称
- `<element index>`:要删除的元素的索引

#### 1.7 删除列表中指定的元素

```cmake
list(REMOVE_ITEM <list> <value> [<value> ...])
```

- `REMOVE_ITEM`:子命令，用于删除列表中指定的元素
- `<list>`:list()的名称
- `<value>`:要删除的元素

#### 1.8 删除列表中的重复元素

```cmake
list(REMOVE_DUPLICATES <list>)
```

- `REMOVE_DUPLICATES`:子命令，用于删除列表中的重复元素
- `<list>`:list()的名称

#### 1.9 列表反转

```cmake
list(REVERSE <list>)
```

#### 1.10 列表排序

```cmake
list(SORT <list> [COMPARE <compare>] [CASE <case>] [ORDER <order>])
```

- COMPARE:用于指定比较函数
    - STRING:按照字母顺序排序，是默认的比较函数
    - FILE_BASENAME:如果是一系列路径名，按照文件名排序
    - NATURAL:按照数字顺序排序
    - 如果不指定，则使用默认的比较函数
- CASE:用于指定比较时是否区分大小写
    - INSENSITIVE:不区分大小写
    - SENSITIVE:区分大小写
    - 如果不指定，则默认区分大小写
- ORDER:用于指定排序的顺序
    - ASCENDING:升序
    - DESCENDING:降序
    - 如果不指定，则默认升序

### 2.string()函数

#### 2.1 字符串替换

在**cmake**中，我们可以使用多种方式来替换字符串中的内容。这些替换方法主要可以分为两类：全局替换和单次替换.

##### 2.1.1 全局替换

在CMake中，我们可以使用`string(REPLACE)`来进行全局替换。这个命令会将字符串中所有匹配的子串替换为指定的新子串。

```cmake
string(REPLACE <substring> <replace> <output variable> <input>)
```

- substring: 被替换的字符串
- replace:替换后的字符串
- output variable:存储替换后的结果
- input:要替换的字符串

例如，我们可以这样使用`string(REGEX REPLACE)`:

```cmake
string(REGEX REPLACE "Hello" "Hi" result "Hello, Hello!")
message(${result})
```

结果为：

```powershell
Hi, Hi!
```

#### 2.1.2 单次替换

在cmake中，我们可以使用`string(REGEX REPLACE)`来进行单次替换。这个命令会将字符串中第一个匹配的子串替换为指定的新子串。
例如，我们可以这样使用`string(REGEX REPLACE)`:

```cmake
string(REGEX REPLACE "Hello" "Hi" result "Hello, Hello!")
message(${result})
```

这段代码的结果是：

```powershell
Hi, Hello!
```

以上就是CMake中字符串替换的基本方法。在实际使用中，我们可以根据需要选择合适的替换方法。

#### 2.2 字符串的分割

`string()`函数有多种分割方式，这些分割方法主要可以分为两类：使用`string(REGEX MATCHALL)`和使用`string(STRIP)`。

##### 2.2.1 使用`string(REGEX MATCHALL)`进行分割

在cmake中，我们可以使用`string(REGEX MATCHALL)`来进行字符串分割。这个命令会使用正则表达式来匹配字符串中的所有子串。
例如，我们可以这样使用`string(REGEX MATCHALL)`:

```cmake
string(REGEX MATCHALL "[0-9]+" result "Hello123World456")
message(${result})
```

这段代码会输出`123 456`，因为它将字符串`"Hello123World456"`中的所有数字子串匹配出来

##### 2.3.2 使用`string(STRIP)`进行分割

在cmake中，我们可以使用`string(STRIP)`来进行字符串分割。这个命令会移除字符串两端的空白字符。
例如，我们可以这样使用`string(STRIP)`:

```cmake
string(STRIP result " Hello World ")
message(${result})
```

这段代码会输出`Hello World`，因为它将字符串"` Hello World `"两端的空白字符移除了。

以上就是CMake中字符串分割的基本方法。在实际使用中，我们可以根据需要选择合适的分割方法。

#### 2.3 字符串的替换

在cmake中，我们可以使用`string(REPLACE <substring> <replace> <output variable> <input>)`
函数来替换字符串中的某个子字符串。这在处理测试用例或者其他需要替换的场景中非常有用。
