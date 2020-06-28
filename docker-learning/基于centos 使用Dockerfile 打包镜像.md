[toc]

## 基于centos 使用Dockerfile 打包镜像

创建目录

创建Dockerfile文件

```java
FROM centos
MAINTAINER xiangyang
WORKDIR /opt/
ADD jdk-8u251-linux-x64.tar.gz /opt/
#配置环境变量
ENV JAVA_HOME=/opt/jdk1.8.0_251
ENV CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV PATH=$JAVA_HOME/bin:$PATH
```

构建镜像

```java
docker build -t centos_jdk8:v1.0 .
. 代表当前目录下
运行
docker run -it centos_jdk8:v1.0 /bin/bash

 java -version #查看是否成功
```

