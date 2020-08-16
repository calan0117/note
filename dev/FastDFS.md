## 简介

代码地址：[GITHUB](https://github.com/happyfish100/fastdfs)

官方wiki：[WIKI](https://github.com/happyfish100/fastdfs/wiki)

## 安装方法

### 1.安装依赖

作者的推荐是：

```shell
yum install git gcc gcc-c++ make automake autoconf libtool pcre pcre-devel zlib zlib-devel openssl-devel wget vim -y

```

在ubuntu下

```shell
sudo apt install git gcc make automake autoconf libtool libpcre3 libpcre3-dev zlib1g-dev  wget vim
```



### 2.安装libevent

```shell
sudo apt install libevent
```



### 3.安装libfastcommon

```shell
# 解压文件
tar -zxvf libfastcommon-1.0.39.tar.gz
cd libfastcommon-1.0.39
#编译文件
sudo ./make.sh
#安装
sudo ./make.sh install
```



### 4.安装FastDFS

FastDFS的tracker和storage在刚刚的安装过程中，都已经被安装了。不同的是，两种需要不同的配置文件。

```shell
# 解压文件
tar -zxvf fastdfs-5.11.tar.gz
cd fastdfs-5.11
#编译文件
sudo ./make.sh
#安装
sudo ./make.sh install
#安装验证
ll /etc/init.d | grep fdfs
```



## 配置配置文件

配置文件目录：/etc/fdfs

#### 1. 配置tracker

编辑tracker配置

```shell
sudo cp tracker.conf.sample tracker.conf
sudo vim tracker.conf
```

修改tracke配置

```shell
base_path=/home/fdfs/tracker # tracker的数据和日志存放目录
```

创建目录

```shell
sudo mkdir -p /home/fdfs/tracker
```

启动tracker

```shell
#启动命令
sudo sh /etc/init.d/fdfs_trackerd start
#安装时fdfs已经为系统服务，可以使用service命令
sudo service fdfs_trackerd start
#设置tracker开机启动
sudo chkconfig fdfs_trackerd on
```



#### 2. 启动storage

编辑storage配置

```shell
sudo cp storage.conf.sample storage.conf
sudo vim storage.conf
```

修改storage配置

```shell
base_path=/home/fdfs/storage # storage的数据和日志存放目录
store_path0=/home/fdfs/storage # storage的上传文件存放路径
tracker_server=192.168.86.129:22122 # tracker的地址
```

创建目录

```shell
sudo mkdir -p /home/fdfs/storage
```

启动storage

```shell
#启动命令
sudo sh /etc/init.d/fdfs_storaged start
#安装时fdfs已经为系统服务，可以使用service命令
sudo service fdfs_storaged start
#设置tracker开机启动
sudo chkconfig fdfs_storaged on
```



#### 3. 进程检查

查看进程,是否安装成功且启动成功。

```shell
ps -ef | grep fdfs
```



## 测试服务

使用自带的客户端来测试

配置客户端

```shell
sudo cp client.conf.sample client.conf
sudo vim client.conf
```

修改客户端配置

```shell
base_path=/home/fdfs/client
tracker_server=192.168.86.129:22122 # tracker的地址
```

测试命令

```shell
#再带脚本测试方式                         # 要上传的文件地址
/usr/bin/fdfs_upload_file client.conf  /home/test.jpg 
```

成功会返回一个ID

```
group1/M00/00/00/weDEJDdedeEdeEFjliUBuiwepoJMBU.jpeg
```

ID的解析：

1. group1:组信息

2. M00:store_path0

3. /00/00:磁盘路径



## 安装Nginx的FastDFS模块

### 解压

```shell
tar -xvf fastdfs-nginx-module_v1.16.tar.gz
```

### 配置config文件

进入文件夹

```shell
#进入到解压后的文件夹
cd /FstDFS/fastdfs-nginx-module-1.20/src
```

#### 配置config文件

```shell
# 进入配置目录
cd /home/lolo/FstDFS/fastdfs-nginx-module/src/
# 修改配置
vim config
# 执行下面命令（将配置中的/usr/local改为/usr）：
:%s+/usr/local/+/usr/+g
```

#### 配置mod_fastsfd.conf文件

```shell
# 将src目录下的mod_fastdfs.conf复制到 /etc/fdfs目录：
sudo cp mod_fastdfs.conf /etc/fdfs/
# 编辑该文件
sudo vim /etc/fdfs/mod_fastdfs.cof
```

##### 修改配置

```shell
connect_timeout=10                  		# 客户端访问文件连接超时时长（单位：秒）
tracker_server=192.168.86.129:22122  	# tracker服务IP和端口
url_have_group_name=true            		# 访问链接前缀加上组名
store_path0=/home/fdfs/storage        		# 文件存储路径
```

##### 复制 FastDFS的部分配置文件到/etc/fdfs目录

```shell
#解压文件的路径中/fastdfs-5.11/conf/
cd /home/lolo/FastDFS/fastdfs-5.11/conf/
sudo cp http.conf mime.types /etc/fdfs/
```

#### 配置nginx整合fastdfs-module模块

进入nginx包文件目录

```shell
sudo ./configure --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx  --conf-path=/etc/nginx/nginx.conf --add-module=/home/lolo/FastDFS/fastdfs-nginx-module/src/

#重新编译不安装
sudo make
```



#### 替换之前的nginx脚本

备份之前的脚本

```shell
sudo cp /usr/sbin/nginx /usr/sbin/nginx-back
```

用编译好的替换原先的nginx

```shell
sudo cp /home/lolo/Downloads/nginx-1.16.0/objs/nginx /usr/sbin
```
