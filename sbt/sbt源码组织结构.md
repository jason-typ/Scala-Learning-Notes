## sbt源码组织结构

假设项目的基础目录是`hello`，源码可以直接放在基础目录下，如`hello/hello.scala`。但是这样太乱了，sbt和Maven默认的源目录结构是一样的：

<pre>
HelloWorld  (base directory)
    build.sbt
    .gitignore  (用于忽略文件或目录)

    project/
        build.properties
        xx.sbt (辅助对象和一次性插件等)
        project/
        target

    src/
        main/
            resources/ (files to include in main jar here)
            scala/ (main scala sources)
            java/ (main java sources)
        test/
            resources/ (files to include in test jar here)
            scala/ (test scala sources)
            java/ (test java sources)

    target/ (generated files, like compiled classes, packaged jars, managed files, caches, and documentation)
</pre>

关于代码源文件，sbt能够自动找到以下文件：

- 项目根目录下的源文件
- src/main/scala和src/main/java目录下的源文件
- src/test/scala和src/test/scala目录下的测试文件
- src/main/resources和src/test/resources下的数据文件
- lib目录下的jar包

target目录下存放的是sbt构建产生的文件（编译生成的class文件、打包生成的jar包等），因此一般需要将这个文件夹放在.gitignore文件中。

build.sbt文件中存放的是构建定义的描述(实际上，这个文件只需要以.sbt为结尾，名字并不重要)。另外，在project文件夹下的，以scala或sbt为结尾的文件，也是构建定义的一部分。

project文件夹，build.properties文件指定使用哪个版本的sbt来编译当前项目。plugins.sbt指定当前项目需要使用的插件。
