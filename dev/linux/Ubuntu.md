开机启动项设置

```shell
# 查询开机启动项
systemctl list-unit-files 
systemctl list-unit-files |  grep mysql

# 启动一个服务
systemctl enable xxxxx.service

# 关闭一个自动启动
systemctl disable xxxxx.service

# 重启
systemctl restart xxxxx.service

# 查看状态
systemctl status xxxxx.service

# 启动服务
systemctl start xxxxx.service

```

