---
layout: post
title: "redis base"
date: 2016-08-07 08:30:29
description:
categories:
- redis
- database
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

Redis Hashes是字符串字段和字符串值之间的映射,因此他们是展现对象的完美数据类型。 (例如:一个有名，姓，年龄等等属性的用户):一个带有一些字段的hash仅仅需要一块很小的空间存储,因此你可以存储数以百万计的对象在一个小的Redis实例中。 哈希主要用来表现对象，他们有能力存储很多对象，因此你可以将哈希用于许多其他的任务。

```shell
127.0.0.1:6379> hmset user:1000 username xiaoming birthyear 1999 sid 001
OK
127.0.0.1:6379> hgetall user:1000
1) "username"
2) "xiaoming"
3) "birthyear"
4) "1999"
5) "sid"
6) "001"
127.0.0.1:6379> hget user:1000 birthyear
"1999"
127.0.0.1:6379> hget user:1000 sid
"001"
127.0.0.1:6379> hmget user:1000 sid birthyear
1) "001"
2) "1999"
```

- `hmset`命令设置一个多域的hash表
- `hget`命令获取指定的单域
- `hgetall`命令获取指定key的所有信息
- `hmget`类似`hget`,只是返回一个value数组

同样还有很多函数，这里不展开了。

### 2.4 Set(无序集合)

Redis 集合（Set）是一个无序的字符串集合. 你可以以`O(1)`的时间复杂度 (无论集合中有多少元素时间复杂度都是常量)完成添加，删除，以及测试元素是否存在。 Redis 集合拥有令人满意的不允许包含相同成员的属性。多次添加相同的元素，最终在集合里只会有一个元素。 实际上说这些就是意味着在添加元素的时候无须检测元素是否存在。 一个Redis集合的非常有趣的事情是他支持一些服务端的命令从现有的集合出发去进行集合运算，因此你可以在非常短的时间内进行`合并（unions）`, 求`交集（intersections）`,找出不同的元素（differences of sets）。
(如果之前接触过python的set，他们都是一个意思)

```shell
127.0.0.1:6379> sadd myset 1 2 3
(integer) 3
127.0.0.1:6379> SMEMBERS myset
1) "1"
2) "2"
3) "3"
127.0.0.1:6379> sadd myset 7
(integer) 1
127.0.0.1:6379> SMEMBERS myset
1) "1"
2) "2"
3) "3"
4) "7"
127.0.0.1:6379> SISMEMBER myset 3
(integer) 1
127.0.0.1:6379> SISMEMBER myset 4
(integer) 0
127.0.0.1:6379> SISMEMBER myset 2
(integer) 1
```

- `sadd`命令产生一个无序集合，返回集合的元素的个数。
- `smembers`用于查看集合。
- `sismember`用户查看元素是否存在，存在返回`1`，不存在返回`0`.

### 2.5 Sorted Set(有序集合)

Redis有序集合与普通集合非常相似，是一个没有重复元素的字符串集合。不同之处是__有序集合的每个成员都关联了一个评分，这个评分被用来按照从最低分到最高分的方式排序集合中的成员__。集合的成员是唯一的，但是评分可以是重复了。 使用有序集合你可以以非常快的速度（O(log(N))）添加，删除和更新元素。因为元素是有序的, 所以你也可以很快的根据评分（score）或者次序（position）来获取一个范围的元素。访问有序集合的中间元素也是非常快的,因此你能够使用有序集合作为一个没有重复成员的智能列表。在有序集合中，你可以很快捷的访问一切你需要的东西：有序的元素，快速的存在性测试，快速访问集合的中间元素！ 简而言之使用有序集合你可以做完成许多对性能有极端要求的任务，而那些任务使用其他类型的数据库真的是很难完成的。

```shell
127.0.0.1:6379> zadd hackers 1940 "Alan Kay"
(integer) 0
127.0.0.1:6379> zadd hackers 1957 "Sophie Wilson"
(integer) 1
127.0.0.1:6379> zadd hackers 1953 "Anita Borg"
(integer) 1
127.0.0.1:6379> zadd hackers 1965 "Yukihiro Matsumoto"
(integer) 1
127.0.0.1:6379> zadd hackers 1912 "Alan Turing"
(integer) 1

127.0.0.1:6379> zrange hackers 0 -1
1) "Alan Kay1"
2) "Alan Turing"
3) "Alan Kay"
4) "Anita Borg"
5) "Sophie Wilson"
6) "Yukihiro Matsumoto"
127.0.0.1:6379> zrevrange hackers 0 -1
1) "Yukihiro Matsumoto"
2) "Sophie Wilson"
3) "Anita Borg"
4) "Alan Kay"
5) "Alan Turing"
6) "Alan Kay1"

127.0.0.1:6379> zrange hackers 0 -1 withscores
 1) "Alan Kay1"
 2) "1900"
 3) "Alan Turing"
 4) "1912"
 5) "Alan Kay"
 6) "1940"
 7) "Anita Borg"
 8) "1953"
 9) "Sophie Wilson"
10) "1957"
11) "Yukihiro Matsumoto"
12) "1965"
```

- `zadd`与`sadd`类似，但是在元素之前多了一个数值(float)参数，用于排序。`返回值0`表示集合第一个元素，`返回值1`表示集合最后一个元素。
- `zrange`查看正序集合, `zrevrange`查看反序集合。`0` means first element. `-1` means last elemnt.
- `WITHSCORES` 参数返回排序值。
