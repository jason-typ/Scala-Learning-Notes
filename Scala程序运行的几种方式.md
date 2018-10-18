## Scala程序运行的几种方式

> 原文地址：http://www.cnblogs.com/jacksu-tencent/p/4428287.html

用到的HelloWorld实例如下（HelloWorld.scala)

<pre>
object HelloWorld {
	def main(args : Array[String]) {
		println("HelloWorld")
	}
}
</pre>

### 1. scala交互式运行

<pre>
➜  ~ scala
Welcome to Scala 2.12.6 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_181).
Type in expressions for evaluation. Or try :help.

scala> println("HelloWorld")
HelloWorld
</pre>

### 2. 通过Scala直接运行

<pre>
➜  hello ls
HelloWorld.scala
➜  hello scala HelloWorld.scala
HelloWorld
</pre>

### 3. 使用scala编译后，打包运行

<pre>
➜  hello mkdir classes
➜  hello scalac HelloWorld.scala -d classes
➜  hello jar cvf test.jar -C classes ./
added manifest
adding: HelloWorld.class(in = 645) (out= 525)(deflated 18%)
adding: HelloWorld$.class(in = 668) (out= 429)(deflated 35%)
➜  hello ls
HelloWorld.scala classes          test.jar
// 使用scala运行
➜  hello scala -cp test.jar HelloWorld
HelloWorld
// 使用java运行
hello java -cp .:test.jar:/usr/local/share/scala/lib/scala-library.jar HelloWorld
HelloWorld
</pre>

### 4. 使用sbt编译、打包，运行

<pre>
➜  hello ls
// 创建工程目录
➜  hello mkdir -p src/main/scala
➜  hello vim src/main/scala/HelloWorld.scala
// 使用sbt编译、打包
➜  hello sbt
[warn] No sbt.version set in project/build.properties, base directory: /Users/tang/Documents/hello
[info] Set current project to hello (in build file:/Users/tang/Documents/hello/)
[info] sbt server started at local:///Users/tang/.sbt/1.0/server/77033287fed435b30f33/sock
sbt:hello> compile
[info] Updating ...
[info] Done updating.
[info] Compiling 1 Scala source to /Users/tang/Documents/hello/target/scala-2.12/classes ...
[info] Done compiling.
[success] Total time: 5 s, completed Aug 9, 2018 9:49:00 AM
sbt:hello> package
[info] Packaging /Users/tang/Documents/hello/target/scala-2.12/hello_2.12-0.1.0-SNAPSHOT.jar ...
[info] Done packaging.
[success] Total time: 0 s, completed Aug 9, 2018 9:49:02 AM
sbt:hello> exit
[info] shutting down server
➜  hello cp target/scala-2.12/hello_2.12-0.1.0-SNAPSHOT.jar ./test.jar
// 使用scala运行
➜  hello scala -cp test.jar HelloWorld
HelloWorld
// 使用java运行
➜  hello java -cp .:test.jar:/usr/local/share/scala/lib/scala-library.jar HelloWorld
HelloWorld
</pre>