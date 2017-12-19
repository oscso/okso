# Flask. 1 helloworld

## \#一级标题

二级标题

```
你好
```

```py
#-*-encoding:utf-8-*-
#从flask框架中导入Flask类
from flask import Flask

#初始化一个Flask对象
#Flask（）
#需要传递一个参数__name__
#1.方便flask框架去寻找资源
#2.方便flask插件比如Flask-Sqlalchemy出现错误的时候，好去寻找问题所在的位置
app = Flask(__name__)

#@app.route是一个装饰器
#@开头，并且在函数的上面，说明是装饰器
#这个装饰器的作用是，做一个url与是图函数的映射
#127.0.0.5000/  ---> 去请求hello_world这个函数，然后将结果返回给浏览器
@app.route("/")
def hello_world():
    return "<h1>这个第一个flask程序</h1>"


#如果当前这个文件是作为入口程序运行，那么就执行app.run()
if __name__ == "__main__":
    #app.run()
    #启动一个应用服务器，来接受用户的请求
    #while True：
    #    listen() 一直在监听用户请求，死循环监听
    app.run()
```

## if

页面代码：

```
    {% if  user and user.age > 18 %}
        <a href = #>{{ user.username }}</a>
        <a href = #>注销</a>
    {% else %}
        <a href = #>登陆</a>
        <a href = #>注册</a>
    {% endif %}
```

后台代码：

```
@app.route('/<is_login>')
def index(is_login):

    if is_login == '1':
        user = {
            'username': '王二麻子',
            'age': 18
        }

        return render_template('index.html',user=user)
    else:
        user = {
            'username': '江湖小子',
            'age': 32
        }
        return  render_template('index.html',user=user)
```

## for

页面代码

```
    {% for k,v in user.items() %} #遍历字典
        <span>{{ k }} : {{ v }}</span><br/>
    {% endfor %}

    {% for website in website %} #遍历列表
        <span>{{ website }}</span>
    {% endfor %}
```

后台代码

```
@app.route('/<is_login>')
def index(is_login):

    if is_login == '1':
        #字典
        user = {
            'username': '王二麻子',
            'age': 18
        }
        #列表
        websites = ['www.baidu.com','www.google.com']

        return render_template('index.html',user = user,website = websites)
```

### 案列

for循环展示4本书

前台代码

```
<table border="1">
    <thead>
        <th>书名</th>
        <th>作者</th>
        <th>价格</th>
    </thead>
    <tbody>
        {% for book in books %}
            <tr>
                <td>{{ book.name }}</td>
                <td>{{ book.author }}</td>
                <td>{{ book.price }}</td>
            </tr>
        {% endfor %}
    </tbody>
</table>
```

后台代码

```
@app.route('/')
def index():
    books = [
        {
            'name':'红楼梦',
            'author':'曹雪芹',
            'price':109
        },

        {
            'name':'水浒传',
            'author':'施耐庵',
            'price':188
        },

        {
            'name':'西游记',
            'author':'吴承恩',
            'price':200
        },

        {
            'name':'三国演义',
            'author':'罗贯中',
            'price':260
        }


    ]

    return render_template('index.html',books=books)
```

## 继承

```
@app.route('/')
def index():
    class Person(object): #定义一个persion类
        name = '张三'
        age = 18

    class Stu(Person): #继承自Persion类
        pass

    stu = Stu() #实例化一个对象
    return render_template('index.html',stu=stu) #把对象一块传去
```

继承的也面代码：

```
 <td>{{ stu.name }}</td>
 <td>{{ stu.age }}</td>
```

继承的显示效果

![](/assets/import.png)

## 页面继承

#### base.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}{% endblock %}</title>
</head>


<body>
<table border="1">
    <thead>
        <th>书名</th>
        <th>作者</th>
        <th>价格</th>
    </thead>
    <tbody>
            <tr>
                <td>王站撒五</td>
                <td>价格</td>
                <td>8</td>
            </tr>
    </tbody>
</table>

{% block one %}{% endblock %}

