# python3（requests）使用代理ip

当需要采集大量数据时，或者有的网站对访问速度特别严格的时候，有的网站就采取封ip，这样就需要使用代理ip。就像马蜂窝一样，，自从被曝数据造假之后，就不好爬了，python使用代理ip的小demo为：
其中，如果你爬的为https://www.xxxxx这类那么proxies里面的https内容有效。如果你爬的是http://biggsai.com这种，那么proxies就http有效。

```python
import requests
from bs4 import BeautifulSoup
header={'User-Agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36'}
proxies = {'http': '120.236.128.201:8060',
 'https': '120.236.128.201:8060'
 }
url="http://www.overlove.xin/html/"
header={'User-Agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36'}
req=requests.get(url,headers=header,proxies=proxies,timeout=5)
html=req.text
soup=BeautifulSoup(html,'lxml')
print(soup.text)

```

