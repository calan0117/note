**几种配置flask的方法**

-----
>[falsk配置项详解](http://www.pythondoc.com/flask/config.html)

之前，做的那个最小应用是在一个127.0.0.1:5000的地址访问flask的应用。这个是由flask默认的。下面就配置一下，从自己要定义的地址和端口访问。
1.最简单的方法-直接在run()中添加参数：
```
app.run(host="192.168.40.7", port=8080)
```
运行起来的时候显示的就是
![](image/2.png)

2.对于flask一些其他的配置参数，可以用传递的方式去设置。
直接设置的方法
```
app.config['DEBUG'] = True
```
或者使用参数文件读取出来然后用变量传递，然后使用app.config()来设置
```
SQLALCHEMY_TRACK_MODIFICATIONS = False # 赋值变量
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = SQLALCHEMY_TRACK_MODIFICATIONS # 配置参数
``` 

3.使用配置文件的方式来将参数传入到flask启动中
    首先我们创建一个config.py文件,在开发和上线是不同配置的，所以我们可能需要多套配置文件。flask提供了一种对象形式的方法，快速便捷。
```
class Config(object):   # 上线配置文件
    pass

class DevConfig(object):    # 开发配置文件
    DEBUG = True
    SQLALCHEMY_DATABASE_URI = 'sqlite:///D:\code-my\data.db'
    SQLALCHEMY_TRACK_MODIFICATIONS = False
```
    
然后在创建的demo.py文件中更改成：
    
```
from flask import Flask
from config import DevConfig #导入配置文件
    
app = Flask(__name__)
app.config.from_object(DevConfig) # 引用配置文件
    
@app.route('/')
    def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    app.run(host='192.168.40.7', port=8080)
```
这样就是使用配置文件来对flask进行配置了。