</body>
</html>
```

#### index.html

```
{% extends 'base.html' %} #继承自base.html

{% block one %} #添加块内容
    <h1 align = 'center'>你是这个世界上的最好的人</h1>
{% endblock %}

{% block title %} #块开始
    这是首页
{% endblock %} #块结束
```

## 网页链接地址翻转：

后台代码

```
@app.route('/loginXXAAS/') #这里的路径可以任意变换不影响
def login():
    return render_template('login.html')
```

前台翻转代码

这的login对应上面的login\(\)函数

```
<a href="{{ url_for('login') }}">登录</a>
```

## 加载静态文件

```
<link rel="stylesheet" href="{{ url_for('static',filename='css/index.css') }}">
<img src="{{ url_for('static',filename='images/abc.jpg') }}">
<scripty src="{{ url_for('static',filename='js/index.js') }}">
```

## 数据库连接

config.py  数据库配置信息

```
#dialect+driver://username:password@host:port/database

DIALECT = 'mysql'
DRIVER = 'mysqldb'
USERNAME = 'root'
PASSWORD = 'root'
HOST = '127.0.0.1'
PORT = '3306'
DATABASE = 'db_demo1'

SQLALCHEMY_DATABASE_URI = "{}+{}://{}:{}@{}:{}/{}?charset=utf8".format(DIALECT,DRIVER,USERNAME,PASSWORD,HOST,PORT,DATABASE)

SQLALCHEMY_TRACK_MODIFICATIONS = False
```

数据库连接成功与否测试condb.py

```
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
import config

app = Flask(__name__)
app.config.from_object(config)
db = SQLAlchemy(app)

db.create_all()

@app.route('/')
def index():
    return 'Hello World!'


if __name__ == '__main__':
    app.run()
