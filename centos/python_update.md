# centOs python2.6.6升级到2.7.13

第1步：更新gcc，因为gcc版本太老会导致新版本python包编译不成功

复制代码 代码如下:

#yum -y install gcc


系统会自动下载并安装或更新，等它自己结束

第2步：下载Python-3.3.0软件包

复制代码 代码如下:

#wget http://python.org/ftp/python/3.3.0/Python-3.3.0.tar.bz2


注意：按照上述命令下载的软件包会存放在你当前的工作目录下，wget命令是一个从网络上自动下载文件的自由工具，具体用法，请参考这篇文章：http://www.jb51.net/os/RedHat/73089.html

说明：命令中的数字就是版本号，你也可以把3.3.0换成你需要的版本，截止至我撰稿时(2013年1月29日)，最新可用版本是3.3.0

第3步：解压已下载的二进制包并编译安装

复制代码 代码如下:

#tar -jxvf Python-3.3.0.tar.bz2
#cd Python-3.3.0
#./configure
#make all
#make install
#make clean
#make distclean
# /usr/local/bin/python3 –V


编译安装完毕以后，可以输入上面一行命令，查看版本

第4步：建立软连接指向到当前系统默认python命令的bin目录，让系统使用新版本python
#mv /usr/bin/python /usr/bin/python2.4 //当前python的版本为2.4所以是python2.4
#ln -s /usr/local/bin/python3.3 /usr/bin/python
输入#python -V，即可查看当前默认python版本
默认的python成功指向3.3.0以后，yum不能正常使用，需要修改yum的配置文件

第5步：修改yum配置文件

#vi /usr/bin/yum
把文件头部的#!/usr/bin/python改成#!/usr/bin/python2.4 //改为之前的老版本号
保存退出，yum即可正常使用。如若有其他命令、软件不能正常使用，仿照yum配置文件的修改方法，修改其配置文件即可。
至此，更新完毕。
