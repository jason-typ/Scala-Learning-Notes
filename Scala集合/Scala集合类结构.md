
## Scala集合类顶层结构

![Scala集合类框架](http://docs.scala-lang.org/resources/images/collections.png)

集合类的顶端是`Traversable`特质，`Iterable`特质扩展了`Traversable`特质，然后分为三大类：`Seq`、`Set`和`Map`。

`Traversable`特质的唯一抽象操作是`foreach`。这个函数会返回集合中的每个元素，并将`f`作用在每个元素上。
  ```Scala
  def foreach[U](f: Elem => U)
  ```

`Iterable`特质的抽象方法是`iterator`，返回一个迭代器。迭代器具有`next`方法，能一次返回一个集合中的元素。

`Traversable`和`Iterator`这两个特质的主要区别在于控制循环的位置：迭代器具有`next`方法，可以在我们需要下一个元素的时候调用；`foreach`方法则是内部控制循环，即是说，集合中所有的元素都会返回，每个元素都会被传入函数`f`，上层无法控制。之所以需要在`Iterator`上层增加`Traversable`，是因为`foreach`方法实现起来很简单，在某些情况下就是需要作用于全部的元素。

继承关系中，位于Iterable之下有三个特质：`Seq`、`Set`和`Map`。这三个特质都实现了`PartialFunction`特质，定义了相应的`apply`和`isDefinedAt`方法。其中`Seq`分为两种实现形式：`IndexedSeq`和`LinearSeq`。底层分别使用数组和链表来实现，以满足不同使用场景下的性能要求。


### trait Traversable
### trait Iterable

除了`iterator`方法外，`Iterable`特质还有两个返回迭代器的接口：`grouped`和`sliding`。这两个迭代器都返回原始集合中的某个子序列。`grouped`按给定数值将原始集合分组，`sliding`返回原始集合的一个滑动窗口，窗口大小由传入参数指定。

```Scala
scala> val l = for (i <- 'a' to 'g') yield i
l: scala.collection.immutable.IndexedSeq[Char] = Vector(a, b, c, d, e, f, g)

scala> val groupRes = l.grouped(3)
groupRes: Iterator[scala.collection.immutable.IndexedSeq[Char]] = non-empty iterator

scala> groupRes.next
res1: scala.collection.immutable.IndexedSeq[Char] = Vector(a, b, c)

scala> groupRes.next
res2: scala.collection.immutable.IndexedSeq[Char] = Vector(d, e, f)

scala> groupRes.next
res3: scala.collection.immutable.IndexedSeq[Char] = Vector(g)

scala> val slidingRes = l.sliding(4)
slidingRes: Iterator[scala.collection.immutable.IndexedSeq[Char]] = non-empty iterator

scala> slidingRes.next
res4: scala.collection.immutable.IndexedSeq[Char] = Vector(a, b, c, d)

scala> slidingRes.next
res5: scala.collection.immutable.IndexedSeq[Char] = Vector(b, c, d, e)

scala> slidingRes.next
res6: scala.collection.immutable.IndexedSeq[Char] = Vector(c, d, e, f)

scala> slidingRes.next
res7: scala.collection.immutable.IndexedSeq[Char] = Vector(d, e, f, g)
```

**Traversable特质的其他操作**

- 子集合
  - xs.takeRight(n) 由xs的后n个元素组成的集合(如果没有定义顺序，就是任意的n个元素)
  - xs.dropRight(n) 集合除去takeRight(n)以外的部分
- 拉链
  - xs.zip(ys) 由xs和ys对应元素的对偶组成的Iterable
  - xs.zipAll(ys, x, y) 由xs和ys对应元素的对偶组成的Iterable。zip函数会丢弃较长的集合的元素，zipAll会使用默认的元素(x或y)，分别填充较短的那个集合
  - xs.zipWithIndex 由xs中的元素及其下表的对偶组成的Iterable
- 比较
  - xs.sameElements(ys) 测试是否xs和ys包含相同顺序的相同元素

### trait Seq
Seq特质表示序列：一种有长度且元素都由固定的从0开始的下标位置的迭代。Seq特质有两个子特质：`LinearSeq`和`IndexedSeq`。这两个特质并没有添加任何新的操作，不过各自拥有不同的性能特征，类似于数组与链表的区别。

**Seq特质的操作**

### trait Set
Set(集)是没有重复元素的Iterable。

**Set特质的操作**

### trait Map
Map是由键值对组成的Iterable，可以快速的实现查找。


## 可变集合与不可变集合
Scala中集合特质的具体实现类分为两种：可变集合和不可变集合。默认得到的是一个不可变集合对象，使用不可变集合时，需要显式的引入。

<https://blog.csdn.net/adsadadaddadasda/article/details/80801359>