```

测试结果如下表示成功

```
D:\PVenv\P27\Scripts\python.exe D:/ppy/dbcon/dbcon.py
D:\PVenv\P27\lib\site-packages\sqlalchemy\engine\default.py:470: Warning: Incorrect string value: '\xD6\xD0\xB9\xFA\xB1\xEA...' for column 'VARIABLE_VALUE' at row 480
  cursor.execute(statement, parameters)
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
127.0.0.1 - - [13/Dec/2017 13:04:42] "GET / HTTP/1.1" 200 -
```

## 创建表

SQL语句

```
 #article表对应的平时用的SQL语句
 create table article(
     id int primary key autoincrement,
     title varchar(100) not null,
     content text not null
```

对应的ORM模型关系映射

```
class Article(db.Model):
    __tablename__ = 'article'  #表名article
    id = db.Column(db.Integer,primary_key=True,autoincrement=True)
    title = db.Column(db.String(100),nullable=False)
    content = db.Column(db.Text,nullable=False)

db.create_all() #这一步是执行上面的操作
```

## 增加插入数据

```
@app.route('/')
def index():
    article1 = Article(title = 'aaa', content = 'bbb') #一条数据
    db.session.add(article1) #事务 会话增加数据 
    db.session.commit()  #提交
```

## 查询数据

```
    #select *from article where title='aaa'  #这是对应的SQL语句
    result = Article.query.filter(Article.title == 'aaa').all()
    article1 =  result[0]
    print article1.title
    print article1.content
```

查询数据2

```
result = Article.query.filter(Article.title == 'aaa').first()
print  'title: %s' %result.title
print  'content: %s' %result.content
```

## 修改数据

```
    # 改
    # 1.先把你要更改的数据查询出来
    # article1 = Article.query.filter(Article.title == 'onetitle').first()
    # 2. 把这条数据，你需要修改的地方进行修改。
    # article1.title = 'new title'
    # 3. 做实务提交
    # db.session.commit()
```

## 删除记录

```
    #删除
    #1.先把要删除的数据查询出来
    article1 = Article.query.filter(Article.content == 'bbb').first()
    #2. 把这条数据删除掉
    db.session.delete(article1)
    #3. 做实务提交
    db.session.commit()
```

## SQLAlchemy 外键和关系映射

页面

```
#-*-encoding:utf8 -*-
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
import config


app = Flask(__name__)
app.config.from_object(config)
db = SQLAlchemy(app)

# #用户表
# create table users (
#     id int primary key autoincrement,
#     username varchar(100) not null
# )
#
# #文章表
# create table article (
#     id int primary key autoincrement,
#     title varchar(100) not null,
#     content text not null
#     author_id int,
#     foreign key 'aut' references 'users.id'  #关联外键
# )

class User(db.Model):
    __tablename__ = 'user'
    id = db.Column(db.Integer,primary_key=True,autoincrement=True)
    username = db.Column(db.String(100),nullable=False)
    password = db.Column(db.String(100),nullable=False)


class Article(db.Model):
    __tablename__ = 'article'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    title = db.Column(db.String(100),nullable=False)
    content = db.Column(db.Text,nullable=False)
    author_id = db.Column(db.Integer,db.ForeignKey('user.id')) #创建并关联外键

db.create_all()


@app.route('/')
def index():
    return 'Hello World! 这是首页'

if __name__ == '__main__':
    app.run()
```

config文件的另外一种方式

```
#dialect+driver://username:password@host:port/database

HOSTNAME = '127.0.0.1'
PORT = '3306'
#DIALECT = 'mysql'
#DRIVER = 'mysqldb'
USERNAME = 'root'
PASSWORD = 'root'
DATABASE = 'db_demo3'

#SQLALCHEMY_DATABASE_URI = "{}+{}://{}:{}@{}:{}/{}?charset=utf8".format(DIALECT,DRIVER,USERNAME,PASSWORD,HOST,PORT,DATABASE)

DB_URI = 'mysql+mysqldb://{}:{}@{}:{}/{}'.format(USERNAME,PASSWORD,HOSTNAME,PORT,DATABASE)
SQLALCHEMY_DATABASE_URI = DB_URI
SQLALCHEMY_TRACK_MODIFICATIONS = True
```

定义创建关系

```
class User(db.Model):
    __tablename__ = 'user'
    id = db.Column(db.Integer,primary_key=True,autoincrement=True)
    username = db.Column(db.String(100),nullable=False)


class Article(db.Model):
    __tablename__ = 'article'
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    title = db.Column(db.String(100),nullable=False)
    content = db.Column(db.Text,nullable=False)

    author_id = db.Column(db.Integer,db.ForeignKey('user.id')) #创建并关联外键

    author = db.relationship('User',backref = db.backref('articles')) #创建一个反向引用的关系

db.create_all()
```

通过模型创建数据

```
    # #想要添加一篇文章，因为文章必须依赖而存在,所以得先添加一个用户。
    # user1 = User(username = "zhiliao")
    # db.session.add(user1)
    # db.session.commit()

    #添加文章
    # article = Article(title = 'aaa',content = 'bbb',author_id = 1)
    # db.session.add(article)
    # db.session.commit()

    #找出文章标题为aaa的作者原始方法
    # article = Article.query.filter(Article.title == 'aaa').first()
    # author_id = article.author_id
    # user = User.query.filter(User.id == author_id).first()
    # print  "username:  %s" % user.username

    #定义了一种反向引用的关系后，另外一种添加数据的方法
    # article = Article(title = 'aaa',content = 'bbb')
    # article.author = User.query.filter(User.id == 1).first()  #通过上面定义的关系添加数据
    # db.session.add(article)
    # db.session.commit()

    #找出文章标题为aaa的改善方法
    # article= Article.query.filter(Article.title == 'aaa').first()
    # print 'username: %s' % article.author.username

    #在添加一篇文章
    # article = Article(title = '111', content = '222', author_id = '1')
    # db.session.add(article)
    # db.session.commit()

    #找到zhiliao用户写过的所有文章,这才是真正的ORM
    user = User.query.filter(User.username == 'zhiliao').first()
    result = user.articles
    for article in result:
        print '-' * 10
        print article.title
```



