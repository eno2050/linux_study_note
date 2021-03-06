# 第一章 系统安装 VMware 创建虚拟机

* 创建虚拟机(标准安装)

> 高级安装和标准安装差不多，这里没有深究

* 选择新的硬盘

> 选择创建一个新的硬盘，不要直接安装镜像，安装镜像的是简化安装

* 卸载以前安装的VMware

> 百度一个VMware_install_cleaner,可以快速卸载残留

* 选择磁盘容量

> 这里选不选没关系，一般只需3-4个G,磁盘是根据你文件的大小来定的

***
# 安装centOs

*  网络适配器


    桥接:采用真实机的网卡,但是要占用局域网的ip,可上网
    NAT模式：采用虚拟出来的VMnet8 这个网卡，可上网
    仅主机模式：采用虚拟出来的VMnet1 这个网卡，不能上网

> 小技巧
>>  善于应用快照
>
>>  虚拟机>管理>克隆
>
>>这个可以复制当前的系统。注意链接克隆和完整克隆。链接克隆的特点是占少量的空间就可以完成和目标主机一样的功能，缺点就是删除目标主机后，他的功能也就消失了。完整克隆就是在复制了一份。缺点：比较耗费硬盘，优点是，自成一体，不随着目标主机的删除而消失



* 系统分区



    主分区:最多只能有4个。硬盘的结构决定的。硬盘被分成了一个一个等大小的扇区，每个扇区是512字节, 其中的一个扇区是主引导扇区，其中446扇区启动信息，剩下的64个扇区记录分区信息，每16个字节可以记录一个扇区，所以就可以分4个
    扩展分区：
      1.最多只能有一个
      2.主分区加扩展分区最多有4个
      3.不能写入数据，只能包含逻辑分区
    逻辑分区：

    格式化：根本目的是为了写入文件系统。又称逻辑格式化，他是根据用户的选定的文件系统，在磁盘的特定区域写入特定的数据，在分区的划出一片用于存放文件分配表、目录表等文件管理的磁盘空间。



### 文件系统 越新的文件系统越先进
* FAT16：最大支持2GB的分区
* FAT32: 单个文件大小不能超过4个G,分区16TB
* NTFS:window 最先进的文件系统,
* EXT2:linux
* EXT3:linux
* EXT4:linux centos 6.3中使用

### 格式化的目的
* 把我们的分区搞成等大小的数据块(block)，默认是4KB
* 分区列表里面建立了I节点，里面记录文件的名字、权限、修改时间、block的id

### 硬件设备文件名,linux一切皆文件
* IDE硬盘:/dev/hd[a-d]
* SCSI/SATA/USB硬盘:/dev/sd[a-p]
* 光驱:/dev/cdrom 或者 /dev/hdc
* 软盘:/dev/fd[0-1]
* 打印机(25针):/dev/lp[0-2]
* 打印机(USB):/dev/usb/lp[0-15]
* 鼠标:/dev/mouse

### 设备文件名
* /dev/hda1 (IDE硬盘接口) 理论速度133M/s 针孔
* /dev/sda1 (SCSI硬盘接口200M/s或者SATA的硬盘接口500M/s)

> 上面的a代表第一块硬盘，b就是第二个硬盘 1代表第一个分区.SCSI的接口就是想小时候玩游戏的卡一样，SATA的是红色的那种

### 挂载
> 必须分区
>> 根分区 /
>
>> swap分区 交换分区，内存2倍，不超过2GB,虚拟内存，生产环境:
> 内存小于4G,分的大小是内存的2倍，大于4个G，保持和内存一样就行了，我们在实验环节，所以不超过2G就ok
>
>> 推荐分区 boot 启动分区 200MB

![linux示意图](../img/pic01.jpg)

### linux 系统安装

* install or upgrade an existing system : 安装或者升级现有系统
* install system with basic video driver: 安装过程采用系统的显卡驱动
* rescue installed system :进入系统修复模式
* boot from local drive : 退出安装从硬盘启动
* Memory test :存储介质检测

### 设置linux ip

>我用的是VMware 安装的虚拟机。配置ip的时候遇到不少问题，基本上都是按着教程上面走的，但后面还是联系不上外网。下面是我总结的一些我遇到的问题

* 在关机状态下，打开VM的虚拟网络编辑器，没有eth0 桥接这个选项，默认是3个，而我只有2个，见下图:

![linux安装](../img/pic02.png)

    解决办法：点击上图红框处的更改设置，然后还原默认设置就出来了
***
* 上面衍生出来的问题

    在你选中桥接后，我们还要设置下面的VMnet的信息，这里必须设置成自己主机可用的网卡
![pic04](../img/pic04.jpg)


* 本人用的是window 10,当发现这个的虚拟网络编辑器，出现2个网卡的时候，我先查到的是修改window服务，具体见下方的百度经验
[没有桥接网卡](http://jingyan.baidu.com/article/af9f5a2d11af4243140a4585.html)


* linux配置ip的文件名


    vi /etc/sysconfig/network-scripts/ifcfg-eth0

* ip配置各个参数的意思,


    * DEVICE=eth0  网卡名
    * HWADDR=00:0c:29:9d:71:56 网卡mac地址
    * TYPE=Ethernet 网络连接类型
    * UUID=1e7af373-9ac0-42d9-9e40-0b7d71bd4c52 唯一标识符
    * ONBOOT=yes **自动连接**
    * NM_CONTROLLED=yes 是否由Network Manager控制该网络接口
    * BOOTPROTO=none **ip分配方式 none static dhcp**
    * IPADDR=192.168.1.241 ip地址
    * NETMASK=255.255.255.0 掩码
    * GATEWAY=192.168.1.1 网关
    * DNS1=192.168.1.1 dns服务器
    * IPV6INIT=no ipv6
    * USERCTL=no 用户权限设置，非root账户不能控制

* ip配置好后，还是连不上。说一下我发现的我的问题：开机状态下，点击VM菜单栏的虚拟机>设置

![pic03.jpg](../img/pic03.jpg)


    我发现这个钩钩没有打上，尝试勾上之后，我这边就正常了



* 修改UUID
1.  vi /etc/sysconfig/network-scripts/ifcfg-eth0 到这个文件先删除MAC地址行
2. rm -rf /etc/udev/rules.d/70-persistent-net.rules 删除这个文件
3. reboot or shutdown -r now 重新启动
