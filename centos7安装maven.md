## centos7安装maven

步骤

```

下载jar
wget https://mirror.bit.edu.cn/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
解压jar
tar -zvxf apache-maven-3.6.3-src.tar.gz

cd /opt/apache-maven-3.6.3/conf

vim settings.xml


```

复制以下内容到配置文件中

配置文件

```java
<?xml version="1.0" encoding="UTF-8"?>

<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" 
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
 <servers>
    
    <!--新的MAVEN私库组 -->
    <server>
      <id>central</id>
      <username>wowkai</username>
      <password>ad147min789fddfre54564ad147min789fddfre54564</password>
    </server>
  </servers>


<profiles>
<profile> 
  <id>central</id>
  <repositories>
    <repository>
      <id>central</id>
      <url>http://nexus.wowkai.cn/repository/maven-public/</url>
      <releases>
        <enabled>true</enabled>
      </releases> 
      <snapshots>
        <enabled>true</enabled>
      </snapshots>
    </repository>
  </repositories>
      
  <pluginRepositories>
    <pluginRepository>
      <id>central</id>
      <url>http://nexus.wowkai.cn/repository/maven-public/</url>
            <releases>
              <enabled>true</enabled>
            </releases> 
            <snapshots>
              <enabled>false</enabled>
            </snapshots>
    </pluginRepository>
  </pluginRepositories>
    </profile>
  </profiles>

  <!--激活仓库-->
  <activeProfiles>
    <activeProfile>central</activeProfile>
  </activeProfiles>

</settings>
```

开始配置环境变量，编辑文件/etc/profile

vim /etc/profile

在文件最后添加以下内容

```java
export MAVEN_HOME=/opt/apache-maven-3.6.3/

export PATH=$MAVEN_HOME/bin:$PATH
```

刷新环境变量

```java
source /etc/profile
```

安装git 

```java
yum -y install git
```

