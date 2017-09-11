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
* ln -s [原文件 绝对路径] [目标文件 绝对路径]
* 英文翻译:link
* 功能描述:生成链接文件
* 选项: -s 创建软连接
* 硬链接和软连接的对比

|Id|硬链接|软连接|
|:-:|:-|:-:|
|1|拥有相同的i节点和block,可以看做是同一个文件|软链接拥有自己的i节点和block块,只是block中放的是元数据的i节点号和文件名，自己不存储数据|
|2|可以通过i节点识别|删除源文件，软连接不可用|
|3|不能跨分区|Irwxrwxrwx I代表软连接，软连接的权限都是rwxrwxrwx|
|4|不能针对目录使用|修改任意文件，另外一个都改变|
|5|相当于一个教室有2个门|类似window快捷方式|


### 文件搜索命令
* 文件搜索命令 locate **只能搜索文件名**
1.  locate 文件名 在后台数据库中按文件名搜索，速度非常快
2. /var/lib/mlocate 搜索的是这个数据库 **默认是一天一更新**
3.  利用updatedb 立刻更新数据库
4. 按照/etc/updatedb.conf 这个文件配置搜索，参数如下表

|设置参数|解释|
|:-|:-|
|PRUNE_BIND_MOUNTS="yes"|#开启搜索限制|
|PRUNEFS=|#搜索时,不搜索的文件系统|
|PRUNENAMES=|#搜索时,不搜索的文件类型|
|PRUNEPATHS=| 搜索时,不搜索的路径|

* 命令搜索命令 whereis or which

|命令|解释|
|:-|:-|
|whereis|搜索系统命令所在的位置，后面可以加参数 -b or -m ,-b 是只显示命令所在位置，-m是只显示帮助文档所在位置|
|which|搜索命令的位置和别名 **关键是可以看到别名**|
|whoami|显示当前用户 whoami|
|whatis|后面加命令，解释命令的作用 whatis ls|

* linux下执行命令只能用绝对路径,当你输入命令的时候，会在环境变量:
$PATH/usr/lib/qt-3.3/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin 这些目录下面去找

* 文件搜索命令 **find**
1. find [搜索范围] [搜索条件]
2. 非常强大，非常慢
3. example: find / -name install.log
4. find在系统中搜索符合条件的文件名(完全匹配).若要模糊查询需要使用通配符
5. 通配符(要用双引号括起来搜索内容及通配符)
* \* 匹配任意内容
* ?  匹配任意一个单个字符
* [] 匹配任意一个中括号内的字符

|参数|解释|例子|
|:-|:-|:-|
|-name|文件名搜索|find /root -name "*[db]"|
|-iname|不区分大小写|find /root -iname "*[db]"|
|-user|文件所有者|find /root -user root|
|-nouser|查找没有所有者的文件|find /root -nouser|
|-size|根据文件大小|find /root -size 25K|
|-inum|根据文件I节点|find /root -inum 25K|

    注意:没有所有者的文件一般有两种情况，第一种是外来文件，另外一种是系统产生的缓存文件

* 关于参数time的表格 find /var/log/ -mtime +10

|参数|解释|例子|
|:-|:-|:-|
|-10|10天内修改的文件|find /var/log/ -mtime +10|
|10|10天当天修改的文件|find /var/log/ -mtime 10|
|+10|10天前修改的文件|find /var/log/ -mtime -10|
|-mtime|修改文件内容|find /var/log/ -mtime +10|
|-atime|文件的访问时间|find /var/log/ -atime +10|
|-ctime|修改文件属性|find /var/log/ -ctime +10|

* find的复杂操作

|表达式|解释|
|:-|:-|
|find /etc -size +20k -a -size -50K|查找etc目录下面大于20k而且小于50k的文件  -a and 逻辑与  -o or 逻辑或|
|find /etc -size +20k -a -size -50K -exec ls -lh {}\;|查找/etc目录下大于20kb小于50kb的文件,并显示详细信息;-exec/ok 命令 {} \; 对搜索结果执行操作;注意大括号和斜杠之间有空格|

* 字符串搜索命令 grep

* grep [选项] 字符串 文件名 #在文件当中匹配符合条件的字符串
> 选项:
>* -i 忽略大小写
>* -v 排除制定字符串

* find 命令和 grep 命令的区别

1. find命令:在系统中搜索符合条件的文件名,若需匹配,使用通配符匹配,通配符是完全匹配
2. grep命令:在文件中搜索符合条件的字符串,若需匹配,使用正则表达式匹配,正则表达式是包含匹配

### 帮助命令
#### man 命令
获取指定命令的帮助
> man ls
####查看ls的帮助
> man -f 命令相当于whatis 命令
####显示帮助级别,也可以用whereis
> man -k 命令相当于apropos 命令
#### 命令 --help
> ls --help
