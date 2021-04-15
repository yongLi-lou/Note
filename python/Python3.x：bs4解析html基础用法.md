# Python3.x：bs4解析html基础用法

```python
import urllib.request
from bs4 import BeautifulSoup
import re

url = r'http://fund.eastmoney.com/340007.html?spm=search'
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36'}
req = urllib.request.Request(url=url, headers=headers)
res = urllib.request.urlopen(req)
html = res.read().decode('utf-8')

#使用自带的html.parser解析，速度慢但通用
soup = BeautifulSoup(html, "html.parser")
#或者soup = BeautifulSoup(html, "html5lib")
```

```python
#输出第一个 title 标签
print(soup.title)
#输出第一个 title 标签的标签名称
print(soup.title.name)
#输出第一个 title 标签的包含内容
print(soup.title.string)
#输出第一个 title 标签的父标签的标签名称
print(soup.title.parent.name)
```

```python
#输出第一个  p 标签
print(soup.p)
#输出第一个  p 标签的 class 属性内容
print(soup.p['class'])
#输出第一个  a 标签的  href 属性内容
print(soup.a['href'])
#输出第一个  p 标签的所有子节点
print(soup.p.contents)
```

```python
#输出第一个  a 标签
print(soup.a)
#输出所有的  a 标签，以列表形式显示
print(soup.find_all('a'))
```

```python
#输出第一个 id 属性等于  gz_gszze 的标签
print(soup.find(id='gz_gszze'))
#输出第一个 id 属性等于  gz_gszze 的标签的文本内容
print(soup.find(id='gz_gszze').get_text())
```

```python
#获取所有文字内容
print(soup.get_text())
#输出第一个  a 标签的所有属性信息
print(soup.a.attrs)
```

```python
#循环a标签
for link in soup.find_all('a'):
    #获取 link 的  href 属性内容
    print(link.get('href'))

#对soup.p的子节点进行循环输出    
for child in soup.p.children:
    print(child)

#正则匹配，标签名字中带有sp的标签
for tag in soup.find_all(re.compile("sp")):
    print(tag.name)
```

```python
#按照CSS类名搜索tag的功能非常实用,但标识CSS类名的关键字 class 在Python中是保留字,使用 class 做参数会导致语法错误.从Beautiful Soup的4.1.1版本开始,可以通过 class_ 参数搜索有指定CSS类名的tag
#查找dl标签class为dataItem02的所有dl标签
for tag in soup.find_all("dl", class_="dataItem02"):
    print(tag.name)
#或者
for tag in soup.find_all('dl', attrs={'class': "dataItem02"}):
    print(tag.name)
#查找dl标签class为包含'ui-font-'字符的所有dl标签
for tagspan in child.find_all("span", class_=re.compile('ui-font-')):
    print(tagspan.get_text())
```

```python
#数组对象定义（用于存放对象）
content_list = []
#按照CSS类名搜索tag的功能非常实用,但标识CSS类名的关键字 class 在Python中是保留字,使用 class 做参数会导致语法错误.从Beautiful Soup的4.1.1版本开始,可以通过 class_ 参数搜索有指定CSS类名的tag
#查找dl标签class为dataItem02的所有dl标签
for tag in soup.find_all("dl", class_="dataItem02"):
    #对tag的子节点进行循环输出  
    for child in tag.children:
        print(child)
        #将对象存进数组
        content_list.append(child)
#获取数组中的第一个对象的值
print('content_list[0]：'+content_list[0].get_text())
```

find与find_all一起用：

