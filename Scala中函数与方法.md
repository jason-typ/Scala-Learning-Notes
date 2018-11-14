# Scala方法与函数区别

在Scala中，方法与函数有着细微的区别，并非完全一样。

### 定义方式

1. 函数定义

  A Function Type is (roughly) a type of the form (T1, ..., Tn) => U,
  which is a shorthand for the trait FunctionN in the standard library.
  Anonymous Functions and Method Values have function types, and function types
  can be used as part of value, variable and function declarations and definitions.
  In fact, it can be part of a method type.

  ```
  scala> val s2 = (x: Int, y: Int) => {
     | x + y
     | }
  s2: (Int, Int) => Int = $$Lambda$1046/841894382@4aeb0e2b
  ```
  Scala中，函数是一等公民，原因是函数可以像其他数据类型一样，被传递或操作。更具体的原因是，函数是FunctionN的一个实例：

  ```
  scala> s2.isInstanceOf[Function2[Int, Int, Int]]
  res6: Boolean = true
  ```

2. 方法定义(method)

  A Method Type is a non-value type. That means there is no value - no object, no instance - with a method type.
  As mentioned above, a Method Value actually has a Function Type. A method type is a def declaration - everything about a def except its body.
  ```
  scala> def s1(x: Int, y: Int) = {
     | x + y
     | }
  s1: (x: Int, y: Int)Int
  ```

  方法定义是一个def声明，但实际上不包括方法体。加上方法体后，方法的返回值才是一个Function类型。

3. 参数列表

  在Scala中，方法的参数列表可以省略(如果没有的话)，如：

  ```
  scala> def test = {
     | println("hi")
     | }
  test: Unit
  ```
  在上层调用的时候的表现就是可以省略掉括号：

  ```
  scala> test
  hi
  ```

  但是对于函数定义来说，参数列表无法省略，如果为空，那也要写上小括号：

  ```
  scala> val test2 = () => {
     | println("hi")
     | }
  test2: () => Unit = $$Lambda$1218/1354373599@726882da
  ```

  如果把`() =>`都省略了，定义会通过，但返回结果只不过是大括号中的函数体执行的结果，也就不是函数的定义了:

  ```
  scala> val test3 = {
     | println("hi")
     | }
  hi
  test3: Unit = ()

  scala> test3

  scala>
  ```

前面已经说过，函数是一个实例，因此直接打印函数对象，可以看到在内存中的实例：

```
s2: (Int, Int) => Int = $$Lambda$1046/841894382@4aeb0e2b
```
但是方法不是一个对象，因此无法直接打印(见下面)。

### 调用方式

函数是一个对象，直接调用这个函数表示的是这个对象本身，只有后面加上括号，才是调用这个函数：

```
scala> test2
res11: () => Unit = $$Lambda$1218/1354373599@726882da

scala> test2()
hi
```

对于方法而言，调用时加不加括号是根据定义方式不同而不同的，比如上面的test3，在定义时，省略了参数列表，就可以在
调用时，不用加括号。又如上面的s1，调用时必须传入参数列表：

```
scala> s1
<console>:13: error: missing argument list for method s1
Unapplied methods are only converted to functions when a function type is expected.
You can make this conversion explicit by writing `s1 _` or `s1(_,_)` instead of `s1`.
```

直接调用s1，是执行s1这个方法，但是缺少这个方法所需要的参数，因此报`missing argument`的错误。后面又提示了，
方法只有在传入一个需要函数类型的地方的时候，才会被编译器转换为一个函数，其他情况下不会自动转换。

### 方法转换为函数

在需要函数的地方传入一个方法，编译器会自动完成转化。另外，也可以手动转换：

1. 在方法的后面加上一个下划线
```
scala> val s1Function = s1 _
s1Function: (Int, Int) => Int = $$Lambda$1227/119819655@210c4071
```

2. 加上类型

```
scala> val s1Fun2: (Int, Int) => Int = s1
s1Fun2: (Int, Int) => Int = $$Lambda$1247/1721826017@53a1cff1
```

### 嵌套使用

```
scala> def hello = {
     | var t = 3
     | println("t = " + t)
     | () => {
     | t = t + 3
     | println(t)
     | }
     | }
hello: () => Unit

scala> hello
t = 3
res0: () => Unit = $$Lambda$1079/2027549979@23ed382c
```

上面的这段代码，以def定义方法的形式，实际上得到的是个函数(对象)，所以直接调用，不加括号，只能看到第一条打印。
