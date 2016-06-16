# 环境配置

### vim 风格设置

使用vim编辑~/.vimrc文件，将其设置为如下内容

```
set expandtab
set tabstop=4
set shiftwidth=4
set nu
set cursorline
```

### crontab 设置

执行`crontab -e`,设置程序自动执行

```
3,18,33,48 * * * * source ~/.bashrc && cd /home/liudiwei/project/ && sh run.sh>> log/run.out 2>&1; cd -
```
