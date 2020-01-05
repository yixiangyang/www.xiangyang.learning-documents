<h1><center>kafka学习 </center></h1>
##### 一、kafka启动

- kafka使用zookeeper  ，首先启动zookeeper  

  ```java
  bin/zookeeper-server-start.sh config/zookeeper.properties
  ```

- 后台启动kafka

  ```java
  nohup bin/kafka-server-start.sh config/server.properties 1>/dev/null 2>&1 &
  ```

