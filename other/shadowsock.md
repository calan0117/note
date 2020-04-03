1.后台运行命令
```
ssserver -c /etc/shadowsocks.json -d start
ssserver -c /etc/shadowsocks.json -d stop
```

2.ss多端口配置
```
{
    "server":"xx.xx.xx.xx",
    "local_address": "127.0.0.1",
    "local_port":1080,
    "port_password": {
         "8383": "password",
         "8384":"password",
     },  
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```