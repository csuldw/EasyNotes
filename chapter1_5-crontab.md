# Crontab详解

## cron机制

cron可以让系统在指定的时间，去执行某个指定的工作，我们可以使用crontab指令来管理cron机制

## crontab参数

    -u:这个参数可以让我们去编辑其他人的crontab，如果没有加上这个参数的话就会开启自己的crontab
    crontab -u 使用者名称
    -l:可以列出crontab的内容
    -r:可以移除crontab
    -e:可以使用系统预设的编辑器，开启crontab
    -i:可以移除crontab，会跳出系统信息让你再次确定是否移除crontab

## crontab时间格式说明


    minute(分)可以设置0-59分
    hour（小时）可以设置0-23小时
    day of month（日期）可以设置1-31号
    month（月份）：可以设置1-12月
    day of week（星期）：可以设置0-7星期几，其中0和7都代表星期天，或者我们也可以使用名称来表示星期天到星期一，例如sun表示星期天，mon表示星期一等等

## crontab时间格式范例

    1-3表示123
    1-9/2表示13579

## crontab范例

    每五分钟执行  */5 * * * *
    每小时执行    0   * * * *
    每天执行      0   0 * * *
    每周执行      0   0 * * 0
    每月执行      0   0 1 * *
    每年执行      0   0 1 1 *


## 设定cron的权限


    /etc/cron.allow
    /etc/cron.deny


- 系统首先判断是否有`cron.allow`这个文件，如果有这个文件的话，系统会判断这个使用者有没有在`cron.allow`的名单里面，如果在名单里面的话，就可以使用cron机制。如果这个使用者没有在`cron.allow`名单里面的话，就不能使用`cron`机制。
- 如果系统里面没有`cron.allow`这个文件的话，系统会再判断是否有`cron.deny`这个文件，如果有`cron.deny`这个文件的话，就会判断这个使用者有没有在`cron.deny`这个名单里面，如果这个使用者在`cron.deny`名单里面的话，将不能使用cron机制。如果这个使用者没有在`cron.deny`这个名单里面的话就可以使用`cron`机制。
- 如果系统里这两个文件都没有的话，就可以使用`cron`机制

## 介绍crontab文件

        /etc/crontab    #在这个文件里并没有记录系统要执行哪些工作，而是记录了下面四个子目录
        /etc/cron.hourly
        /etc/cron.daily
        /etc/cron.weekly
        /etc/cron.monthly
        
这些子目录里存放了一些脚本，到了crontab所指定的时间点，系统就会去执行这些子目录里的脚本。


### crontab 设置

执行`crontab -e`,设置程序自动执行

```
3,18,33,48 * * * * source ~/.bashrc && cd /home/liudiwei/project/ && sh run.sh>> log/run.out 2>&1; cd -
```

