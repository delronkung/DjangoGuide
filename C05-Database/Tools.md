# 演示工具使用

## 1 shell工具

Django的manage工具提供了**shell**命令，帮助我们配置好当前工程的运行环境（如连接好数据库等），以便可以直接在终端中执行测试python语句。

通过如下命令进入shell

```python
python manage.py shell
```

![django shell](/images/django_shell.png)

导入两个模型类，以便后续使用

```python
from booktest.models import BookInfo, HeroInfo
```

## 2 查看MySQL数据库日志

查看mysql数据库日志可以查看对数据库的操作记录。 mysql日志文件默认没有产生，需要做如下配置：

```shell
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```

![mysql 日志](/images/mysql_log.png)

把68，69行前面的#去除，然后保存并使用如下命令重启mysql服务。

```shell
sudo service mysql restart
```

使用如下命令打开mysql日志文件。

```shell
tail -f /var/log/mysql/mysql.log  # 可以实时查看数据库的日志内容
# 如提示需要sudo权限，执行
# sudo tail -f /var/log/mysql/mysql.log
```

