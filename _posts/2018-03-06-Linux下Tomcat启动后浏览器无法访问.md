---
layout:     post
title:     【原创】Linux下Tomcat启动后浏览器无法访问
subtitle:   Linux服务配置
date:       2018-03-06
author:     Bob
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - Linux,运维，学习笔记
---
## 虚拟机上安装centOS6.8 Tomcat后可以正常运行，可是这时Tomcat还不能被外界浏览器访问

## 解决方案： 需要在centOS默认防护墙上打开8080端口；
### 关闭防火墙：
```
sudo service iptables stop
```
### 编辑防火墙配置
```
vi /etc/sysconfig/iptables
```

在

-A INPUT -j REJECT --reject-with icmp-host-prohibited

-A FORWARD -j REJECT --reject-with icmp-host-prohibited

这句上面添加

```
-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 9904 -j ACCEPT
```
-----

```
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
```
（之前我添加在下面，浏览器也是不能访问的，必须放在上面！）

###允许8080端口通过防火墙
```
-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
```
###允许3306端口通过防火墙
```
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
```
###允许9904端口通过防火墙
```
-A INPUT -m state --state NEW -m tcp -p tcp --dport 9904 -j ACCEPT
```
####输入esc，输入：wq保存退出；



### 重启防火墙：
```
service iptables restart
```


打开外部浏览器，输入
```
http://centOS IP:8080
```
即可看到Tomcat欢迎界面！
![](https://ws1.sinaimg.cn/large/006tNc79gy1fq8tuueoanj30jh0e7wf1.jpg)

