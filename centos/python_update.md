# centOs python2.6.6升级到2.7.13

## 第1步：更新gcc，因为gcc版本太老会导致新版本python包编译不成功

> yum -y install gcc

## 第2步：下载Python-2.7.13软件包

> wget http://python.org/ftp/python/2.7.13/Python-2.7.13.tar.bz2


    注意：按照上述命令下载的软件包会存放在你当前的工作目录下，wget命令是一个从网络上自动下载文件的自由工具.说明：命令中的数字就是版本号，你也可以把2.7.13换成你需要的版本

## 第3步：解压已下载的二进制包并编译安装

1. tar -jxvf Python-2.7.13.tar.bz2
2. cd Python-2.7.13
3. ./configure
4. make all
5. make install
6. make clean
7. make distclean
8. /usr/local/bin/python2 –V


编译安装完毕以后，可以输入上面一行命令，查看版本

## 第4步：建立软连接指向到当前系统默认python命令的bin目录，让系统使用新版本python


> mv /usr/bin/python /usr/bin/python2.6.6 //当前python的版本为2.6.6所以是python2.6.6

>ln -s /usr/local/bin/python2.7.13 /usr/bin/python
输入#python -V，即可查看当前默认python版本

    默认的python成功指向2.7.13以后，yum不能正常使用，需要修改yum的配置文件

## 第5步：修改yum配置文件

> vi /usr/bin/yum

    把文件头部的#!/usr/bin/python改成#!/usr/bin/python2.4 //改为之前的老版本号


    保存退出，yum即可正常使用。如若有其他命令、软件不能正常使用，仿照yum配置文件的修改方法，修改其配置文件即可。
至此，更新完毕。
