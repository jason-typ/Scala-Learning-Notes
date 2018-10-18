## Mac上查看和设置JAVA_HOME

Mac上JDK的安装路径可以通过`/usr/libexec/java_home`工具查看：

<pre>
➜  ~ /usr/libexec/java_home
/Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home
</pre>

上面的命令就会输出jdk的真实路径，根据这个值来修改`/etc/profile`文件，设置`JAVA_HOME`。在文件中增加以下内容：

<pre>
#JAVA VARIABLE START
export JAVA_HOME=$(/usr/libexec/java_home)
export PATH=$PATH:${JAVA_HOME}
#JAVA VARIABLE END
</pre>

## Mac上ssh失败

提示`connect to host localhost port 22: Connection refused`，需要开启远程登录：`System Preferences -> Sharing -> remote login`，勾选上该选项即可

