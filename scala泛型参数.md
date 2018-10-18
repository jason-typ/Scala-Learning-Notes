## 泛型参数

### 1. 泛型类

Scala中使用方括号来定义类型参数，如：

```Scala
class Pair[T, S](val first: T, val second: S)
```

以上将定义一个带有两个类型参数T和S的类。在类的定义中，类型参数可以用来定义变量，方法的参数及返回类型。

带有一个或多个类型参数的类是泛型的。Scala会从构造函数中，自动推断出实际类型，如：

```Scala
val p = new Pair("hello", 2)    // 类型为Pair[String, Int]
```

当然也可以自己指定类型：

```Scala
val p = new Pair[String, Int]
```

### 2. 泛型函数

方法和函数也可以带参数类型，如：

```Scala
def getMiddle[T](a: Array[T]) = a(a.length/2)
```

与泛型类一样，类型参数放在方法名之后，使用方括号括起来，并且Scala能自动完成类型推断。

###3. 类型变量界定
#### 3.1 指定上界

上界确定类型参数属于某个类的子类，使用`<:`操作符，如：

```Scala
scala> class Pair[T<: Comparable[T]](val first: T, val second: T) {
     | def smaller = if(first.compareTo(second) < 0) first else second
     | }
```

加上了上界的限定，就使得传入的参数的类型T必须是`Comparable[T]`的子类(实现了`compareTo`方法)，如：

```Scala
scala> val p = new Pair("hello", "world")
p: Pair[String] = Pair@3346e906

scala> p.smaller
res5: String = hello
```

#### 3.2 指定下界

也可以为参数类型指定一个下界，使得传入的参数的类型必须是给定的下界的类型的超类。

#### 3.3 视图界定(隐式转换)

视图界定可以使用隐式转化，使用`<%`操作符。如上面上界的定义时，由于`Int`类型没有实现`Comparable`特质，因此直接使用`Pair(1, 2)`时会出错。但是`RichInt`类实现了`Comparable`特质，并且存在`Int`到`RichInt`的隐式转化函数，此时就可以使用视图界定，使得在类型不匹配时，能够完成隐式转换，如：

```Scala
scala> class MyPair[T<% Comparable[T]](val first: T, val second: T) {
     | def smaller = if (first.compareTo(second) < 0) first else second
     | }
defined class MyPair

scala> val p = new MyPair(1, 2)
p: MyPair[Int] = MyPair@56a6aadb

scala> p.smaller
res7: Int = 1
```

#### 3.4 上下文界定(隐式值)

上下文界定的形式为`T: M`，这要求在可视的范围内，必须要有一个类型为`M[T]`的隐式值，该隐式值可以在类的方法定义中作为隐式参数使用，如：

```Scala
scala> class MyPair[T: Ordering](val first: T, val second: T) {
     | def smaller(implicit ord: Ordering[T]) = {
     | if (ord.compare(first, second) < 0) first else second
     | }
     | }
```

#### 3.5 多重界定

类型参数可以同时有上界和下界：

```Scala
T>: Lower <: Upper
```
类型参数只能由一个上界或一个下界。类型参数可以由多个视图界定或上下文界定(多种隐式转换或多个隐式值)

### 4. 型变

型变包括协变、逆变和不变。`+`和`-`叫做型变注解。

#### 4.1 协变

比如`Student`类是`Person`类的子类，但`MyPair[Student]`与`MyPair[Person]`类型之间并没有什么联系。要想使`MyPair[Student]`成为`MyPair[Person]`的子类，需要在`MyPair`的定义中用到协变：

```Scala
scala> class MyPair[+T](val first: T, val second: T)
```

`+`意味着协变：与T按照同样的方向型变。如果`Student`类是`Person`类的子类，那么`MyPair[Student]`就是`MyPair[Person]`的子类。

#### 4.2 逆变

逆变意味着相反的方向型变：如果`Student`类是`Person`类的子类，那么`MyPair[Person]`就是`MyPair[Student]`的子类。
