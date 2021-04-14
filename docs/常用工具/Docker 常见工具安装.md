# Docker 常见工具安装

## MySQL

>   https://www.runoob.com/docker/docker-install-mysql.html

```shell
# 拉取镜像
docker pull mysql

# 启动服务
docker run -itd --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql

#进入容器
docker exec -it mysql-test bash

#登录mysql
mysql -u root -p
```

| 参数                         | 作用                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| `-i`                         | 以交互模式运行容器，通常与 -t 同时使用                       |
| `-t`                         | 为容器重新分配一个伪输入终端，通常与 -i 同时使用             |
| `-d`                         | 后台运行容器，并返回容器ID                                   |
| `-p 3306:3306`               | 映射容器服务的 3306 端口到宿主机的 3306 端口，外部主机可以直接通过 **宿主机ip:3306** 访问到 MySQL 的服务。 |
| `MYSQL_ROOT_PASSWORD=123456` | 设置 MySQL 服务 root 用户的密码                              |

## Redis

>   https://www.runoob.com/docker/docker-install-redis.html

```shell
# 拉取镜像
docker pull redis
# 启动服务
docker run -itd --name redis-test -p 6379:6379 redis
# 测试服务
docker exec -it redis-test bash
```

## Kafka

>   https://www.jianshu.com/p/1d7759d77ec1

```shell
# 下载最新版的docker-compose文件 
sudo curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo curl -L "https://get.daocloud.io/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# 添加可执行权限 
sudo chmod +x /usr/local/bin/docker-compose

# 测试安装结果 
docker-compose --version 
```

编辑docker-compose.yml

```ruby
version: '2'
services:
  zookeeper:
    image: wurstmeister/zookeeper   ## 镜像
    ports:
      - "2181:2181"                 ## 对外暴露的端口号
  kafka:
    image: wurstmeister/kafka       ## 镜像
    volumes:
        - /etc/localtime:/etc/localtime ## 挂载位置（kafka镜像和宿主机器之间时间保持一直）
    ports:
      - "9092:9092"
    environment:
      KAFKA_ADVERTISED_HOST_NAME: lcoalhost   ## 修改:宿主机IP
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181       ## kafka运行是基于zookeeper的
      KAFKA_ADVERTISED_PORT: 9092
```

启动服务

```shell
docker-compose up -d
```

停止服务

```shell
docker-compose stop
```

停止并删除服务

```shell
docker-compose down
```

验证

```shell
# 通过容器名称进入到kafka容器中：
docker exec -it docker-kafka_kafka_1 /bin/bash

# 创建一个名称为test的topic：
kafka-topics.sh --create --topic test --zookeeper zookeeper:2181 --replication-factor 1 --partitions 1

# 查看刚刚创建的topic信息：
kafka-topics.sh --zookeeper zookeeper:2181 --describe --topic test

# 打开生产者发送若干条消息：
kafka-console-producer.sh --topic=test --broker-list kafka:9092

# 开发消费者接收消息：
kafka-console-consumer.sh --bootstrap-server kafka:9092 --from-beginning --topic test

# 如果可以成功接收到消息，则说明kafka已经启动成功了，可以进行本地开发以及调试工作了
```