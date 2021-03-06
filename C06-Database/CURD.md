# 数据库操作—增、删、改、查

## 1 增加

增加数据有两种方法。

**1）save**

通过创建模型类对象，执行对象的save()方法保存到数据库中。

```python
>>> from datetime import date
>>> book = BookInfo(
	btitle='西游记',
    bput_date=date(1988,1,1),
    bread=10,
    bcomment=10
)
>>> book.save()
>>> hero = HeroInfo(
    hname='孙悟空',
    hgender=0,
    hbook=book
)
>>> hero.save()
>>> hero2 = HeroInfo(
    hname='猪八戒',
    hgender=0,
    hbook_id=book.id
)
>>> hero2.save()
```

**2）create**

通过模型类.objects.create()保存。

```python
>>> HeroInfo.objects.create(
	hname='沙悟净',
    hgender=0,
    hbook=book
)
<HeroInfo: 沙悟净>
```

## 2 查询

### 2.1 基本查询

**get** 查询单一结果，如果不存在会抛出**模型类.DoesNotExist**异常。

**all **查询多个结果。

**count** 查询结果数量。

```python
>>> BookInfo.objects.all()
<QuerySet [<BookInfo: 射雕英雄传>, <BookInfo: 天龙八部>, <BookInfo: 笑傲江湖>, <BookInfo: 雪山飞狐>, <BookInfo: 西游记>]>
>>> book = BookInfo.objects.get(btitle='西游记')
>>> book.id
5

>>> BookInfo.objects.get(id=3)
<BookInfo: 笑傲江湖>
>>> BookInfo.objects.get(pk=3)
<BookInfo: 笑傲江湖>
>>> BookInfo.objects.get(id=100)
Traceback (most recent call last):
  File "<console>", line 1, in <module>
  File "/Users/delron/.virtualenv/dj/lib/python3.6/site-packages/django/db/models/manager.py", line 85, in manager_method
    return getattr(self.get_queryset(), name)(*args, **kwargs)
  File "/Users/delron/.virtualenv/dj/lib/python3.6/site-packages/django/db/models/query.py", line 380, in get
    self.model._meta.object_name
db.models.DoesNotExist: BookInfo matching query does not exist.
    
>>> BookInfo.objects.count()
6
```

### 2.2 过滤查询

实现SQL中的where功能，包括

* **filter**  过滤出多个结果
* **exclude** 排除掉符合条件剩下的结果
* **get**  过滤单一结果

对于过滤条件的使用，上述三个方法相同，故仅以**filter**进行讲解。

过滤条件的表达语法如下：

```python
属性名称__比较运算符=值
# 属性名称和比较运算符间使用两个下划线，所以属性名不能包括多个下划线
```

**1）相等**

**exact：表示判等。**

例：查询编号为1的图书。

```
BookInfo.objects.filter(id__exact=1)
可简写为：
BookInfo.objects.filter(id=1)
```

**2）模糊查询**

**contains：是否包含。**

> 说明：如果要包含%无需转义，直接写即可。

例：查询书名包含'传'的图书。

```python
BookInfo.objects.filter(btitle__contains='传')
```

**startswith、endswith：以指定值开头或结尾。**

例：查询书名以'部'结尾的图书

```python
BookInfo.objects.filter(btitle__endswith='部')
```

> 以上运算符都区分大小写，在这些运算符前加上i表示不区分大小写，如iexact、icontains、istartswith、iendswith.

**3） 空查询**

**isnull：是否为null。**

例：查询书名不为空的图书。

```python
BookInfo.objects.filter(btitle__isnull=False)
```

**4） 范围查询**

**in：是否包含在范围内。**

例：查询编号为1或3或5的图书

```python
BookInfo.objects.filter(id__in=[1, 3, 5])
```

**5）比较查询**

* **gt**  大于 (greater then)
* **gte** 大于等于 (greater then equal)
* **lt** 小于 (less then)
* **lte** 小于等于 (less then equal)

例：查询编号大于3的图书

```python
BookInfo.objects.filter(id__gt=3)
```

**不等于的运算符，使用exclude()过滤器。**

例：查询编号不等于3的图书

```python
BookInfo.objects.exclude(id=3)
```

**6）日期查询**

**year、month、day、week_day、hour、minute、second：对日期时间类型的属性进行运算。**

例：查询1980年发表的图书。

```python
BookInfo.objects.filter(bpub_date__year=1980)
```

例：查询1980年1月1日后发表的图书。

```python
BookInfo.objects.filter(bpub_date__gt=date(1990, 1, 1))
```

 #### F对象

