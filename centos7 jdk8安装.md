centos7 jdk8安装

下载：Windows下载 传至服务器

修改配置文件 vim /etc/profile

```java
# java 8配置

JAVA_HOME=/opt/jdk1.8.0_241
CLASSPATH=$JAVA_HOME/lib/
PATH=$PATH:$JAVA_HOME/bin
export PATH JAVA_HOME CLASSPATH
```

java -version 验证环境是否正确

遇到错误 

```java
-bash: /opt/jdk1.8.0_241/bin/java: /lib/ld-linux.so.2: bad ELF interpreter: No such file or directory

```



yum install glibc.i686

再次验证