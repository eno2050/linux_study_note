# pip 安装

    安装pip真可谓是一波三折，感叹自己真的是太小白了


### 温馨提示：安装的时候预装一下这几个程序

1. yum install openssl -y
2. yum install openssl-devel -y
3. yum install zlib  
4. yum install zlib-devel
5. setuptools


    前两个没有啥问题，关键是最后一个，安装5对3和4有依赖，所以3＆和4，也顺便安装了吧

### 升级pip
> python -m pip install -U pip

### error

* 不安装1和2 ImportError: cannot import name HTTPSHandler
* 不安装3和4 RuntimeError: Compression requires the (missing) zlib module
* eget 安装的时候忽略证书 --no-check-certificate
* example: wget --no-check-certificate source(url)
