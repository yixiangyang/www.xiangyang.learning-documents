centos中 配置阿里云镜像加速器

##### 1、打开阿里云的容器镜像服务 查看自己的加速器地址

##### 2、在 /etc/docker 下配置 daemon.json

```java
cd /etc/docker
vim daemon.json
粘贴以下内容

{
  "registry-mirrors": ["https://0o5w306g.mirror.aliyuncs.com"]
}


```

##### 3、重启dokcer

```java
systemctl daemon-reload

systemctl restart docker.service
```

##### 4、查看docker是否配置成功

```java
docker info
```

