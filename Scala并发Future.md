## Future

Future提供了便捷的异步操作的方案。Future用于指代某种尚未就绪的值的对象，通常是某个计算的结果，由另外一个线程来完成。

事件驱动：在某种事件发生时，执行某些代码。Future就完成了这个特性，而不会阻塞在调用点。

Future隐式的处理了两种情况：失败与延时。

使用Future，最简单是使用Future的工厂方法来创建：

```Scala
def apply[T](body: =>T)(implicit @deprecatedName('execctx) executor: ExecutionContext): Future[T] =
    unit.map(_ => body)
```

apply方法中传入的body就是另一个线程待执行的方法，或者说线程体。方法返回一个`Future[T]`。

其余的看不懂，先不管。

1. 对结果进行查询

有两个api可以用于查询Future的状态或结果：`isComplete`和`value`。

调用`isComplete`返回布尔值，在Future的结果(计算结束)可用后，才会返回`true`，否则为`false`。
调用`value`用于获取计算结果，类型是`Option[Try[T]]`
  * Try类型有两个子类型，`Success`和`Failure`
  * 如果计算没有完成，返回`None`
  * 如果计算完成，会返回一个`Some`。可能是`Some(Success(value))`，也有可能是`Some(Failure(value))`。

2. 回调函数的注册

使用`onComplete`方法注册回调，使得在`Future`值计算完成后执行注册的方法。

```Scala
def main(args: Array[String]): Unit = {
   val f = Future {
     Thread.sleep(1000)
     println("Hello")
   }
   f.onComplete({
     case Success(value) => println("success")
     case  Failure(exception) => println("fail")
   })
   Thread.sleep(5000)
 }
```
`onComplete`函数的函数字面量参数两侧的大括号不能省略，原因？
3. 对返回结果进行转换

4. 对结果进行异步转换

得到一个Future结果后，如果需要另一个异步调用，这样结果会嵌套在一个两层Future中。要将结果扁平化，变成一个Future，此时需要flatmap

5. 处理Future列表

如果想要对集合中的每个元素执行异步方法，那么可以使用Future列表。

例如有一个消息列表(msgList: List[String])需要处理：

```Scala
val result = msgList.map{msg =>
  Future{handleMsg(msg)}}
```

此时result的状态为`List[Future[String]]`。对Future列表的处理并不容易，我们希望得到的是结果列表`Future(List[String])`。此时可以使用`sequence`函数来完成类型反转：

```Scala
val resultList = Future.sequence(result)
```

此时我们就得到了一个可用的类型：`Future(List[String])`。
