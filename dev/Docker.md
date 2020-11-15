### 主要站点

推荐书籍[docker从入门到实践](https://yeasy.gitbook.io/docker_practice/)

[官网](https://www.docker.com/)

[官方文档](https://docs.docker.com/)



### 安装

#### 在Ubuntu下安装

[Ubuntu安装教程](https://yeasy.gitbook.io/docker_practice/install/ubuntu)

#### 切换docker镜像源实现下载加速

添加文件

```shell
cd /etc/docker
# 检查有没有daemon.json 没有就创建
touch daemon.json
```

添加镜像源

```json
{
  "registry-mirrors": [
    "https：//docker.mirrors.ustc.edu.cn",
    "https://hub-mirror.c.163.com",
    "https://mirror.baidubce.com"
  ]
}
```

重启服务刷新

```shell
sudo systemctl restart docker.service
```



### docker

#### 基本概念

**镜像(image)** 提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。

**容器(container)**容器是镜像运行时的实体  

**仓库(Repository)** 包含注册中心(Registry)和仓库(Repository)。一个 **Docker Registry** 中可以包含多个 **仓库**。每个仓库可以包含多个 **标签**（`Tag`）；每个标签对应一个镜像。

#### 常用命令

```shell
# 获取镜像
docker pull mysql:8.0

# 查询镜像
docker image ls 
docker image ls -a # 查询所有镜像，包括中间镜像
docker image ls mysql # 查询部分镜像
docker image ls -f since=mysql:8.0 # 查询mysql 8.0之后安装的镜像。也可以使用before等
docker image ls --format "{{.ID}}: {{.Repository}}" # 以指定格式显示

# 删除镜像
docker image rm mysql 

# 查询容器
docker container ls

# 进入容器
docker exec -it mysql bash # -i
```



### dockerfile

可以自行定制镜像

#### 示例

将nginx的初始页面替换一下。

```shell
FROM nginx
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
```



#### 常用命令

```shell
# 这里在nginx:v3后面有一个点 . 代表的是打包上下文目录(Context)
docker build -t nginx:v3 .
```



### docker-compose

对docker 编排，快速部署工具。

#### 安装

```shell
# 版本这里是1.25.5 可以去github上找到对应的版本。也可以手动下载上传至安装机器。
$ sudo curl -L https://github.com/docker/compose/releases/download/1.25.5/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

# 添加权限
$ sudo chmod +x /usr/local/bin/docker-compose
```

#### 示例

在本地配置一个mysql的镜像使用

```yml
version: '3' # 根据版本来
services:
  mysql:
   image: mysql:8.0
   container_name: mysql8.0
   ports:
     - 3306:3306
   environment:
     - "MYSQL_ROOT_PASSWORD=rootroot" 
     - "TZ=Asia/Shanghai"
   # 映射本地目录到容器内
   volumes: 
     - $PWD/data:/var/lib/mysql
     - $PWD/log:/var/log/mysql
     - $PWD/conf:/etc/mysql/conf.d
   # 让mysql 8使用原先的认证方式
   command: --default-authentication-plugin=mysql_native_password
```

配置文件

```
[mysqld]
default-time-zone = '+8:00'
```

#### 常用命令

```shell
# 检测配置文件格式
dokcer-compose config

# 尝试自动完成包括构建镜像，（重新）创建服务，启动服务，并关联服务相关容器的一系列操作。
docker-compose up 

# 停止正在运行的容器
docker-compose stop

# 启动已存在的容器
docker-compose start

```

