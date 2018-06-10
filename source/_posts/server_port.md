---
title: linux下关于服务端口的配置
tags: 运维
categories: 服务器
---
### 端口服务

>查看端口使用情况

```
netstat命令各个参数说明如下:
    -t : 指明显示TCP端口
    -u : 指明显示UDP端口
    -l : 仅显示监听套接字(所谓套接字就是使应用程序能够读写与收发通讯协议(protocol)与资料的程序)
    -p : 显示进程标识符和程序名称，每一个套接字/端口都属于一个程序。
    -n : 不进行DNS轮询，显示IP(可以加速操作)
netstat -ntlp   //查看当前所有tcp端口·
netstat -ntulp |grep 80   //查看所有80端口使用情况·
netstat -an | grep 3306   //查看所有3306端口使用情况·
```

### 端口映射配置

>需求：
>PC_A是 eth0: 172.18.10.212  内网；eth1: 219.239.xx.xx  外网
>
>PC_B是 172.18.10.205  内网
>
>A的8080端口映射到B的80端口

#### 1. 首先应该做的是
```
/etc/sysctl.conf配置文件:
    net.ipv4.ip_forward = 1 默认是0.
这样允许iptalbes FORWARD。
```
#### 2. 在/etc/rc.d/init.d目录下有iptables 文件，使用格式如下
```
Usage: ./iptables {start|stop|restart|condrestart|status|panic|save}
    相当与service iptables {....}
    把iptables 服务停止，清除以前的规则，存盘
    
到/etc/rc.d/init.d目录下，运行
    ./iptables stop
    iptalbes -F
    iptalbes -X
    iptalbes -Z
    ./iptables save
```
#### 3. 重新配置规则
```
iptables -t nat -A PREROUTING -d 219.239.xx.xx -p tcp --dport 8080 -j DNAT --to-destination 172.18.10.205:80

iptables -t nat -A POSTROUTING -d 172.18.10.205 -p tcp --dport 80 -j SNAT --to 172.18.10.212

iptables -A FORWARD -o eth0 -d 172.18.10.205 -p tcp --dport 80 -j ACCEPT

iptables -A FORWARD -i eth0 -s 172.18.10.205 -p tcp --sport 80 -j ACCEPT

DNAT SNAT 的请参考帮助，这里不再陈述。
```
#### 4. 新的规则存盘
```
 ./iptables save

规则存盘后在/etc/sysconfig/iptables这个文件里面，若你对这个文件很熟悉
直接修改这里的内容也等于命令行方式输入规则。
```
#### 5. 启动iptables 服务
```
 ./iptables start
 在/proc/net/ip_conntrack文件里有包的流向，如下面

    tcp 6 53 TIME_WAIT src=221.122.59.2 dst=219.239.xx.xx sport=7958 dport=8080 packets=9 bytes=1753
    
    src=172.18.10.205 dst=172.18.10.212 sport=80 dport=7958 packets=9 bytes=5777 [ASSURED] use=1
```