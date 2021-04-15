# Python标准库—urllib和urllib3

一、urllib
urllib是Python中请求url连接的官方标准库，在Python2中主要为urllib和urllib2，在Python3中整合成了urllib。urllib中一共有四个模块，分别如下：

 request：主要负责构造和发起网络请求,定义了适用于在各种复杂情况下打开 URL (主要为 HTTP) 的函数和类
error：处理异常
parse：解析各种数据格式
robotparser：解析robot.txt文件
1. urllib.request模块
模块中最常用的函数为urllib.request.urlopen()，参数如下：

urllib.request.urlopen(url, data=None, [timeout, ]*, cafile=None, capath=None, cadefault=False, context=None)
 url:  请求的网址
 data：要发送到服务器的数据
 timeout：设置网站的访问超时时间
urlopen返回对象提供方法：

read() , readline() ,readlines() , fileno() , close() ：对HTTPResponse类型数据进行操作
info()：返回HTTPMessage对象，表示远程服务器返回的头信息
getcode()：返回Http状态码。如果是http请求，200请求成功完成 ; 404网址未找到
geturl()：返回请求的url
（1）发送简单的GET请求：

from urllib import request

response = request.urlopen('https://www.baidu.com')
print(response.read().decode())
在urlopen()方法中，直接写入要访问的url地址字符串，该方法就会主动的访问目标网址，然后返回访问结果，返回的访问结果是一个    http.client.HTTPResponse对象，使用该对象的read()方法即可获取访问网页获取的数据，这个数据是二进制格式的，所以我们还需要使用decode()方法来将获取的二进制数据进行解码，转换成我们可以看的懂得字符串。

（2）发送简单的POST请求：

from urllib import reuqest

response = request.urlopen('http://httpbin.org/post', data=b'word=hello')

print(response.read().decode())   #decode()解码
在urlopen()方法中，urlopen()默认的访问方式是GET，当在urlopen()方法中传入data参数时，则会发起POST请求。注意：传递的data数据需要为bytes格式

（3）复杂的请求——请求头

当我们需要模拟一些其他的参数的时候，简单的urlopen() 方法已经无法满足我们的需求了，这个时候我们就需要使用urllib.request中的Request对象来帮助我们实现一些其它参数的模拟，比如请求头。

Request对象如下所示：

# Request对象实例化
req  = urllib.request.Request(url, data=None, headers={},origin_req_host=None,unverifiable=False, method=None)
举例如下： 

  from urllib import request

  url = 'http://httpbin.org/get'
  headers = {'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_5) \
AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.181 Safari/537.36'}

  # 需要使用url和headers生成一个Request对象，然后将其传入urlopen方法中
  req = request.Request(url, headers=headers)
  resp = request.urlopen(req)
  print(resp.read().decode())


（4）复杂的请求—代理IP

使用爬虫来爬取数据的时候，如果过于频繁的访问，而且网站还设有限制的话，很有可能会禁封我们的ip地址，这个时候就需要设置代理，来隐藏我们的真实IP。

代理IP的原理：以本机先访问代理IP，再通过代理IP地址访问服务器，这样服务器接受到的访问IP就是代理IP地址。

