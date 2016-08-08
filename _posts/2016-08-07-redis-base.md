---
layout: post
title: "redis base"
date: 2016-08-07 08:30:29
image: '/assets/img/'
description:
main-class: "redis"
color:
tags:
- redis
- database
categories:
twitter_text:
introduction:
---

## 1. Install & Start

* mac    

~~~bash
$ brew install redis # install redis
$ redis-sever # start redis serve
$ redis-cli # start redis clent
~~~

* linux

~~~bash
$ wget http://labfile.oss.aliyuncs.com/files0422/redis-2.8.9.tar.gz
$ tar -zvxf redis-2.8.9.tar.gz
$ cd redis-2.8.9
$ make
$ make install   # install

~~~

copy execute files to `/usr/local/bin`

~~~bash
$ cp src/redis-server /usr/local/bin/
$ cp src/redis-cli /sur/local/bin/
~~~

start

~~~bash
$ redis-server # start server
$ redis-cli # start client
~~~

## 2. 数据类型（data type)

### 2.1 String

字符串是一种最基本的Redis值类型。Redis字符串是二进制安全的，这意味着一个Redis字符串能包含任意类型的数据，例如： 一张JPEG格式的图片或者一个序列化的Ruby对象。一个字符串类型的值最多能存储512M字节的内容。

```bash
127.0.0.1:6379> set firstkey hello-world # set key: set keyname value
OK
127.0.0.1:6379> get firstkey # get key: get keyname
"hello-world"
```

`set`是update or create的原理，无论如何都会将key更新为其后面的值。   
`nx`, `xx`是两个特殊的参数，以后再说。

redis可以通过`mset`,`mget`实现一次性完成多个key-value的赋值

```bash
127.0.0.1:6379> mset a 10 b 20 c 30
OK
127.0.0.1:6379> get a b
(error) ERR wrong number of arguments for 'get' command
127.0.0.1:6379> mget a b
1) "10"
2) "20"
```

另外还有一些其他的命令，如`incr`, `incrby`, `decr`, `decrby`.

### 2.2 List

redis list是简单的 字符串 列表   

- list头部    左边      `lpush`-在头部插入一个元素
- list尾部    右边      `rpush`-在尾部插入一个元素

```bash
127.0.0.1:6379> lpush firstlist lo loo  # 头部加入两个元素
(integer) 2
127.0.0.1:6379> rpush firstlist ro roo  # 尾部加入两个元素
(integer) 4
127.0.0.1:6379>
```

`lrange` - 查看 list 元素

`0` - 索引 第一个

`-1` - 索引 最后一个

```bash
127.0.0.1:6379> lrange firstlist 0 -1 # first to last
1) "loo"
2) "lo"
3) "ro"
4) "roo"
127.0.0.1:6379> lrange firstlist 0 -3 # first to 倒数第三个
1) "loo"
2) "lo"
127.0.0.1:6379> lrange firstlist 1 -3 # second to 倒数第三个
1) "lo"
```

有`push`就会有`pop`

- lpop 从头部取出一个元素
- rpop 从尾部取出一个元素

```bash
127.0.0.1:6379> lpop firstlist
"loo"
127.0.0.1:6379> rpop firstlist
"roo"
127.0.0.1:6379> lrange firstlist 0 -1
1) "lo"
2) "ro"
```

删除list

```bash
127.0.0.1:6379> del firstlist
(integer) 1
127.0.0.1:6379> lrange firstlist 0 -1
(empty list or set)
```

`del`命令还可以删除string， `del firstkey`

### 2.3 Hash

### 2.4 Set

### 2.5 Sorted Set

