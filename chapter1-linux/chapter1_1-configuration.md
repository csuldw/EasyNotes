# 环境配置

### vim 风格设置

使用vim编辑~/.vimrc文件，将其设置为如下内容

```text
set expandtab
set tabstop=4
set shiftwidth=4
set nu
set cursorline
```

## 环境搭建

1. WinPython: [https://sourceforge.net/projects/winpython/files/WinPython\_2.7/](https://sourceforge.net/projects/winpython/files/WinPython_2.7/)
2. git
3. Sublime
4. EmEditor
5. cutemarkdown [https://cloose.github.io/CuteMarkEd/](https://cloose.github.io/CuteMarkEd/)
6. Xshell 5
7. python，相关库
   * setuptools
   * pip
   * json/simplejson
   * numpy
   * scipy
   * sklearn
   * matplotlib，需要依赖libpng

```text
./pip install --install-option="--prefix=$home/software/python/python27" simplejson
```

> 尝试使用screen在linux平台开发项目.

