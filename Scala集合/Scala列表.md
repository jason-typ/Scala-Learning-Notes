# Scala列表
***

Scala中的列表的底层实现形式是(单向)链表。这意味着对List在任意位置的访问的时间复杂度是n。另外，List是不可变的，所有对List做出修改的方法，实际上都是新创建的一个List并返回。

### 1. 列表构建
Scala中所有的列表都构建自两个基础的构建单元：`Nil`和`::`(读作“cons”)。Nil表示空列表，`::`
表示在列表前面追加元素。例如`List(1, 2, 3, 4)`创建的列表，其实会被翻译成`1 :: 2 :: 3 :: 4 :: Nil`。

## 2. 列表模式
列表可以使用模式匹配展开，如：

```Scala
scala> val fruit = List("a", "b", "c")
fruit: List[String] = List(a, b, c)

scala> val List(a, b, c) = fruit
a: String = a
b: String = b
c: String = c
```

`List(a, b, c)`匹配长度为3的列表，并将列表的3个元素分别绑定到模式变量a, b和c上。

如果事先不知道列表的长度，可以使用`::`，

```Scala
scala> val a :: b :: rest = List("a", "b", "c", "d")
a: String = a
b: String = b
rest: List[String] = List(c, d)
```

这样匹配的是长度大于等于2的列表。

## 3. 列表的基本操作
### 3.1 `:::`

列表连接操作符，用于连接两个列表

### 3.2 length

获取列表中元素的个数。Scala中列表是用链表实现的，获取长度需要一个个遍历元素，因此这个方法的时间消耗与元素个数成正比。如果要判断列表是否为空，使用`list.isEmpty`，而不是`list.length == 0`

### 3.3 `init`和`last`

列表有基本的操作`head`和`tail`，分别用于获取列表的首个元素和除了首个元素外剩余的部分(注意，这两个方法不能用于空列表)。这两个方法有一组对偶的方法：`last`返回列表的最后一个元素，`init`返回列表中除了最后一个元素以外的剩余部分。同样，这两个方法也不能用于空列表。

同样，由于数据结构形式是链表，找到`last`的时间消耗与元素个数成正比。(`head`和`tail`消耗常数时间)

### 3.4 反转列表reverse

使用`reverse`方法反转列表。跟所有其他操作一样，`reverse`操作会创建一个新的列表。

### 3.5 drop、take和splitAt

`drop`和`take`是对`init`和`take`的一般化：`list take n`返回列表的前n个元素，`list drop n`返回列表中除了前n个元素之外的所有元素。当n大于列表的长度时，这两个方法分别返回列表的全部元素和空列表。

`splitAt`操作会将列表在指定的下标位置切开，返回这两个列表组成的对偶(`Tuple2`)。所以，`list split n`的结果，等价于`(list take n, list drop n)`。不过`splitAt`会避免两次遍历列表。

另外，有了`drop`和`take`方法就不难实现`list.slice(n1, n2)`方法：选取列表中从n1到n2之间的数据(包括n1，不包括n2):`list.take(n2).drop(n1)`

### 3.6 apply方法与下标

与数组相同，List的下标从0到列表的长度减一。同样可以使用`apply`方法或`()`

```Scala
scala> val list = List("a", "b", "c")
list: List[String] = List(a, b, c)

scala> list apply 0
res12: String = a

scala> list(1)
res13: String = b
```

从列表的任意位置选取元素，同样需要遍历链表。其实，`list apply n`的实现，就是`(list drop n).head`

### 3.7 flatten

`flatten`方法接收一个列表的列表作为参数，并将它扁平化，返回单个列表。

### 3.8 zip和unzip

拉链操作zip接收两个列表，并返回两个列表对应位置元素组成的对偶(Tuple2)形成的列表。如：

```Scala
scala> val list = List("a", "b", "c", "d", "e", "f", "g")
list: List[String] = List(a, b, c, d, e, f, g)

scala> val list2 = List(1, 2, 3, 4)
list2: List[Int] = List(1, 2, 3, 4)

scala> list zip list2
res22: List[(String, Int)] = List((a,1), (b,2), (c,3), (d,4))
```

如果两个列表的长度不一致，那么没有匹配的元素将会被丢弃。

一个有用的特例是将列表和它的下标zip起来，使用`zipWithIndex`方法：

```Scala
scala> list.zipWithIndex
res23: List[(String, Int)] = List((a,0), (b,1), (c,2), (d,3), (e,4), (f,5), (g,6))
```

任何元组的列表，也可以使用`unzip`方法转换回由列表组成的元组。

### 3.9 toString和mkString