之前的查询都是对象的属性与常量值比较，两个属性怎么比较呢？ 答：使用F对象，被定义在django.db.models中。

语法如下：

```
F(属性名)
```

例：查询阅读量大于等于评论量的图书。

```python
from django.db.models import F

BookInfo.objects.filter(bread__gte=F('bcomment'))
```

可以在F对象上使用算数运算。

例：查询阅读量大于2倍评论量的图书。

```python
BookInfo.objects.filter(bread__gt=F('bcomment') * 2)
```

 #### Q对象

**多个过滤器逐个调用表示逻辑与关系，同sql语句中where部分的and关键字。**

例：查询阅读量大于20，并且编号小于3的图书。

```python
BookInfo.objects.filter(bread__gt=20,id__lt=3)
或
BookInfo.objects.filter(bread__gt=20).filter(id__lt=3)
```

**如果需要实现逻辑或or的查询，需要使用Q()对象结合|运算符**，Q对象被义在django.db.models中。

语法如下：

```
Q(属性名__运算符=值)
```

例：查询阅读量大于20的图书，改写为Q对象如下。

```python
from django.db.models import Q

BookInfo.objects.filter(Q(bread__gt=20))
```

Q对象可以使用&、|连接，&表示逻辑与，|表示逻辑或。

例：查询阅读量大于20，或编号小于3的图书，只能使用Q对象实现

```python
BookInfo.objects.filter(Q(bread__gt=20) | Q(pk__lt=3))
```

Q对象前可以使用~操作符，表示非not。

例：查询编号不等于3的图书。

```python
BookInfo.objects.filter(~Q(pk=3))
```

#### 聚合函数

使用aggregate()过滤器调用聚合函数。聚合函数包括：**Avg** 平均，**Count** 数量，**Max** 最大，**Min** 最小，**Sum** 求和，被定义在django.db.models中。

例：查询图书的总阅读量。

```python
from django.db.models import Sum

BookInfo.objects.aggregate(Sum('bread'))
```

注意aggregate的返回值是一个字典类型，格式如下：

```python
  {'属性名__聚合类小写':值}
  如:{'bread__sum':3}
```

使用count时一般不使用aggregate()过滤器。

例：查询图书总数。

```python
BookInfo.objects.count()
```

注意count函数的返回值是一个数字。

### 2.3  排序

使用**order_by**对结果进行排序

```python
BookInfo.objects.all().order_by('bread')  # 升序
BookInfo.objects.all().order_by('-bread')  # 降序
```

### 2.4 关联查询

由一到多的访问语法：

一对应的模型类对象.多对应的模型类名小写_set
例：

```python
b = BookInfo.objects.get(id=1)
b.heroinfo_set.all()
```

由多到一的访问语法:

多对应的模型类对象.多对应的模型类中的关系类属性名
例：

```python
h = HeroInfo.objects.get(id=1)
h.hbook
```

访问一对应的模型类关联对象的id语法:

多对应的模型类对象.关联类属性_id

例：

```python
h = HeroInfo.objects.get(id=1)
h.hbook_id
```

#### 关联过滤查询

**由多模型类条件查询一模型类数据**:

语法如下：

```python
关联模型类名小写__属性名__条件运算符=值
```

**注意：如果没有"__运算符"部分，表示等于。**

例：

查询图书，要求图书英雄为"孙悟空"

```python
BookInfo.objects.filter(heroinfo__hname='孙悟空')
```

查询图书，要求图书中英雄的描述包含"八"

```python
BookInfo.objects.filter(heroinfo__hcomment__contains='八')
```

**由一模型类条件查询多模型类数据**: 

语法如下：

```
一模型类关联属性名__一模型类属性名__条件运算符=值
```

**注意：如果没有"__运算符"部分，表示等于。**

例：

查询书名为“天龙八部”的所有英雄。

```python
HeroInfo.objects.filter(hbook__btitle='天龙八部')
```

查询图书阅读量大于30的所有英雄

```python
HeroInfo.objects.filter(hbook__bread__gt=30)
```

## 3 修改

修改更新有两种方法

**1）save**

**修改模型类对象的属性，然后执行save()方法**

```python
hero = HeroInfo.objects.get(hname='猪八戒')
hero.hname = '猪悟能'
hero.save()
```

**2）update**

**使用模型类.objects.filter().update()**，会返回受影响的行数

```python
HeroInfo.objects.filter(hname='沙悟净').update(hname='沙僧')
```

## 4 删除

删除有两种方法

**1）模型类对象delete**

```python
hero = HeroInfo.objects.get(id=13)
hero.delete()
```

**2）模型类.objects.filter().delete()**

```python
HeroInfo.objects.filter(id=14).delete()
```

