# linux 操作指南

## 网络环境查看命令
> ifconfig : 查看网卡配置信息

> ifup : 启用

>ifdown : 禁用

>netstat [option] : 查询网络状态
* -t:列出TCP协议端口
* -u:列出UDP协议端口
* -n:不适用域名和服务名，而使用IP地址和端口号
* -l:仅列出在监听状态下的网络服务
* -a:列出所有网络连接

> 常用的组合 netstat -tuln || netstat -an || netstat -rn

> route命令
* route -n 列出网关
* route add default gw 192.168.1.1 设置网关，一般不用
* rount del default gw 192.168.1.1 删除

> 域名解析命令
* nslookup [主机名或者ip] 进行域名与ip地址解析

> ping
* ping www.baidu.com -c 3  ping3次

> telnet 探查指定id是否开的端口号
* telnet 192.168.1.241 80

> traceroute 路由跟踪命令
* traceroute www.baidu.com

> tcpdump -i eth0 -nnX port 21 抓包命令
* -i 指定网卡接口
* -nn 将数据包当中的域名和服务转换成ip和端口
* -X 以16进制和ASCII码来显示数据包中的内容
* port 指定监听端口

## 常用命令
### 命令提示符

![基础](./img/pic05.jpg)
### 文件权限

![文件权限](./img/pic06.jpg)

. 代表ACL 权限
### 命令的基本格式
* ls 查询目录中的内容

|选项|注解|
|:-:|:-|
|-a|显示所有文件,包括隐藏文件|
|-l|显示详细信息|
|-d|查看目录属性|
|-h|人性化显示文件大小|
|-i|显示inode|

    常用方法:
    * ls -ah
    * ls -al
    * 以点开头的文件都是隐藏文件

### 目录文件处理命令
* 建立文件 mkdir -p [目录名]
> -p 递归创建 英文 make directories

### 链接处理命令
* ln -s [原文件] [目标文件]
* 英文翻译:link
* 功能描述:生成链接文件
* 选项: -s 创建软连接
* 硬链接和软连接的对比

|Id|硬链接|软连接|
|:-:|:-|:-:|
|1|拥有相同的i节点和block,可以看做是同一个文件||
|2|可以通过i节点识别||
|3|不能跨分区||
|4|不能针对目录使用|haha|
|5|相当于一个教室有2个门|haha|
