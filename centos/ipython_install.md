# ipython

    安装ipython的时候要注意版本，新版本只支持3(python)以上，所以2(python)系列需要安装5(ipython)版本

### 下载，解压
* wget http://archive.ipython.org/release/5.1.0/ipython-5.1.0.tar.gz
* tar -xzf ipython-5.1.0.tar.gz
* cd ipython-5.1.0
* python setup.py install

### 运行时提示 ERROR
> No module named traitlets.config.applicaton
然后安装
* pip install traitlets

### 运行提示 ERROR
> No module named pygments
然后安装
* pip install pygments

### 运行提示 ERROR
> No module named pexpect
然后安装
* pip install pexpect

### 运行提示 ERROR
> No module named backports.shutil_get_terminal_size
然后安装
* pip install backports.shutil_get_terminal_size

### 运行提示 ERROR
> No module named pathlib2
然后安装
* pip install pathlib2

### 运行提示 ERROR
> No module named pickleshare
然后安装
* pip install pickleshare

### 运行提示 ERROR
> No module named prompt_toolkit.document
然后安装
* pip install prompt_toolkit
