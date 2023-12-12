# Jenkins

## 1. 使用docker-compose安装

```yml
version: '3.1'
services:
  jenkins:
    image: jenkins/jenkins:lts
    volumes:       # 挂载目录  本地文件夹目录:容器文件夹目录
      - ./jenkins_home/:/var/jenkins_home #将本地的/data/jenkins/文件夹挂载到容器内的/var/jenkins_home目录，用于存储Jenkins数据和配置。
      - /var/run/docker.sock:/var/run/docker.sock  #将宿主机的Docker套接字挂载到容器内，以便在容器内执行Docker命令。
      - /usr/bin/docker:/usr/bin/docker #将宿主机的Docker二进制文件挂载到容器内，以便在容器内执行Docker命令。
      - /usr/lib/x86_64-linux-gnu/libltdl.so.7:/usr/lib/x86_64-linux-gnu/libltdl.so.7 #将宿主机的libltdl.so.7库文件挂载到容器内，以解决一些插件所需的依赖问题。
    ports:   # 绑定端口
      - "8080:8080"
    expose:  # 暴露端口
      - "8080"
      - "50000"
    privileged: true #在容器内运行时使用特权模式。
    user: root #以root用户身份运行容器。
    restart: always #设置容器始终自动重启。
    container_name: jenkins #指定容器的名称。
    environment: #定义容器的环境变量。
      JAVA_OPTS: '-Djava.util.logging.config.file=/var/jenkins_home/log.properties' #设置Jenkins的日志配置文件路径
```