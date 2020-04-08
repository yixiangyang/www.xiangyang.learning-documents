进入  /opt 下 

下载 wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.19-el7-x86_64.tar.gz

tar -vxf mysql-8.0.19-el7-x86_64.tar.gz 

cd mysql-8.0.19-el7-x86_64

创建用户组  groupadd mysql

useradd -g mysql mysql

为mysql的安装目录授权

chown -R mysql:mysql /opt/mysql-8.0.19-el7-x86_64

初始化

bin/mysqld --initialize --user=mysql --basedir=/opt/mysql-8.0.19-el7-x86_64 --datadir=/opt/mysql-8.0.19-el7-x86_64/data



初始化 报错 

error while loading shared libraries: libaio.so.1: cannot open shared object file: No such file or directory



安装 yum install -y libaio

安装成功 初始化的密码 rudaj?*vr0aC

修改 mysql 配置文件 vim /etc/my.cnf

```java
[mysqld]
datadir=/opt/mysql-8.0.19-el7-x86_64
socket=/opt/mysql-8.0.19-el7-x86_64/mysql.sock
port = 3317
user = mysql
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
init_connect=’SET NAMES utf8mb4'
default_authentication_plugin=mysql_native_password
max_connections=2500
# time zone
default-time_zone = '+0:00'

# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mariadb according to the
# instructions in http://fedoraproject.org/wiki/Systemd

[mysqld_safe]
# log-error=/var/log/mariadb/mariadb.log
# pid-file=/var/run/mariadb/mariadb.pid

#
# include all files from the config directory
#
!includedir /etc/my.cnf.d

```



添加到启动

```
  cp support-files/mysql.server /etc/init.d/mysqld
  chkconfig  --add mysqld
  检查服务是否生效  
  chkconfig  --list mysqld
```

修改mysql密码

ALTER user 'root'@'localhost' IDENTIFIED BY 'yourpassword'

设置可以远程登录

use mysql

update user set host='%' where user='root' limit 1; 

刷新权限

flush privileges