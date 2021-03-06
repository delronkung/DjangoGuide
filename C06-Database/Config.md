# 配置

在settings.py中保存了数据库的连接配置信息，Django默认初始配置使用**sqlite**数据库。

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

1. 使用**MySQL**数据库首先需要安装驱动程序

  ```shell
  pip install PyMySQL
  ```

2. 在Django的工程同名子目录的\_\_init\_\_.py文件中添加如下语句

   ```python
   from pymysql import install_as_MySQLdb

   install_as_MySQLdb()
   ```

   作用是让Django的ORM能以mysqldb的方式来调用PyMySQL。

3. 修改**DATABASES**配置信息

   ```python
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.mysql',
           'HOST': '127.0.0.1',  # 数据库主机
           'PORT': 3306,  # 数据库端口
           'USER': 'root',  # 数据库用户名
           'PASSWORD': 'mysql',  # 数据库用户密码
           'NAME': 'django_demo'  # 数据库名字
       }
   }
   ```

4. 在MySQL中创建数据库

   ```mysql
   create database django_demo default charset=utf8;
   ```

   ​