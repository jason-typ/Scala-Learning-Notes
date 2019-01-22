## ProtocolBuf使用

Scala有个插件可以简化ProtocolBuf的使用，GitHub地址[sbt-protoc](https://github.com/thesamet/sbt-protoc)

sbt-protoc是一个插件，可以从`proto`文件直接生成Jar包，不需要安装protoc(当然插件本身肯定是使用protoc来完成的编译)

使用方法，按照READ.me里面的介绍，很简单：

1. 修改`project/protoc.sbt`，添加以下内容：

  ```
  addSbtPlugin("com.thesamet" % "sbt-protoc" % "0.99.15")

  libraryDependencies += "com.thesamet.scalapb" %% "compilerplugin" % "0.7.0"
  ```
  然后sbt import一下。(直接放到`project/plugins.sbt`里面是一样的使用方法)
2. 修改`build.sbt`文件

  如果是想产生ScalaPB，在`build.sbt`文件(project settings里)中添加以下内容：

  ```
  PB.targets in Compile := Seq(
    scalapb.gen() -> (sourceManaged in Compile).value
  )
  ```

  如果是只想产生Java的Jar包，GitHub上也有具体说明
3. 在`src/main/protobuf`文件夹下添加`.proto`文件，如`src/main/protobuf/person.proto`:

  ```
  message Person {
    required string name = 1;
    required int32 id = 2;
    optional string email = 3;
  }
  ```
之后，在命令行执行命令`sbt compile`就完成了编译。编译产生类放在`target/scala-2.xx/src-managed`文件夹下。
