#Scala集合操作
***

### 1. List的操作

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