urllib提供了urllib.request.ProxyHandler()方法可动态的设置代理IP池。将代理IP以字典形式传入该方法，然后通过urllib.reques.build_opener(）创建opener对象，用该对象的open()方法向服务器发送请求。

 from urllib import request

  url = 'http://httpbin.org/ip'
  proxy = {'http': '218.18.232.26:80', 'https': '218.18.232.26:80'}
  proxies = request.ProxyHandler(proxy)  # 创建代理处理器
  opener = request.build_opener(proxies)  # 创建opener对象

  resp = opener.open(url)
  print(resp.read().decode())
在上述的方法中我们还用到了一个新的东西，即request.build_opener()方法，其实urlopen()就是通过构造好了的opener对象发送请求，在这里我们使用request.build_opener()方法重构了一个opener()对象，我们可以通过这个对象实现urlopen()实现的任何东西。

（5）复杂的请求—cookies

有时候当我们访问一些网站的时候需要进行翻页或者跳转等其它操作，为了防止无法访问我们想要的数据，需要让网站识别我们是同一个用户。这个时候我们就需要带上cookie进行访问。

在设置cookie的时候由于urllib并没有很好的处理cookie的对象，所以在这里我们需要用到一个别的库，即http库，并使用里面的cookiejar来进行cookie的管理：

from http import cookiejar
from urllib import request

url = 'https://www.baidu.com'
# 创建一个cookiejar对象
cookie = cookiejar.CookieJar()
# 使用HTTPCookieProcessor创建cookie处理器
cookies = request.HTTPCookieProcessor(cookie)
# 并以它为参数创建Opener对象
opener = request.build_opener(cookies)
# 使用这个opener来发起请求
resp = opener.open(url)

# 查看之前的cookie对象，则可以看到访问百度获得的cookie
for i in cookie:
    print(i)
  当然，如果把上面这个生成的opener对象使用install_opener方法来设置为全局的，opener对象之后的每次访问都会带上这个cookie。设置全局后既可以用urlopen()方法， 也可以用opener.open() ，不安装的话只能用opener.open()方法

# 将这个opener设置为全局的opener，之后所有的，不管是opener.open()还是urlopen() 发送请求，都将使用自定义
request.install_opener(opener)
resp = request.urlopen(url)
（6）复杂的请求—证书验证

CA(Certificate Authority)是数字证书认证中心的简称，是指发放、管理、废除数字证书的受信任的第三方机构。

CA的作用是检查证书持有者身份的合法性，并签发证书，以防证书被伪造或篡改，以及对证书和密钥进行管理。现实生活中可以用身份证来证明身份， 那么在网络世界里，数字证书就是身份证。和现实生活不同的是，并不是每个上网的用户都有数字证书的，往往只有当一个人需要证明自己的身份的时候才需要用到数字证书。普通用户一般是不需要，因为网站并不关心是谁访问了网站，现在的网站只关心流量。但是反过来，网站就需要证明自己的身份了。比如说现在钓鱼网站很多的，比如你想访问的是www.baidu.com，但其实你访问的是www.daibu.com”，所以在提交自己的隐私信息之前需要验证一下网站的身份，要求网站出示数字证书。一般正常的网站都会主动出示自己的数字证书，来确保客户端和网站服务器之间的通信数据是加密安全的。

最简单的方法就是通过添加忽略ssl证书验证关闭证书验证,由于urllib并没有很好的处理ssl的对象，所以在这里我们需要用到一个别的库，即ssl库，如下：

import ssl
from urllib import request

context = ssl._create_unverified_context()
res = urllib.request.urlopen(request, context=context)
2. urllib.error 模块
在urllib中主要设置了两个异常，一个是URLError，一个是HTTPError，HTTPError是URLError的子类。

HTTPError还包含了三个属性：

code：请求的状态码
reason：错误的原因
headers：响应的报头
from urllib.error import HTTPError

try:
    request.urlopen('https://www.jianshu.com')
except HTTPError as e:
    print(e.code)

3.urllib.parse 模块
data参数需要用urllib.parse模块对其进行数据格式处理。

urllib.parse.quote(url)：（URL编码处理）主要对URL中的非ASCII码编码处理

urllib.parse.unquote(url)：（URL解码处理）URL上的特殊字符还原

urllib.parse.urlencode：对请求数据data进行格式转换

 

二、urllib3
来自官方网站的解释：

urllib3是一个功能强大，对SAP 健全的 HTTP客户端。许多Python生态系统已经使用了urllib3，你也应该这样做。                    

通过urllib3访问一个网页，那么必须首先构造一个PoolManager对象，然后通过PoolMagent中的request方法或者 urlopen()方法来访问一个网页，两者几乎没有任何区别。

class urllib3.poolmanager.PoolManager（num_pools = 10，headers = None，** connection_pool_kw ）
生成一个PoolManager所需要的参数：

num_pools 代表了缓存的池的个数，如果访问的个数大于num_pools，将按顺序丢弃最初始的缓存，将缓存的个数维持在池的大小。
headers 代表了请求头的信息，如果在初始化PoolManager的时候制定了headers，那么之后每次使用PoolManager来进行访问的时候，都将使用该headers来进行访问。
** connection_pool_kw 是基于connection_pool 来生成的其它设置
当访问网页完成之后，将会返回一个HTTPResponse对象，可以通过如下的方法来读取获取GET请求的响应内容：

import urllib3

http = urllib3.PoolManager(num_pools=5, headers={'User-Agent': 'ABCDE'})

resp1 = http.request('GET', 'http://www.baidu.com', body=data)
# resp2 = http.urlopen('GET', 'http://www.baidu.com', body=data)

print(resp2.data.decode())
除了普通的 GET 请求之外，你还可以使用request()或者urlopen()进行 POST 请求：

import urllib3
import json

data = json.dumps({'abc': '123'})
http = urllib3.PoolManager(num_pools=5, headers={'User-Agent': 'ABCDE'})

resp1 = http.request('POST', 'http://www.httpbin.org/post', body=data,timeout=5,retries=5)
#resp2 = http.urlopen('POST', 'http://www.httpbin.org/post', body=data,timeout=5,retries=5)

print(resp1.data.decode())
注意事项

urllib3 并没有办法单独设置cookie，所以如果你想使用cookie的话，可以将cookie放入到headers中


request()和urlopen()方法：
request(self, method, url, fields=None, headers=None, **urlopen_kw)

urlopen(self, method, url, redirect=True, **kw):
差距还是很明显的，urlopen()比request()有三个参数是不一样的，你会发现request()具有fields，headers两个参数。而且相比之下 reuqest() 方法还是比较符合人们的使用方式的，所以更多的也就使用 request() 方法了。

推荐使用request()来进行访问的，因为使用request()来进行访问有两点好处，

可以直接进行post请求，不需要将 data参数转换成JSON格式
直接进行GET请求，不需要自己拼接url参数
import urllib3
import json

data = {'abc': '123'}
http = urllib3.PoolManager(num_pools=5, headers={'User-Agent': 'ABCDE'})

resp1 = http.request('POST', 'http://www.httpbin.org/post', fields=data)
# resp1 = http.request('GET', 'http://www.httpbin.org/post', fields=data)  

print(resp1.data.decode())


虽然urlopen()没有fields，但这并不代表它不能进行POST访问，相反，两个方法都有一个body属性，这个属性就是我们平时POST请求时所熟知的data参数，只不过你需要将data字典先转换成JSON格式再进行传输。

不过特别要声明的一点是 fielder 和 body 中只能存在一个。

2.2 urllib3 ProxyManager
如果你需要使用代理来访问某个网站的话, 那么你可以使用 ProxyManager 对象来进行设置

def __init__(self, proxy_url, num_pools=10, headers=None,proxy_headers=None, **connection_pool_kw):
ProxyManager和PoolManager的方法基本完全相同，这里举个简单的小例子，就不再赘述了：

import urllib3
import json

data = {'abc': '123'}
proxy = urllib3.ProxyManager('http://50.233.137.33:80', headers={'connection': 'keep-alive'})
resp1 = proxy.request('POST', 'http://www.httpbin.org/post', fields=data)
print(resp1.data.decode())

————————————————
版权声明：本文为CSDN博主「爱编程的喵汪人」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_42415326/article/details/90794150