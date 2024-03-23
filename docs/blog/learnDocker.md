# Docker学习

Docker包括三个基本概念

- 镜像（Image），可以看作一个root的文件系统
- 容器 （Container），跟镜像的关系，可以理解为面向对编程中类和实例的关系，镜像式静态的，可容器是运行的，动态的
- 仓库（Repository）。可以理解为代码管理系统

![img](https://www.runoob.com/wp-content/uploads/2016/04/576507-docker1.png)

Docker使用客户端-服务端（C/S）模式，可以用通过远程api来创建和管理Docker容器





## Docker安装（Ubuntu）



### 官方脚本

```bash
 curl -fsSL https://test.docker.com -o test-docker.sh
 sudo sh test-docker.sh
```





### 卸载Docker

删除安装包

````
sudo apt-get purge docker-e
````

删除镜像，容器，配置文件

```
sudo rm -rf /var/lib/docker
```





### Docker Helloo,world

```
docker run ubuntu:15.10 /bin/echo "Hello world"
```

各个参数解析：

- **docker:** Docker 的二进制执行文件。
- **run:** 与前面的 docker 组合来运行一个容器。
- **ubuntu:15.10** 指定要运行的镜像，Docker 首先从本地主机上查找镜像是否存在，如果不存在，Docker 就会从镜像仓库 Docker Hub 下载公共镜像。
- **/bin/echo "Hello world":** 在启动的容器里执行的命令



### 运行交互式容器

```
docker run -i -t ubuntu:15.10 /bin/bash
```

查看容器环境版本

```bash
cat proc/version
```

退出虚拟环境

```bash
exit
```

### 后台运行

```
docker run  -d ubuntu:15.10 /bin/sh -c "while true; do echo hello,world;sleep 1;done"
```

### 查看后台运行

```bash
docker ps
```

![image-20240323120133672](D:\xiaolog\docs\blog\image-20240323120133672.png)

输出详情介绍：

**CONTAINER ID:** 容器 ID。

**IMAGE:** 使用的镜像。

**COMMAND:** 启动容器时运行的命令。

**CREATED:** 容器的创建时间。

**STATUS:** 容器状态。

状态有7种：

- created（已创建）
- restarting（重启中）
- running 或 Up（运行中）
- removing（迁移中）
- paused（暂停）
- exited（停止）
- dead（死亡）

**PORTS:** 容器的端口信息和使用的连接类型（tcp\udp）。

**NAMES:** 自动分配的容器名称。

### 查看运行日志

```bash
docker logs ID
```

### 停止容器

```bash
docker stop ID
```

或

```bash
docker stop NAMES
```



### 容器使用

#### 获取镜像

```bash
docker pull ubuntu
```

#### 启动容器

```bash
docker run -it ubuntu /bin/bash
```

### 进入容器

在使用 **-d** 参数时，容器启动后会进入后台。此时想要进入容器，可以通过以下指令进入：

- **docker attach**
- **docker exec**：最好使用 docker exec 命令，因为此命令会退出容器终端，但不会导致容器的停止。

```bash
docker exec -it ID /bin/bash	
```

### 导出和导入容器

#### 导出容器

```bash
docker export id > ubuntu.tar
```

#### 导入容器

```bash
cat docker/ubuntu.tar | docker import - test/ubuntu:v1
```

**详细解释：**

1. `cat docker/ubuntu.tar`：
   - `cat` 是Linux命令，用于连接并打印文件内容到标准输出（stdout）。在这里，它读取并输出`docker/ubuntu.tar`文件的内容。
2. `|`（管道符）：
   - 它将`cat`命令的输出传递给下一个命令作为输入，也就是管道操作符将tar文件内容流式传输至下一条命令。
3. `docker import - test/ubuntu:v1`：
   - `docker import` 是Docker CLI命令，用于从tar归档文件中导入一个镜像。
   - `-` 表示标准输入（stdin），在此情境中意味着接受上一步`cat`命令输出的tar文件内容作为输入。
   - `test/ubuntu:v1` 是新镜像的命名规范，其中`test`是镜像仓库名，`ubuntu`是镜像名称，`:v1`则是镜像标签。这意味着导入后的镜像将被注册为`test`仓库下的`ubuntu`镜像，并打上标签`v1`。



### 删除容器

```bash
docker rm -f ID
```

### 运行一个web应用

```bash
docker pull training/webapp
docker run -d traning/webapp python app.py
```

![image-20240323122911630](C:\Users\colds\AppData\Roaming\Typora\typora-user-images\image-20240323122911630.png)

指定某一端口映射，可使用-p参数

```bash
dockr run -d -p 5001:5000 training/webapp python app.py
```

快捷查看某一容器运行的端口

```
docker port id
```



### 查看本机docker镜像

```bash
docker images
```

