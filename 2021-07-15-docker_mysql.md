# docker 上安装mysql

[toc]

## 一、拉取mysql镜像

> sudo docker image pull mysql:5.7

```
$ sudo docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
mysql        5.7       09361feeb475   3 weeks ago   447MB
```



## 二、运行mysql镜像

```
# --name指定容器名字 -v目录挂载 -p指定端口映射  -e设置mysql参数 -d后台运行
sudo docker run -p 3307:3306 --name mysql \
-v /Users/lifei/Documents/opt/docker_wp/mydata/mysql/log:/var/log/mysql \
-v /Users/lifei/Documents/opt/docker_wp/mydata/mysql/data:/var/lib/mysql \
-v /Users/lifei/Documents/opt/docker_wp/mydata/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7
```

## 三、查看容器

```
$ sudo docker container ls
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS         PORTS                                                  NAMES
03d567d751f9   mysql:5.7   "docker-entrypoint.s…"   13 seconds ago   Up 7 seconds   33060/tcp, 0.0.0.0:3307->3306/tcp, :::3307->3306/tcp   mysql

$ sudo docker container ls -a
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                                                  NAMES
03d567d751f9   mysql:5.7   "docker-entrypoint.s…"   16 seconds ago   Up 10 seconds   33060/tcp, 0.0.0.0:3307->3306/tcp, :::3307->3306/tcp   mysql
```



## 四、运行mysql

```
mysql -h127.0.0.1  -P3307 -uroot -p
```

## 五、重启docker 中的mysql服务

```
# 查看正在运行的容器
$ sudo docker container ls
CONTAINER ID   IMAGE       COMMAND                  CREATED         STATUS         PORTS                                                  NAMES
03d567d751f9   mysql:5.7   "docker-entrypoint.s…"   9 minutes ago   Up 8 minutes   33060/tcp, 0.0.0.0:3307->3306/tcp, :::3307->3306/tcp   mysql
# 停掉正在运行的容器
$ sudo docker container stop 03d567d751f9
03d567d751f9

# 停掉容器之后，连接mysql，连接失败
$ mysql -h127.0.0.1  -P3307 -uroot -p
Enter password:
ERROR 2003 (HY000): Can't connect to MySQL server on '127.0.0.1' (61)

# 查看所有的容器
$ sudo docker container ls -a
CONTAINER ID   IMAGE       COMMAND                  CREATED         STATUS                      PORTS     NAMES
03d567d751f9   mysql:5.7   "docker-entrypoint.s…"   9 minutes ago   Exited (0) 25 seconds ago             mysql

# 启动容器
$ sudo docker container start 03d567d751f9
03d567d751f9

# 启动容器后，连接mysql
$ mysql -h127.0.0.1 -P3307 -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.34 MySQL Community Server (GPL)

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

