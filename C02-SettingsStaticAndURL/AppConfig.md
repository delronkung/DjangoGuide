# App应用配置

在每个应用目录中都包含了apps.py文件，用于保存该应用的相关信息。

在创建应用时，Django会向apps.py文件中写入一个该应用的配置类，如

```python
from django.apps import AppConfig

class UsersConfig(AppConfig):
    name = 'users'
```

我们将此类添加到工程settings.py中的INSTALLED_APPS列表中，表明注册安装具备此配置属性的应用。

* **AppConfig.name** 属性表示这个配置类是加载到哪个应用的，每个配置类必须包含此属性，默认自动生成。

* **AppConfig.verbose_name** 属性用于设置该应用的直观可读的名字，此名字在Django提供的Admin管理站点中会显示，如

  ```
  from django.apps import AppConfig

  class UsersConfig(AppConfig):
      name = 'users'
      verbose_name = '用户管理'
  ```

  ​



