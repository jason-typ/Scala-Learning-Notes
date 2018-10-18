## Scala的Option类型

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