# 调整站点信息

Admin站点的名称信息也是可以自定义的。

未调整前如下图：

![原始admin站点](/images/origin_site.png)

* **admin.site.site_header**  设置网站页头
* **admin.site.site_title**  设置页面标题
* **admin.site.index_title**  设置首页标语

在booktest/admin.py文件中添加一下信息

```python
from django.contrib import admin

admin.site.site_header = '传智书城'
admin.site.site_title = '传智书城MIS'
admin.site.index_title = '欢迎使用传智书城MIS'
```

刷新网站，效果如下

![调整后的admin站点](/images/modified_admin_site.png)