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



