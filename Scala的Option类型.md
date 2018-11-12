## Scala的Option类型与Try类型

### Option类型

scala由`Option`类型来表示可选值，即这个值可能存在，也可能不存在。如果存在，则使用子类`Some`包装：`Some(x)`；如果不存在，则使用`None`对象表示。

对于一个具体的函数，完全有可能在某些情况下返回某个值，在另一些情况下不做返回。scala中将这两种情况统一起来，可以返回同一种类型`Option`。

将`Option`的值解开，最常用的是模式匹配：

```
Option[SomeType] option
option match {
	case Some(val) => val
	case _ => "none"
}
```

其中`Some(val)`匹配的有数据返回的情况，`val`的类型为`SomeType`

## Try类型

Scala2.10中引入，使用Try来包裹可能抛出异常的操作，得到Try的子类Success或Failure。简单的使用如：

```
def devide(x: Int, y: Int): Try[Int] = {
	Try(x/y)
}

divide(1, 0).getOrElse(0)
devide(1, 0) match {
	case Success(v) => println(v)
	case Failure(msg) => println("failed: " + msg)
}
```
