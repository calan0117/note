**做一个简单的爬虫**

爬虫技术是一个获取信息和数据的重要手段。学习用python做一个简单的爬虫。    
爬虫主要分为两个部分：    
* 获取数据      
    1. urllib 内建模块(url.request)
    2. Requests 第三方库
    3. Scrapy框架
    4. 第三方的API
* 解析数据
    1. BeautifulSoup库
    2. re模块

一.从网络上取的数据  
简单的爬虫是可以使用[Requests](www.python-requests.org)库来完成的。    
Requests的基本使用方法     
requests.get()用来请求指定URL位置的资源，对应的是HTTP协议的GET方法。 


爬虫有各式各样的，做个简单的来玩耍一下，下面以爬豆瓣读书上《利用Python进行数据分析》这本书的的[书评](https://book.douban.com/subject/25779298/comments/)作为例子。   
1. 最简单的爬回来一个页面
```
import requests

r = requests.get('https://book.douban.com/subject/25779298/comments/')
with open(r'D:\demo.txt', 'w') as file:
    file.writelines(r.text)
```
使用requests模块的get方法，从网站上把一整个页面全都保存到了本地的D盘中的demo.txt文件中。这是一个极度简单的爬虫。其中requests本身自己还有许多属性和方法，可以去官网参考学习。如果需要把爬虫收集回来的数据保存到本地的话，还需要去了解一下文件操作和数据库操作的知识。

2.  去取回一个图片到本地保存
```
import requests

r = requests.get('https://www.baidu.com/img/bd_logo1.png')
with open('c:\\test\\baidu.png', 'wb') as fp:
    fp.write(r.content)
	
```

3. 带头部验证的爬虫
```
import requests

re = requests.get('https://www.zhihu.com')
re.status_code
```
直接请求知乎，返回的结果是500，证明没有请求成功。  
```
import requests

headers = {"User-Agent": "Mozilla/5.0 (X11; Linux i686) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/17.0.963.83 Safari/535.11"}
re = requests.get('https://www.zhihu.com', headers = headers)
re.status_code
```
输出的结果是200，证明成功的返回了数据。  
二.把数据解析出来  
1. 一般的标签网页解析--[BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html)
>安装的时候要注意：Pyhton3要安装BeautifulSoup4才是正常使用的。 

打开刚才生成的demo.txt文件，会发现文件中保存的是一整个页面的代码，内容十分复杂，所以需要去解析一下页面的内容，才能够把我们需要的短评抽取出来。我们先研究一下。  
打开文件，或者在网页页面按F12进入调试模式会发现，短评基本上是在一个相同的class的p标签里面的
```
<p class="comment-content">入门书，零基础看了这本书也能用python的pandas和matplotlib进行一些简单的数据分析，数据分析不在乎用什么工具，而是有目的地去找一y些insight，下一步我需要达到的效果是：如果产生一个想法，能用工具快速验证（如数据预处理，绘出图标等）。</p>
        
```
引入beautifulsoup来解析一下。  
>后面要使用到的lxml解析包同样是需要安装的。

修改一下刚才的极简爬虫：
```
import requests
from bs4 import BeautifulSoup

r = requests.get('https://book.douban.com/subject/25779298/comments/')
bs = BeautifulSoup(r.text, 'lxml')
comments = bs.find_all('p', 'comment-content')

with open(r'D:\demo1.txt', 'w') as file:
    for item in comments:
        file.writelines(item.string)

```
激动的打开了demo.txt 文件后，发现天呐撸，所有的评论都窝在一行了，我们要给他区分出来，并且换行，再简单也要有点可读性。
修改一下刚才的小爬虫：
```
file.writelines(item.string + '\n')
```
只要写文件的时候加入这么一个简单的换行符，就可以把每个评论换行来看了，而不是挤成一坨。  

2. 复杂细节解析--正则表达式  
玩玩爬完文字评论，可以发现，豆瓣本身还是有一个评星星等级的一个评价，把这个也爬下来作为一个数据收集起来。打开网页，F12找到这个评星星会发现，这个直接读取标签内的内容不一样，它是写在标签的class里面的。例如下面的五星推荐 是 allstar50,而一个四星评价是 allstar40。
```
<span class="user-stars allstar50 rating" title="力荐"></span>

<span class="user-stars allstar40 rating" title="推荐"></span>
```
这里就需要使用正则表达式来提取这种复杂的情况。因为不是所有人都有打分，把有分数都拿出来，然后给算一个平均值作为参考。
再让小爬虫变身一下：
```
import requests
import re
from bs4 import BeautifulSoup

# 得到评论正文
r = requests.get('https://book.douban.com/subject/25779298/comments/')
bs = BeautifulSoup(r.text, 'lxml')
comments = bs.find_all('p', 'comment-content')

# 得到评论的评分并计算总分和平均分
rule = re.compile('<span class="user-stars allstar([0-9]*?) rating"') # 配置匹配规则
comments_star = re.findall(rule, r.text)
totalsocre = 0
for star in comments_star:
    totalsocre += int(star)
avgsocre = totalsocre / len(comments_star)

with open(r'D:\demo1.txt', 'w') as file:
    file.write('此书的评价平均分(满分50):  ' + str(avgsocre) + '\n')
    for item in comments:
        file.write(item.string + '\n')

```
allstar([0-9]*?)中的[0-9]*?是正则表达式，这个需要额外的认真学习。

以上，完成了一个极简的爬虫。网络数据千千万，这个爬虫顶多算是蜉蝣，难以撼动各种。不过爬虫是一个强大的数据收集手段，值得与深入的学习。于此与各位学习pyhton的人们共勉。