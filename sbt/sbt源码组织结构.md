## sbt源码组织结构

假设项目的基础目录是`hello`，源码可以直接放在基础目录下，如`hello/hello.scala`。但是这样太乱了，sbt和Maven默认的源目录结构是一样的：

<pre>
HelloWorld  (base directory)
    build.sbt
    .gitignore  (用于忽略文件或目录)

    project/
        xx.scala (defines helper objects and one-off plugins，辅助对象和一次性插件)
        xx.sbt (并不等价于基本目录下的sbt文件)

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

`src/`中其他的目录将被忽略（包括隐藏目录）。sbt构建产生的文件（编译生成的class文件、打包生成的jar包等）默认放在target目录下