Scala中圆括号与大括号的区别

> 没明白

## 问题

```Scala
al queryString = param.openIds match {
      case Some(openids) =>
        val ids = if (openids.isEmpty) "'0'"
        else {
          openids.map { id =>
            "'" + id + "'"
          }.mkString(",")
        }
```
如上代码，我们知道map接收一个函数，这个函数会执行在List中的每个元素上。但是上面的map代码用的是大括号。

再比如：

```Scala
val tupleList = List[(String, String)]()
val filtered = tupleList.takeWhile( case (s1, s2) => s1 == s2 )
// 编译失败
val filtered = tupleList.takeWhile{ case (s1, s2) => s1 == s2 }
// 成功编译
```

上面的代码中，使用小括号会导致编译失败，而大括号就可以。

所以小括号和大括号在scala中究竟有什么区别？


## 解释

1. scala中，如果某个函数的参数是函数类型，那么scala会自动给参数便捷加上大括号，所以下面两种写法是等价的：

  ```Scala
  list.map(_ * 2)
  list.map({_ * 2})
  ```
2. 某些情况下，函数调用中的小括号可以使用大括号替代。如：

  ```Scala
  list.foldLeft(0)(_ + _)
  list.foldLeft(0) {_ + _}
  list.foldLeft(0)({_ + _})
  list.foldLeft{0}{_ + _}
  ```
3. case语句并不是函数
4. 被“{}”包含的一系列case语句可以被看成是一个函数字面量。它可以被用在任何普通的函数字面量适用的地方，如被当做参数传递

由以上三点可以知道，编译失败的情况是由于传参不符：期望的是函数类型。成功编译的情况是由于省略了小括号，并且大括号整体被当做了函数对待。


## 其他

scala中，小括号与大括号只有一种情况可以互相替换：只有当传递一个参数给某个方法时，并且这个方法的这个参数列表中只包含一个参数。
只有这种情况，才能使用大括号替换小括号。

### 使用小括号可以增加编译检查

如下面这种情况，当method方法接收一个整数时：

```Scala
method {
  1 +
  2
  3
}
method(
  1+
  2
  3
  )
```

大括号版本的可以正常编译，因为大括号中的代码段被当做函数，函数的返回值是一个整数3。而小括号版本的，会有类型检查，期望是一个整数，而提供的
是两个语句。

### 中缀表示法

当使用中缀表示法时，如`List(1, 2, 3) indexOf (2)`，如果参数列表只有一个参数，那么可以省略掉圆括号，
如`List(1, 2, 3) indexOf 2`。

另外要注意，当参数包含多个标记时，如`x + 2`或`a => a % 2 == 0`，要用小括号将边界给划分开。

### Tuples

### 函数与偏函数字面量

scala中，case声明只有两个地方会被用到：`match`和`catch`。

```Scala
object match {
  case pattern if guard => statements
  case pattern => statements
}

try {
  block
} catch {
  case pattern if guard => statements
  case pattern => statements
} finally {
  block
}
```

case语句一定需要使用大括号。

### 表达式与代码块

圆括号可以用来形成子表达式。大括号用来形成代码段(代码段并不是一个函数字面量，所以不要试着把它用作像函数字面量一样)。一个代码段包含多条语句，可能是import语句、声明或表达式。像这样：

```Scala
{
  import stuff._
  statement
  var x = 0   // declaration
  (x % 5) + 1   // expression
}
```
