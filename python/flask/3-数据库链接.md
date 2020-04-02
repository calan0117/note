**连接和操作数据库的方法**

-----

* 使用数据库驱动直接连接,使用oracle数据库做示例：
    做一个应用或者建站总是要使用不同的数据库或者其他方式来存贮我们的数据。
    首先要先下载python中支持连接oralce的包cx_Oracle，可以下载安装。
    >[cx_Oracle下载](https://pypi.python.org/pypi/cx_Oracle)

    或者直接在cmd使用命令
    
    ```
    pip install cx_Oracle xxxxxx # xxxx是你的版本号
    ```
    **1.要注意的是不同版本的oralce数据库使用的包的版本是不同的**  
    **2.windows系统下可能要你先安装C++环境**  
    安装完成后，我们创建一个文件，简单测试一下。(创建一个database_link.py)
    ```
    import cx_Oracle

    conn = cx_Oracle.connect('dw_hos/dw_hos@localhost/orcl')

    c = conn.cursor()

    x = c.execute('select sysdate from dual')

    rows = x.fetchone()

    c.close()

    conn.close()

    print(rows)
    ```
    在控制台就可以看到查询回来的数据库时间了。  
    ![](image/3.png)  
    link的方式增删查改都是使用的SQL语句，然后创建oralce的cursor来执行，执行后关闭。
* 使用Flask-SQLAlchemy管理。
    >[Flask-SQLAlchemy官网](http://flask-sqlalchemy.pocoo.org/2.1/)    
    >[Flask-SQLAlchemy文档中文翻译](http://www.pythondoc.com/flask-sqlalchemy/index.html)

    首先给出flask各类数据库的连接字符串格式。格式如下
    ```
    dialect+driver://username:password@host:port/database
    ```
    再举出几个常用的例子,其他的可以在官方文档中查阅：
    ```
    mysql://scott:tiger@localhost/mydatabase # MYSQL
    oracle://scott:tiger@127.0.0.1:1521/sidname # ORACLE
    sqlite:////absolute/path/to/foo.db # SQLLITE
    ```
    这里我用的是oracle数据库：
    创建一个简单的demo来运行一下(创建一个database_orm.py)：
    ```
    from flask import Flask
    from flask.ext.sqlalchemy import SQLAlchemy

    app = Flask(__name__)
    app.config['SQLALCHEMY_DATABASE_URI'] = 'oracle://dw_hos:dw_hos@localhost:1521/orcl'
    db = SQLAlchemy(app)

    # 下面是数据模型的声明
    class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True)
    email = db.Column(db.String(120), unique=True)

    def __init__(self, username, email):
        self.username = username
        self.email = email

    def __repr__(self):
        return '<User %r>' % self.username
    ```
    从python shell中进入db对象并调用SQLAlchemy.create_all() 方法来创建表和数据库:
    ```
    >>> from database_orm import db
    >>> db.create_all()
    ```
    这时候是有了数据表格了，创建一些对象(插入一些数据)
    ```
    >>> from yourapplication import User
    >>> admin = User('admin', 'admin@example.com')
    >>> guest = User('guest', 'guest@example.com')
    ```
    这时候还没插入数据，要将之插入数据库
    ```
    >>> db.session.add(admin)
    >>> db.session.add(guest)
    >>> db.session.commit()
    ```
    查询也是非常简单的
    ```
    >>> users = User.query.all()
    [<User u'admin'>, <User u'guest'>]
    >>> admin = User.query.filter_by(username='admin').first()
    <User u'admin'>
    ```
以上就是两种对数据库连接和操作的方式。其中，使用flask-sqlalchemy的时候只要去变更连接串，就可以实现对不同数据库的操作管理。