```python
#第一个class = 'postlist'的div里的所有a 标签是我们要找的信息
    #注意：BeautifulSoup()返回的类型是<class 'bs4.BeautifulSoup'>
    #　 　find()返回的类型是<class 'bs4.element.Tag'>
    #　 　find_all()返回的类型是<class 'bs4.element.ResultSet'>
    #　 　<class 'bs4.element.ResultSet'>不能再进项find/find_all操作
    all_a = soup.find('div', class_='postlist').find_all('a', target='_blank')
    for a in all_a:
        title = a.get_text()  # 提取文本
        if(title != ''):
            print("标题：" + title)
#最大页数在span标签中的第10个
pic_max = soup.find_all('span')[10].text

#找标题
title = soup.find('h2',class_='main-title').text
#图片地址在img标签alt属性为'图书'地方
pic_url = mess.find('img',alt = '图书')
#获取pic_url中的src属性值：pic_url['src']
html = requests.get(pic_url['src'],headers = headers)
```

```python
#图片不是文本文件，以二进制格式写入，所以是html.content
#open(路径+文件名,读写模式)
#读写模式:r只读,r+读写,w新建(会覆盖原有文件),a追加,b二进制文件.常用模式
f = open(file_name,'wb')
f.write(html.content)
f.close()
```

```python
#正则 re.findall  的简单用法（返回string中所有与pattern相匹配的全部字串，返回形式为数组），用法：findall(pattern, string, flags=0)
#示例1：查找全部r标识代表后面是正则的语句
str_1 = re.findall(r"com","http://www.cnblogs.com/lizm166/p/8143231.html")
print (str_1)
#输出结果：['com']

#示例2：符号^表示匹配以http开头的的字符串返回,
str_2 = re.findall(r"^http","http://www.cnblogs.com/lizm166/p/8143231.html")
print (str_2)
# 输出结果：['http']

#示例3：用$符号表示以html结尾的字符串返回,判断是否字符串结束的字符串
str_3 = re.findall(r"html$","http://www.cnblogs.com/lizm166/p/8143231.html")
print (str_3)
# 输出结果：['html']

# 示例4：[...]匹配括号中的其中一个字符
str_4 = re.findall(r"[n,w]b","http://www.cnblogs.com/lizm166/p/8143231.html")
print (str_4)
# 输出结果：['nb']

# 示例5：“d”是正则语法规则用来匹配0到9之间的数返回列表
str_5 = re.findall(r"\d","http://www.cnblogs.com/lizm166/p/8143231.html")
str_6 = re.findall(r"\d\d\d","http://www.cnblogs.com/lizm166/p/8143231.html")
print (str_5)
# 输出结果：['1', '6', '6', '8', '1', '4', '3', '2', '3', '1']
print (str_6)
# 输出结果：['166', '814', '323']

# 示例6：小d表示取数字0-9，大D表示不要数字，也就是除了数字以外的内容返回
str_7 = re.findall(r"\D","http://www.cnblogs.com/lizm166/p/8143231.html")
print (str_7)
# 输出结果：['h', 't', 't', 'p', ':', '/', '/', 'w', 'w', 'w', '.', 'c', 'n', 'b', 'l', 'o', 'g', 's', '.', 'c', 'o', 'm', '/', 'l', 'i', 'z', 'm', '/', 'p', '/', '.', 'h', 't', 'm', 'l']

# 示例7：“w”在正则里面代表匹配从小写a到z,大写A到Z，数字0到9
str_8 = re.findall(r"\w","http://www.cnblogs.com/lizm166/p/8143231.html")
print (str_8)
# 输出结果：['h', 't', 't', 'p', 'w', 'w', 'w', 'c', 'n', 'b', 'l', 'o', 'g', 's', 'c', 'o', 'm', 'l', 'i', 'z', 'm', '1', '6', '6', 'p', '8', '1', '4', '3', '2', '3', '1', 'h', 't', 'm', 'l']

# 示例8：“W”在正则里面代表匹配除了字母与数字以外的特殊符号
str_9 = re.findall(r"\W","http://www.cnblogs.com/lizm166/p/8143231.html")
print (str_9)
# 输出结果：[':', '/', '/', '.', '.', '/', '/', '/', '.']
```

```python
# 获取所有a标签（属性target为_blank）
tr.find_all('a',target='_blank')
```

