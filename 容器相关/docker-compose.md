# 个人理解

用于统一所有人的环境

例如：开发环境需要有，mysql，redis，mq。。等一系列的组件。在没有docker 之前，需要每个人下载安装调试，最后的效果可能还不同（不同平台，不同配置差异）电脑展示效果不一样。

在docker 之后，都统一的pull 下来同一个compose文件，里面有选好的版本和，统一的配置文件。



# 安装

#### 下载

```
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

#### 检测是否成功

```
$ docker-compose --version
cker-compose version 1.24.1, build 4667896b
```



# 常用命令

#### 

```
$ docker-compose up -d #运行当前文件夹的yml
$ docker-compose down  #关闭cmopose
```







# 实战

#### 需求

搭建一个所有人通用的环境，环境包括redis，mysql。要求配置文件可以修改，数据要保存下来（不能出现重启数据就不存在啦）

```
version: '3'
services:
  #Reids
  redis:
    image: redis:5.0
    ports:
    - "6379:6379"
    volumes:
    - "redis_data:/data:rw"
    - ./conf/redis.conf:/etc/redis.conf

  #Mysql
  mysql:
    image: mysql:5.7
    environment:
      - MYSQL_ROOT_PASSWORD=123456
    ports:
    - "3307:3306"
    volumes:
      - mysql_data:/var/lib/mysql:rw
      - ./config/my.cnf:/etc/mysql/mysql.conf.d/mysqld.cnf

#定义 文件
volumes:
  redis_data: {}
  mysql_data: {}
```