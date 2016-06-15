# FAQ


### 1、安装setuptools的时候，找不到zlib

Ans：到Pyhton安装目录中，找到Moudules/Setup文件，编辑该文件，找到zlib行，然后使用自己安装的zlib目录替换$(zlib)；然后在python安装目录执行make clean & make & make install即可。

### 2、使用pip安装redis时，找不到openssl，报错ImportError: cannot import name HTTPSHandle

Ans：同上，到Python安装目录中，执行`vim Modules/Setup`，找到ssl，然后替换其中的$(SSL)为自己安装的openssl目录，如下：

```
_ssl _ssl.c \
        -DUSE_SSL -I/home/liudiwei/software/openssl/include -I/home/liudiwei/software/openssl/include/openssl \
        -L/home/liudiwei/software/openssl/lib -lssl -lcrypto

```
然后执行make clean & make & make install即可。