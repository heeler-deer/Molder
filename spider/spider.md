# urllib
## 各种请求
1. get不需要任何参数
2. post需要传入一个列表类型的参数，如：  
````
data=bytes(urllib.parse.urlencode({"hello":"world"}),encoding="utf-8")  
response=urllib.request.urlopen("http://httpbin.org/post",data=data,timeout=1.0)  
print(response.read().decode("utf-8"))  
````
3. 响应头  
418:对方发现你是爬虫 
````
url="https://www.douban.com"
req=urllib.request.Request(url=url,data=data,headers=headers,method="POST")
headers={"User-Agent":"    "}
response=urllib.request.urlopen(req)
print(response.read().decode("utf-8"))
````
# beautifulsoup
## tag以及里面的内容
默认只找到第一个标签的内容
````
from bs4 import BeautifulSoup
file=open("./baidu.html","rb")
html=file.read()
bs=BeautifulSoup(html,"html.parser")
print(bs.title.string)
````
## 整个文档
print(bs)   
print(type(bs.a.string))  
print(bs.a.string)  
## 搜索文档
````
import re
from bs4 import BeautifulSoup
file=open("./baidu.html","rb")
html=file.read()
bs=BeautifulSoup(html,"html.parser")
t_list=bs.find_all("a")
正则表达式
t_list=bs.find_all(re.compile("a"))
函数
def nameisexists(tag):
  return tag.has_attr("name")
t_list=bs.find_all(nameisexists)
for item in t_list:
    print(item)
t_list=bs.find_all(id="head")
#css选择器
选择标签
print(bs.select('title'))
选择类名
t_list=bs.select(".mnav")
选择id
t_list=bs.select('#u1')
选择属性
t_list=bs.select("a[class='bri']")
选择子标签
t_list=bs.select("head>title")
选择兄弟结点
t_list=bs.select(".mnav~.bri")
print(t_list[0].get_text())

````
# 正则表达式
1. 主要语法  
![1](https://github.com/heeler-deer/Molder/blob/main/png/1.png)
![2](https://github.com/heeler-deer/Molder/blob/main/png/2.png)

2. 主要函数  
![3](https://github.com/heeler-deer/Molder/blob/main/png/3.png)
![4](https://github.com/heeler-deer/Molder/blob/main/png/4.png)
````
import re
pat=re.compile("AA")
m=pat.search("ABCAA")
上面简写
m=re.search("AA","bbAAcdc")
re.findall("a","ASDsdaasdasds")
print(re.sub("a","A","abcddsf"))
上面找到a并用A替换

````
# SQLite
````
import sqlite3
conn=sqlite3.connect("test.db")
print("open database successfully")
c=conn.cursor()
获取游标
sql='''
create table company
    (id int primary key not null,
    name text not null,
    age int not null,
    address char(50),
    salary real);
'''

c.execute(sql)
执行SQL
conn.commit()
提交
conn.close()
关闭

````
````
import sqlite3
conn=sqlite3.connect("test.db")
print("open database successfully")
c=conn.cursor()
获取游标
sql='''
insert into company(id,name,age,address,salary)
values(1,'dsa',32,"peking",80000);

'''

c.execute(sql)
执行SQL
conn.commit()
提交
conn.close()
关闭

````
````
import sqlite3
conn=sqlite3.connect("test.db")
print("open database successfully")
c=conn.cursor()
获取游标
sql="select id,name,address,salary from company"


cursor=c.execute(sql)
执行SQL
for row in cursor:
    print("id=",row[0],"\n")
    

conn.close()
关闭
````

# excel
````
import xlwt
workbook=xlwt.Workbook(encoding='utf-8')
worksheet=workbook.add_sheet('sheet1')
worksheet.write(0,0,'hello')
workbook.save('stu.xlsx')
````
# 数据可视化
## flask
一个web框架，核心是werkzeug和jinja2
```
from flask import Flask,render_template
import datetime
app=Flask(__name__)
#路由解析
@app.route('/')
def hello_world():
   return 'hello world!'
@app.route("/")
def index2():
   return render_template("index.html")
#动态显示，需要传递变量
@app.route("/")
def indexx():
    time=datetime.date.today()
    name=["xzhang","xw","xzhao"]

    return render_template("index.html",var=time,list=name)

if __name__=='__main__'
   app.run()
```
## echarts
图表
## wordcloud应用
文字
