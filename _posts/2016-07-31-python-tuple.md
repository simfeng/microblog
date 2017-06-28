---
layout: post
title: "python tuple"
date: 2016-07-31 14:01:50
description:
categories:
- python
- tuple

---

**Tuple** is a type of list which can't be changed.

```
In [9]: tu = (1, 4, 'dd', 0, True, False)

In [10]: type(tu)
Out[10]: tuple
```

if you want define a tuple just with one element, take look at this.

```
In [19]: t = (1)

In [20]: type(t)
Out[20]: int

In [21]: t = (1,) # end with ','

In [22]: type(t)
Out[22]: tuple
```

### something tuple can do

1. 定义tuple与定义list的方式相同，只不过整个元素都用`()`,而不是`[]`
2. Tuple的元素是有序的， 从`0`开始索引， 所以一个非空的tuple的第一个元素是`tu[0]`
3. 支持负数索引(negative number index), 从tuple尾部开始计数， 与list相同
4. 与list一样切片(slice)也可以使用，当分割一个tuple时， 将得到一个新的tuple
5. 可以使用 `in` 来判断tuple中是否存在某元素

```
In [11]: tu
Out[11]: (1, 4, 'dd', 0, True, False)

In [12]: 1 in tu
Out[12]: True

In [13]: 2 in tu
Out[13]: False
```

### something tuple can not do

Tuple没有方法， so

1. no `append` and `extend`, can not add elements
2. no `remove` and `pop`, can not delete elements
3. no `index`, so, can not search element

### advantage

- Tuple is fast than list. 如果你定义了一个不需要增删改，只需要遍历的常量集， tuple是首选
- 可以对不需要修改的数据`写保护`, 使代码更安全。如果非得要改变这些值， 可以使用`list()`函数将tuple变成list，当然原有的tuple不会改变   

```
In [14]: l = list(tu) # tuple to list

In [15]: l
Out[15]: [1, 4, 'dd', 0, True, False]

In [29]: tt = tuple(l) # list to tuple

In [30]: type(tt)
Out[30]: tuple

In [31]: tt
Out[31]: (1, 4, 'dd', 0, True, False)
```

- tuple可以作为dictionary的key(dictionary的key必须是不可变的， tuple恰好符合这一点), 但是， 如果tuple中含有了可变的list元素， 则不能作为dictionary的key.

__list's values in tuple can be changed__

```
In [46]: l
Out[46]: [10, 4, 'dd', 0, True, False]

In [47]: nt = (1, 3, l)

In [48]: nt
Out[48]: (1, 3, [1, 4, 'dd', 0, True, False])

In [49]: nt[2][0]=10 # change list's first value

In [50]: nt
Out[50]: (1, 3, [10, 4, 'dd', 0, True, False])    
```

- tuple可以用来格式化字符串

```
In [1]: s = "I'm %s , and I'm %d years old!"

In [2]: print s % ('Lily', 10)
I'm Lily , and I'm 10 years old!
```
