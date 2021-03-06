# Ubuntu下进程守护-supervisor

这是一个进程管理工具，简单话说就是你守护的进程如果关闭了他会自动帮你开起来。

这是之前用过的一个技术，感觉以后还会用到就趁着没忘干净总结一下，所以就不一步步截图记录了。

## 安装

```SHELL
$ sudo apt-get install supervisor
```



安装成功后`supervisor`会自动启动的。



## 创建一个要守护的进程



在`/etc/supervisor/conf.d`目录下创建一个配置文件

例如我创建的一个`workerman.conf`

用于守护基于**Laravel**的**Workerman**服务



## 修改配置文件

```shell
$ sudo vim workerman.conf

# 添加以下内容
[program:workerman]
command=/usr/bin/php /www/wwwroot/site/artisan workerman:command start d
autostart=true
autorestart=true

# 首先名字一定要对应上
# command是要守护的脚本
# 因为不确定自动执行时目录所以干脆都写了绝对路径
# php绝对路径可以通过 whereis php 获取
# 后面的artisan命令是自定义的
# 这段命令其实就是 php artisan workerman:command start d
# 配置文件不止这些 根据情况加吧
```



## 如何守护



```shell
# 对配置文件或者脚本文件有修改一定要重启,重新加载配置文件
supervisorctl reload
# 可以这样启动配置需要守护的脚本 (启动所有)
supervisorctl start all
# 启动指定脚本
supervisorctl start workerman
# 重启 关闭同理 stop / restart

# 查看脚本状态
supervisorctl status
```



## 其他

`/var/log/supervisor/supervisord.log` 可以在这获取到supervisor的日志





## 可能遇到问题

* workerman: ERROR (spawn error)

  实测是脚本有问题，尝试下直接执行脚本文件有没有问题。