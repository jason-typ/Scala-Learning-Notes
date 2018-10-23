## scala.concurrent.duration.Duration

Duration定义：

`sealed abstract class Duration extends Serializable with Ordered[Duration]`

Duration不是用来表示一般性的时间的，它是专用于`scala.current`包而做了优化的。

Duration可以分为两种：

* 有限的时间: `FiniteDuration`
* 有限或无限的时间: `Duration`

包中还提供了`Duration`到Int以及Long、Double的隐式转化
