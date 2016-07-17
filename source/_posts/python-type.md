---
title: Python中使用type的使用
toc: true
date: 2016-06-30 01:23:39
tags: python
---

# python中的type类

python中type函数有两种用法，从pydoc中可以看到：

```
type(object) -> the object's type
type(name, bases, dict) -> a new type
```

第一种是返回参数的类型，常常用于在程序中判断参数类型用；第二种用法比较少见，使用来构建一种新的类型，用法跟class比较类似。

在[深刻理解Python中的元类(metaclass)](http://blog.jobbole.com/21351/)一文中，对type给出了一种很深刻的理解。type起始也是一种python类，跟str、int、list、dict类或者我们自己定义的类等一样，这种类有两种构造方法，对应上述的两种type用法。type一个元类，或者说是类的类，也就是说它的实例是一个类，是大部分python内置类型都是type的实例。在python中class关键字构造类的时候，也是使用type来实现的。此文中还介绍到了\_\_metaclass\_\_关键字的用法，这个就比较高端了，以后在讨论。

# 用type实现动态继承

“动态继承”这个词不一定准确，是在我使用flask_sqlalchemy模块进行数据库操作时。sqlalchemy提供了ORM模型，可以使用python中的class来跟数据库中的table对应，进行相关的CRUD操作，但是此时定义的model类需要继承自一个特殊的父类，这个父类需要先创建Flask实例，再从父类中产生包含数据库连接信息的db对象，再从db对象中获取这个特殊的父类（db.Model），这个做法跟面向接口编程的思想是不相符的。我希望在我定义model类的时候并不需要先实例化flask以及指定数据连接，也就是说我可以先定义model类，包括指定字段（对应到数据库表的字段）和成员函数，然后在再创建Flask实例时，让这些类继承自db.Model，实现动态的继承。

当然，还有另一种方法就是从sqlalchemy的Base作为父类产生model类，也是一种比较好的思路的，但这里先不讨论。

使用动态类继承的话，我们的代码就可以这样写：

```python
import models
from new import classobj


def getAllTables(app):
    '''
    app is Flask instance.
    '''
    app.config['SQLALCHEMY_DATABASE_URI'] = DB_URL
    app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = True
    db = SQLAlchemy(app)

    Tables = {}
    for i in models.__dict__.iteritems():
        if type(i[1]) is classobj:
            tablename = i[0]
            Tables[tablename] = type(tablename, (db.Model,), i[1].__dict__)

    return (db, type("Tables", (), Tables))
```

models.py中定义了所有的model类，上述函数的作用是，遍历models.py中的所有类，并使用type构建一个新的类，使后者继承自db.Model。