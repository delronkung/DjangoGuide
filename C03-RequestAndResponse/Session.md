# Session

## 1 启用Session

**Django项目默认启用Session。**

可以在settings.py文件中查看，如图所示

![session中间件](/images/session_middleware.png)

如需禁用session，将上图中的session中间件注释掉即可。

## 2 存储方式

在settings.py文件中，可以设置session数据的存储方式，可以保存在数据库、本地缓存等。

### 2.1 数据库

存储在数据库中，如下设置可以写，也可以不写，**这是默认存储方式**。

```python
SESSION_ENGINE='django.contrib.sessions.backends.db'
```

如果存储在数据库中，需要在项INSTALLED_APPS中安装Session应用。

![session_app](/images/session_app.png)

数据库中的表如图所示

![session数据库](/images/session_database.png)

表结构如下

![session表结构](/images/session_table.png)

由表结构可知，操作Session包括三个数据：键，值，过期时间。

### 2.2 本地缓存

存储在本机内存中，如果丢失则不能找回，比数据库的方式读写更快。

```python
SESSION_ENGINE='django.contrib.sessions.backends.cache'
```

### 2.3 混合存储

优先从本机内存中存取，如果没有则从数据库中存取。

```python
SESSION_ENGINE='django.contrib.sessions.backends.cached_db'
```

### 2.4 Redis

在redis中保存session，需要引入第三方扩展，我们可以使用**django-redis**来解决。

1） 安装扩展

```python
pip install django-redis
```

2）配置

在settings.py文件中做如下设置

```python
CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379/1",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        }
    }
}
SESSION_ENGINE = "django.contrib.sessions.backends.cache"
SESSION_CACHE_ALIAS = "default"
```

#### 注意

 如果redis的ip地址不是本地回环127.0.0.1，而是其他地址，访问Django时，可能出现Redis连接错误，如下：

![redis连接错误](/images/redis_connect_error.png)

解决方法：

修改redis的配置文件，添加特定ip地址。

打开redis的配置文件

```shell
sudo vim /etc/redis/redis.conf
```

在如下配置项进行修改（如要添加10.211.55.5地址）

![修改redis配置文件](/images/modify_redis_config.png)

重新启动redis服务

```shell
sudo service redis-server restart
```

## 3 Session操作

通过HttpRequest对象的session属性进行会话的读写操作。

1） 以键值对的格式写session。

```
request.session['键']=值
```

2）根据键读取值。

```
request.session.get('键',默认值)
```

3）清除所有session，在存储中删除值部分。

```
request.session.clear()
```

4）清除session数据，在存储中删除session的整条数据。

```
request.session.flush()
```

5）删除session中的指定键及值，在存储中只删除某个键及对应的值。

```
del request.session['键']
```

6）设置session的有效期

```
request.session.set_expiry(value)
```

- 如果value是一个整数，session将在value秒没有活动后过期。
- 如果value为0，那么用户session的Cookie将在用户的浏览器关闭时过期。
- 如果value为None，那么session有效期将采用系统默认值，**默认为两周**，可以通过在settings.py中设置**SESSION_COOKIE_AGE**来设置全局默认值。





