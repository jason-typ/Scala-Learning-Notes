## Scala用法记录

1. 如果一个方法只接受一个参数，在调用它时，可以不使用英文句点或圆括号

	```
	for (i <- 0 to 2)
	  ...
	```
	
	`to`实际上是接收一个`Int`参数的方法。代码`0 to 2`会被解析为`(0).to(2)`
	
	注，这种方式只有在显式的给出方法调用的目标(主体)对象是才有效，前面的目标对象就是`0`。比如说`println`方法，如果直接写作`println 10`，这样无法通过编译，需要加上目标对象，如`Consule println 10`。
	
2. 操作符

	Scala从技术上讲并没有操作符重载，因为它实际上并没有传统意义上的操作符。Scala中所有的操作都是方法调用。类似`+ - * /`这样的字符可以被用作方法名。比如运行`1 + 2`，实际上还是省略了句点和圆括号：调用Int对象1上名为`+`的方法。所以实际解析出来是`(1).+(2)`。
	
3. Scala中为什么使用圆括号而不是方括号来访问数组？

	首先，Scala中数组不过是类的实例，这一点跟其他Scala类没有本质区别。其次，当用一组圆括号将一个或多个值包起来，并将其应用到(apply)到某个对象时，Scala会将这段代码转换成对这个对象的一个名为apply的方法的调用。比如定义一个数组:
	
	```
	val greetStrings: Array[String] = new Array[String](3)
	greetStrings(0) = "hello"
	greetStrings(1) = ", "
	greetStrings(2) = "world\n"
	println(greetStrings(0))
	```
	上面打印语句中的`greetStrings(i)`会被转换成`greetString.apply(i)`。因此，在Scala中访问一个数组的元素就是一个简单的方法调用，跟其他方法调用一样。
	
	当然，这样的代码仅在对象的类型实际上定义了`apply`方法时才能编译通过。
	
	同理，当我们尝试对通过圆括号应用了一个或多个参数的变量进行赋值时，编译器会将代码转换成对`update`方法的调用，这个`update`方法接收两个参数：圆括号扩起来的值，以及等号右边的对象。如上面的`greetStrings(0) = "Hello"`，会被转换为：`greetStrings.update(0, "Hello")`
	
4. 数组的定义

	数组可以使用上面的方法定义：
	
	```
	val greetStrings: Array[String] = new Array[String](3)
	```
	
	一种更简洁的方式是：
	
	```
	val numNames = Array("zero", "one", "two")
	```
	
	Scala会自动推断出类型信息。上面这种定义方法，实际上是调用了名为`apply`的工厂方法。该方法定义在Array类的伴生对象中（伴生对象就类似于一个单例对象，apply方法类似于java中的静态方法）
	
5. 部分应用函数

	如果函数字面量只是一个接收单个参数的语句，可以不必给出参数名和参数本身，如：
	
	```
	val l = List("One", "Two", "Three")
	l.foreach(s => println(s))
	l.foreach(println)
	```

6. Scala方法的参数全是val而非var

	这意味着，无法在方法内部对方法的入参进行重新赋值，如：
	
	```
	def add(b: Byte): Unit = {
		b = 1					// 无法编译通过，因为b是一个val
	}
	```
	
	在Scala方法中，若没有显式的return语句，则该方法返回的是计算出的最后一个(表达式的)值
	
7. Scala在每个Scala源码文件中都隐式的引入了`java.lang`和`scala`包的成员，以及名为`Predef`的单例对象的所有成员。位于`scala`包下的`Predef`包含了很多有用的方法。比如，当你在scala源码中使用`println`时，实际上调用了`Predef`的`println`(`Predef.println`继而调用`Console.println`，执行具体的操作)。当调用`assert`时，实际调用的是`Predef.assert`。

8. fsc命令

	Scala编译器的一个守护进程。
	
9. App特质

	Scala提供了一个`scala.App`的特质，可以省略`main`方法的定义，使用了`extends App`的单例对象，也可以作为Scala程序的入口。
	
10. 相等的判断

	`==`在Java中根据比较的是基本类型还是引用类型，有所不同。对于基本类型，Java中`==`比较的是值得相等性。对于引用类型，Java中`==`比较的是引用相等性，意思是两个变量指向JVM上的同一个对象。
	
	在Scala中，`==`会调用`equals`方法，一般来讲，`equals`方法比较的都是值。Scala也提供了比较引用相等性的机制，叫做`eq`方法。