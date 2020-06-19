[toc]

### zookeeper学习

##### docker安装zookeeper

```java
docker pull zookeeper
docker run --privileged=true -d --name zookeeper --publish 2181:2181 -d zookeeper:latest

```

