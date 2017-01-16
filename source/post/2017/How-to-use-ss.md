```toml
title = "How to use ss"
slug = "How-to-use-ss"
desc = "How to use ss"
date = "2017-01-16 15:00:30"
update_date = "2017-01-16 15:00:30"
author = "juejohn"
thumb = ""
tags = ["tag"]
```
* 安装ShadowSocks:
```  
sudo apt-get install python-pip
sudo pip install shadowsocks  
```  
* 对ShadowSocks进行配置：  
在/etc/目录下建立一个sss.json 文件  
```
{
    "server":"0.0.0.0",
    "server_port":自定义端口,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"自定义密码",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": true
}
```  
"server_port"后跟一个 大于1024, 小于65535 的数字  
* 基础命令  
```  
运行：
ssserver -c /etc/sss.json -d start  
停止：  
ssserver -c /etc/sss.json -d stop  
```
*客户端下载：  
[Site](https://github.com/shadowsocks/shadowsocks/wiki/Ports-and-Clients)  

*客户端的使用：  
在客户端上面输入你的服务器ip，上面sss.json配置文件中的server_port、和password就行了。