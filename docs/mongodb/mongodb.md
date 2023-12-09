# 1.核心概念
## 1.1 库`<DataBase>`
> 与传统数据库类似，通过不同库隔离不同的应用数据<br/>
> 每一个库都有自己的集合和权限，不同数据库也放置在不同的文件中<br/>
> 默认的数据库为`test`，数据库存储在启动时指定的data目录中
## 1.2 集合`<Collection>`
> 集合存在于数据库中，一个库可以创建多个集合。每个集合没有固定的结构，所以可以插入不同的格式和类型的数据<br/>但通常生产情况下我们插入集合的数据需要会具备有一定的关联性<br/>
> 类似于传统数据库表的概念，不过集合中存储的文档是相对松散不作限制的
## 1.3 文档`<Document>`
> 属于集合中的一条条的记录，是一组键值对`（key-value）`<br/>
> 不需要设置相同的字段，并且不需要设置相同的数据类型<br>

一个文档例子如下
```json
{"url":"www.baidu.com","name":"百度"}

```

# 2.docker-compose安装

```yml
version: '3'
# 网桥mongo -> 方便相互通讯
networks:
  mongo:
services:
  # mongodb
  mongodb:
    image: mongo:latest  # 原镜像`mongo:4.4.6`
    restart: unless-stopped #在容器退出时总是重启容器，但是不考虑在Docker守护进程启动时就已经停止了的容器
    container_name: mongodb
    environment: #容器内部的环境变量
      MONGO_INITDB_ROOT_USERNAME: admin #账号
      MONGO_INITDB_ROOT_PASSWORD: 123456 #密码
      MONGO_DATA_DIR: /data/db
      MONGO_LOG_DIR: /data/logs
    volumes:
      - ./mongodb/db:/data/db
      - ./mongodb/log:/data/log
    ports:
      - "27017:27017"
    networks:
      - mongo
  # 可视化图形工具
  adminmongo:
    image: mrvautin/adminmongo
    restart: unless-stopped
    container_name: adminmongo
    environment:
      - HOST=0.0.0.0
    depends_on:
      - 'mongodb'
    links:
      - mongodb
    ports:
      - "1234:1234"
    networks:
      - mongo
```


# 3. 基本操作

## 注意事项
- `docker中运行bash: mongo: command not found报错问题处理`
  - `mongo命令在mongodb 6.0已经不适用了,直接使用mongosh`
