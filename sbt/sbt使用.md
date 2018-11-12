## sbt

### sbt常用命令

- clean，删除target目录下生成的文件
- compile，编译源文件(源文件包括哪些，参考sbt源码组织结构)
- run，编译并运行(如果有多个main方法，需要选择其中一个)
- package，将源文件(包括资源文件，但不包括引用到的(lib目录下的)jar包)编译并打包成一个jar包
- test，编译并运行所有测试
- reload，重新加载构建定义(构建定义由`build.sbt`、`project/*.scala`、`project/*.sbt`共同构成，在修改了这些文件之后都需要reload一下，这个操作在IDEA中会自动询问是否reload)
- publish，编译打包后，将代码发布到远程仓库
- publishLocal，发布到本地仓库(~/.ivy2)

### sbt插件

#### sbt-pack plugin

**插件介绍**
sbt-pack插件会打包一个带有启动脚本的Scala包。命令介绍

- `sbt pack`，在`target/pack`目录下创建一个可发布的包
  - 其中包括一个可执行程序以及所有用到的包(bin目录下)
  - 包括`scala-library.jar`在内的所有依赖的jar包都被(单独)放置在`target/lib`目录下，所以这个可分发的包在另一台机器上运行时，只需要有Java运行环境即可(需要的包都放置在了lib目录下)
  - 另外还有一个Makefile，用来安装这个程序(使用`make install`命令)
- `sbt packArchive`在target目录下创建一个可分发的打包文件
  - 这个命令相当于先执行`sbt pack`，然后将pack目录打包，命名为`{project name}-{version}.tar.gz`放置在`target`目录下

更详细的介绍，参见官方提供的[Readme](https://github.com/xerial/sbt-pack)

**插件使用**

使用方式很简单，首先修改`project/plugins.sbt`文件：

```sbt
addSbtPlugin("org.xerial.sbt" % "sbt-pack" % "version")
// 目前最新的版本是0.11
```

然后修改项目目录下的`build.sbt`文件，增加：

```sbt
enablePlugins(PackPlugin)
```

这样sbt-pack就会自动找到带有main方法的类，完成编译以及打包工作。如果有多个类包含main方法，也可以通过手动指定的方法，在build.sbt文件中增加：

```sbt
packMain := Map("hellome" -> "com.tang.Hello")
// object的名字叫做Hello，打包后的可执行程序叫做hellome
```

**进一步配置**

sbt-pack插件还有完整的配置，没有查看。感兴趣可以去[GitHub](https://github.com/xerial/sbt-pack)查看。
