# 调整列表页展示

#### 1  页大小

每页中显示多少条数据，默认为每页显示100条数据，属性如下：

```python
list_per_page=100
```

1）打开booktest/admin.py文件，修改AreaAdmin类如下：

```python
class BookInfoAdmin(admin.ModelAdmin):
    list_per_page = 2
```

2）在浏览器中查看区域信息的列表页面，效果如下图：

![列表页选项](/images/list_option.png)

#### 2  "操作选项"的位置

顶部显示的属性，设置为True在顶部显示，设置为False不在顶部显示，默认为True。

```python
actions_on_top=True
```

底部显示的属性，设置为True在底部显示，设置为False不在底部显示，默认为False。

```python
actions_on_bottom=False
```

1）打开booktest/admin.py文件，修改BookInfoAdmin类如下：

```python
class BookInfoAdmin(admin.ModelAdmin):
    ...
    actions_on_top = True
    actions_on_bottom = True
```

2）在浏览器中刷新效果如下图：

![列表页选项](/images/list_option_top.png)

#### 3  列表中的列

属性如下：

```
list_display=[模型字段1,模型字段2,...]
```

1）打开booktest/admin.py文件，修改BookInfoAdmin类如下：

```python
class BookInfoAdmin(admin.ModelAdmin):
    ...
    list_display = ['id','btitle']
```

2）在浏览器中刷新效果如下图：

![列表页选项](/images/list_option_col.png)

**点击列头可以进行升序或降序排列。**

#### 4  将方法作为列

列可以是模型字段，还可以是模型方法，要求方法有返回值。

**通过设置short_description属性，可以设置在admin站点中显示的列名。**

1）打开booktest/models.py文件，修改BookInfo类如下：

```python
class BookInfo(models.Model):
    ...
    def pub_date(self):
        return self.bpub_date.strftime('%Y年%m月%d日')

    pub_date.short_description = '发布日期'  # 设置方法字段在admin中显示的标题
```

2）打开booktest/admin.py文件，修改BookInfoAdmin类如下：

```python
class BookInfoAdmin(admin.ModelAdmin):
    ...
    list_display = ['id','atitle','pub_date']
```

3）在浏览器中刷新效果如下图：

![列表页选项](/images/list_option_method_col.png)

方法列是不能排序的，如果需要排序需要为方法指定排序依据。

```
admin_order_field=模型类字段
```

1）打开booktest/models.py文件，修改BookInfo类如下：

```python
class BookInfo(models.Model):
    ...
    def pub_date(self):
        return self.bpub_date.strftime('%Y年%m月%d日')

    pub_date.short_description = '发布日期'
    pub_date.admin_order_field = 'bpub_date'
```

2）在浏览器中刷新效果如下图：

![列表页选项](/images/admin_order_filed.png)

#### 5  关联对象

无法直接访问关联对象的属性或方法，可以在模型类中封装方法，访问关联对象的成员。

1）打开booktest/models.py文件，修改HeroInfo类如下：

```python
class HeroInfo(models.Model):
    ...
    def read(self):
        return self.hbook.bread

    read.short_description = '图书阅读量'
```

2）打开booktest/admin.py文件，修改HeroInfoAdmin类如下：

```python
class HeroInfoAdmin(admin.ModelAdmin):
    ...
    list_display = ['id', 'hname', 'hbook', 'read']
```

3）在浏览器中刷新效果如下图：

![列表页选项](/images/list_relate_obj.png)

#### 6  右侧栏过滤器

属性如下，只能接收字段，会将对应字段的值列出来，用于快速过滤。一般用于有重复值的字段。

```
list_filter=[]
```

1）打开booktest/admin.py文件，修改HeroInfoAdmin类如下：

```python
class HeroInfoAdmin(admin.ModelAdmin):
    ...
    list_filter = ['hbook', 'hgender']
```

2）在浏览器中刷新效果如下图：

![列表页选项](/images/admin_filter.png)

#### 7  搜索框

属性如下，用于对指定字段的值进行搜索，支持模糊查询。列表类型，表示在这些字段上进行搜索。

```
search_fields=[]
```

1）打开booktest/admin.py文件，修改HeroInfoAdmin类如下：

```python
class HeroInfoAdmin(admin.ModelAdmin):
    ...
    search_fields = ['hname']
```

2）在浏览器中刷新效果如下图：

![列表页选项](/images/list_search.png)

