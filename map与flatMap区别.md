## map与flatMap区别

这两个函数都接收一个函数作为输入，并在Seq中的每个元素上应用这个func。区别在于flatMap比map多了一个扁平化的过程。因此flatMap对func有个要求，其结果必须是可迭代的，而map对输入函数没有任何要求。map函数与flatMap函数的定义如下：

```
def map[B, That](f: A => B)
def flatMap[B, That](f: A => GenTraversableOnce[B])
```

可以用一个简单的例子来解释：

```
scala> val l = List("Hello World", "Hello Coffee", "Hello Rourou")
l: List[String] = List(Hello World, Hello Coffee, Hello Rourou)

scala> l.map(str => str)
res0: List[String] = List(Hello World, Hello Coffee, Hello Rourou)

scala> l.flatMap(str => str)
res1: List[Char] = List(H, e, l, l, o,  , W, o, r, l, d, H, e, l, l, o,  , C, o, f, f, e, e, H, e, l, l, o,  , R, o, u, r, o, u)

scala> l.map(str => str.split(" "))
res2: List[Array[String]] = List(Array(Hello, World), Array(Hello, Coffee), Array(Hello, Rourou))

scala> l.flatMap(str => str.split(" "))
res3: List[String] = List(Hello, World, Hello, Coffee, Hello, Rourou)
```

上面可以看出map和flatMap对相同操作的区别。flatMap可以看做两步操作，map+扁平化。第一步执行结果是个可迭代的对象，第二部把这个可迭代的对象的每个元素取出。
