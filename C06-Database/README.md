# 数据库

##### ORM框架

O是object，也就**类对象**的意思，R是relation，翻译成中文是关系，也就是关系数据库中**数据表**的意思，M是mapping，是**映射**的意思。在ORM框架中，它帮我们把类和数据表进行了一个映射，可以让我们**通过类和类对象就能操作它所对应的表格中的数据**。ORM框架还有一个功能，它可以**根据我们设计的类自动帮我们生成数据库中的表格**，省去了我们自己建表的过程。

django中内嵌了ORM框架，不需要直接面向数据库编程，而是定义模型类，通过模型类和对象完成数据表的增删改查操作。

使用django进行数据库开发的步骤如下：

1. 配置数据库连接信息
2. 在models.py中定义模型类
3. 迁移
4. 通过类和对象完成数据增删改查操作

**ORM作用**

![orm](/images/orm.png)

![orm](/images/orm2.png)