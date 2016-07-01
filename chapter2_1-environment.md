# 环境搭建 - Python及相关库的安装

以前在使用python的时候，都是使用root用户安装好的全局python，现在，因为root用户安装的Python版本太低，同时自己没有root权限去对全局Python升级，所以要在非root用户下安装自己指定的Python。因此，就重新整理了一份如何在Linux环境下使用非root用户安装python及其相关的库，以备不时之需。

## 安装python

python版本库[https://www.python.org/ftp/python/](https://www.python.org/ftp/python/)，此处我选择2.7.5版本的，在安装python的时候，使用`--prefix`指定安装路径即可，命令如下：


```
wget https://www.python.org/ftp/python/2.7.5/Python-2.7.5.tgz
tar -xzf Python-2.7.5.tgz
cd Python-2.7.5
mkdir -p /home/liudiwei/software/python27 
./configure --prefix="/home/liudiwei/software/python27"
make
make install
```


## 安装setuptools

setuptools主要是为安装pip做准备的，下面是从下载到安装的全部命令,使用上面安装的指定路径的python`/home/liudiwei/software/python27/bin/python`进行安装：

```
wget --no-check-certificate http://pypi.python.org/packages/source/s/setuptools/setuptools-2.0.tar.gz
tar -xzvf setuptools-2.0.tar.gz
cd setuptools-2.0
/home/liudiwei/software/python27/bin/python setup.py install
```

## 安装pip

使用pip来安装python相关库，方便简单，此处将`python setup.py install`的python换成自己安装的指定路径下的python`/home/liudiwei/software/python27/bin/python setup.py install`.

```
wget --no-check-certificate https://pypi.python.org/packages/41/27/9a8d24e1b55bd8c85e4d022da2922cb206f183e2d18fee4e320c9547e751/pip-8.1.1.tar.gz#md5=6b86f11841e89c8241d689956ba99ed7
tar -xzf pip-8.1.1.tar.gz
cd pip-8.1.1
/home/liudiwei/software/python27/bin/python setup.py install
```

## 安装相关库

进入python安装目录的bin路径下，安装下面相关库，经测试，下列库均可安装。


- simplejson
- redis
- numpy
- scipy
- sklearn

安装命令：

```
cd /home/liudiwei/software/python27/bin/
./pip install simplejson
./pip install redis
./pip install numpy
./pip install scipy
./pip install sklearn
```

关于matplotlib的安装，因为系统有些依赖包没有安装而导致matplotlib安装失败，如libpng， freetype等，待后续安装完成后，再来完善。


## 附

如果想将自己安装的python使用指定的变量运行，比如使用`mypython`来运行python，只需配置一下`~/.bashrc`文件即可，如下：

```
vim ~/.bashrc
```

在文件末尾追加下列内容:

> alias mypython='/home/liudiwei/software/python27/bin/python'

然后使用`source ~/.bashrc`让该配置生效。接着就可以使用`mypython`进入python控制台了。