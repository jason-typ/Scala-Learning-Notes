#Scala集合操作
***

### 1. List的操作

### 拉链操作

1. zip操作接收两个列表，返回由一个对偶组成的列表

  ```scala
  scala> List(1, 2, 3, 4, 5) zip List("a", "b", "c", "d")
  res1: List[(Int, String)] = List((1,a), (2,b), (3,c), (4,d))
  ```
  如果两个列表的长度不同，那么任何没有配对上的元素将被丢弃
2. zipWithIndex

  根据名字也能推断出来，将集合的元素与计数或者说序号对应起来

  ```scala
  scala> List("a", "b", "c", "d").zipWithIndex
  res2: List[(String, Int)] = List((a,0), (b,1), (c,2), (d,3))
  ```


### 2. Set的交集、并集、差集

```Scala
scala> val s1 = Set(1, 2, 3)
s1: scala.collection.immutable.Set[Int] = Set(1, 2, 3)

scala> val s2 = Set(2, 4)
s2: scala.collection.immutable.Set[Int] = Set(2, 4)
```

* 交集

  ```Scala
  scala> s1 & s2
  res7: scala.collection.immutable.Set[Int] = Set(2)

  scala> s1.intersect(s2)
  res8: scala.collection.immutable.Set[Int] = Set(2)
  ```
* 并集

  ```Scala
  scala> s1 | s2
  res5: scala.collection.immutable.Set[Int] = Set(1, 2, 3, 4)

  scala> s1.union(s2)
  res6: scala.collection.immutable.Set[Int] = Set(1, 2, 3, 4)
  ```
* 差集

  ```Scala
  scala> s1 &~ s2
  res9: scala.collection.immutable.Set[Int] = Set(1, 3)

  scala> s1.diff(s2)
  res10: scala.collection.immutable.Set[Int] = Set(1, 3)
  ```

这些方法都是针对Set的，如果是非Set的集合，需要先转化为Set，再执行并、交、差操作。
