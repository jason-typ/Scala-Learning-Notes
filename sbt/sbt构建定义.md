# sbt构建项目

## 构建定义

构建定义是sbt在检查项目和处理构建定义文件之后，形成一个的Project定义，是对项目的描述。描述采用键值对的形式。
构建定义主要由项目目录下的build.sbt文件来描述，并由项目目录下project文件夹下的\*.scala和\*.sbt文件来辅助形成的。

在构建定义中，首先应该指定sbt的版本(非必须，但推荐，因为没有这个文件，sbt-launcher会随机使用一个本地的sbt版本，这样代码的可移植性就不高)。
方法是创建文件`project/build.properties`，在其中指定版本：

```
sbt.version = 1.2.6
```

构建定义有两种风格：

- bare .sbt构建定义
- 多工程 .sbt构建定义

### bare .sbt构建
bare.sbt是简单的形式，一般用于单工程的构建。这种构建定义的形式由一个Setting\[_\]表达式的列表组成，如：

```sbt
name := "scala-learning"

version := "0.1"

scalaVersion := "2.12.6"

libraryDependencies ++= Seq(
  "com.typesafe.scala-logging" %% "scala-logging" % "3.9.0",
  "ch.qos.logback" % "logback-classic" % "1.2.3"
)
```

这是对sbt文件的简单使用。可以看到定义其实都是键值对配置，其中name和version是必须的(打包生成文件时需要)。
libraryDependencies键值对用来指示需要用到的依赖包。

## 多工程 .sbt构建

多工程构建是最新的形式，适用于所有的情况，如本地多工程、工程之间还有依赖关系的构建，推荐使用。
多工程构建最终是形成一个Project定义，它持有一个名为settings的scala表达式列表，描述的键值对就放在这里面。比如在build.sbt中创建一个当前目录的Project工程定义

```sbt
ThisBuild / organization := "com.example"
ThisBuild / scalaVersion := "2.12.4"
ThisBuild / version      := "0.1.0-SNAPSHOT"
lazy val projectName = (project in file("."))
  .settings(
    name := "Hello"
    scalaVersion := "2.12.6
    )
```
上面的配置，指定了本地构建使用的scala的版本，构建产生的版本，以及会增加(或替换)键为name的值为Hello(项目名称)。


1. 构建子项目

  使用多项目 .sbt构建方式来构建子项目会很方便，如：

  ```sbt
  ThisBuild / scalaVersion := "2.12.6"
  ThisBuild / organization := "com.example"

  lazy val hello = (project in file("."))
    .settings(
      name := "Hello",
      libraryDependencies += "com.eed3si9n" %% "gigahorse-okhttp" % "0.3.1",
      libraryDependencies += "org.scalatest" %% "scalatest" % "3.0.5" % Test,
    )

  lazy val helloCore = (project in file("core"))
    .settings(
      name := "Hello Core",
    )
  ```

上面定义了两个构建定义(即两个Project，两个项目)：hello和helloCore。项目的根目录分别是"."和"./core"。
如果想要单独编译子项目，可以在sbt中使用命令`helloCore/compile`。

2. 项目间的依赖

  使用dependsOn来指明某个项目依赖于另一个项目。如：

  ```
  ThisBuild / scalaVersion := "2.12.6"
  ThisBuild / organization := "com.scala-learning"

  val scala_logging = "com.typesafe.scala-logging" %% "scala-logging" % "3.9.0"
  val logback =  "ch.qos.logback" % "logback-classic" % "1.2.3"

  lazy val root = (project in file("."))
    .settings(
      name := "Hello",
      version := "0.1",
      packMain := Map("hello" -> "com.tang.Hello"),
      libraryDependencies ++= Seq(scala_logging, logback)
      )
    .enablePlugins(PackPlugin)
    .dependsOn(core)

  lazy val core = (project in file("core"))
    .settings(
      name := "core"
    )
  ```
这样就指定了项目root依赖于项目core。此时在项目root中要想使用core打包生成的jar包，首先需要先对core完成编译打包，以及发布，
让root项目能够找到这个jar包。在本地，可以使用`publishLocal`命令发布到.ivy2目录下。
当然，手动的打包完成后，把jar包放在lib目录下也是可以的.

3. 命令的广播

在项目的根目录下执行某个命令，如compile，则编译的只会是root项目，而不会编译core项目(原因？)。如果想要这个命令广播到其他(子)项目中，使用aggregrate方法：

```
ThisBuild / scalaVersion := "2.12.6"
ThisBuild / organization := "com.scala-learning"

val scala_logging = "com.typesafe.scala-logging" %% "scala-logging" % "3.9.0"
val logback =  "ch.qos.logback" % "logback-classic" % "1.2.3"

lazy val root = (project in file("."))
  .settings(
    name := "Hello",
    version := "0.1",
    packMain := Map("hello" -> "com.tang.Hello"),
    libraryDependencies ++= Seq(scala_logging, logback)
    )
  .enablePlugins(PackPlugin)
  .dependsOn(core)
  .aggregrate(core)

lazy val core = (project in file("core"))
  .settings(
    name := "core"
  )
```

## sbt文件中的setting
