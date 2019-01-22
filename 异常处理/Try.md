### Try/Success/Failue
对于try catch语法，scala提供了一个工具类scala.util.Try来更优雅的使用。对于可能抛出异常的操作，可以使用Try来包裹它，得到Try的子类Success或Failure。

Try还有一些其他操作，后面学习：
- 类似集合的操作 filter, flatMap, flatten, foreach, map
- get, getOrElse, orElse方法
- toOption可以转化为Option
- recover，recoverWith，transform可以让你优雅地处理Success和Failure的结果


### Option/Some/None

### Either/Left/Right
