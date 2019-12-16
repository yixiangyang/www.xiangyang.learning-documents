## docker学习

#### docker镜像学习

#### docker images 列出本地镜像列表

- **REPOSITORY：** 表示镜像的仓库源
- **TAG：**镜像的标签
- **IMAGE ID：**镜像ID
- **CREATED：**镜像的创建时间
- **size：**镜像的大小

docker run -t -i ubuntu:15.10 /bin/bash 

- **-i：**交互式操作
- **-t：**终端
- **ubuntuL15.10：**这是指用ubuntu 15.10版本为镜像来启动容器
- **/bin/bash：**放在镜像后的是命令，这里我们希望有个交互式shell,因此用的是/bin/bash

#### 获取一个新的镜像 docker pull

docker 在拉取镜像的时候都是默认为 lastest  这里如果想指定拉取一个指定的tag镜像，我们可以先去

 [https://hub.docker.com](https://hub.docker.com/) 官网搜索镜像名字，然后再去查看tags ，最后拉取我们想要的版本 比如我这里的 docker pull centos:centos7  拉的是centos中标签名为centos7的镜像

#### 查找镜像docker search

- **NAME：**镜像仓库源的名称
- **DESCRIPTION：**镜像的描述
- **OFFICIAL：**是否docker官方发布
- **starts:**类似于github里面的star，表示点赞、欢迎的意思
- **AUTOMATED：**自动构建。

#### 运行镜像 docker run

#### 删除镜像 docker rmi 镜像名字 或镜像ID

#### 创建镜像

**1、 **从已经创建的容器中更新镜像，并提交这个镜像。

**2、** 使用Dockerfile指令来创建一个新的镜像

#### 更新镜像

更新镜像之前，我们需要使用镜像来创建一个容器

```java
docker run -i -t centos:centos7 /bin/bash
```

 在运行的容器内使用 **apt-get update** 命令进行软件更新

更新完容器后我们使用docker commit 来提交镜像

```java
docker commit -m="第一次测试修改" -a="xiangyang" a7e0ea1e77cc xiangyang/centos:test
```

- **-m:**提交的描述信息
- **-a:**指定的作者信息
-  **a7e0ea1e77cc：**容器的ID 我们可以使用docker ps -a 去查看所有的容器
-  **xiangyang/centos:test：**这里是我们指定要创建的目标镜像名和标签

#### 构建镜像

使用Dockerfile 以下是一个 Dockerfile文件

```java
#指定基础镜像
FROM  centos:centos7
# ADD可以直接解压压缩包
ADD jdk-13.0.1_linux-x64_bin.tar.gz /opt/
ENV JAVA_HOME /opt/jdk-13.0.1
ENV JRE_HOME $JAVA_HOME/jre
ENV CLASSPATH $JAVA_HOME/lib/:$JRE_HOME/lib/
ENV PATH $PATH:$JAVA_HOME/bin
#运行java -version 确定环境变量是否安装成功
RUN java -version

```

我们使用docker build来构建我们的镜像

```java
docker build -t xiangyang/centosdockerfile:test .
```

- **-t：**指定要创建的目标镜像名

- **. ：** Dockerfile 文件所在目录，可以指定Dockerfile 的绝对路径 

最后使用docker images查看我们的镜像，会发现刚刚创建的镜像已经在里面了。

