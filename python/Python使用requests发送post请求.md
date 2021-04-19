# [Python使用requests发送post请求](https://www.cnblogs.com/android-it/p/9558751.html)

**1.我们使用postman进行接口测试的时候，发现POST请求方式的编码有3种，具体的编码方式如下：**

A：application/x-www-form-urlencoded ==最常见的post提交数据的方式，以form表单形式提交数据

B：application/json  ==以json格式提交数据

C：multipart/form-data ==一般使用来上传文件（较少用）

**2.我们使用python做接口测试时，经常使用的方式为：requests.post(url,data),具体我们使用不同的编码方式来做接口测试：**

A：Requests以form表单形式发送post请求，具体代码实现如下所示：

```
import` `requests,json` `url ``=` `'http://httpbin.org/post'``data ``=` `{``'key1'``:``'value1'``,``'key2'``:``'value2'``}``r ``=``requests.post(url,data)``print``(r)``print``(r.text)``print``(r.content)
```

A1：运行结果如下所示：

![img](https://images2018.cnblogs.com/blog/1034663/201808/1034663-20180830103022792-73613269.png)

B：Requests以json形式发送post请求，具体代码实现如下所示：

```
import` `requests,json` `url_json ``=` `'http://httpbin.org/post'``data_json ``=` `json.dumps({``'key1'``:``'value1'``,``'key2'``:``'value2'``})  ``#dumps：将python对象解码为json数据``r_json ``=` `requests.post(url_json,data_json)``print``(r_json)``print``(r_json.text)``print``(r_json.content)
```

B1：运行结果如下所示：

![img](https://images2018.cnblogs.com/blog/1034663/201808/1034663-20180830103401283-237370186.png)

C:Requests以multipart形式发送post请求，具体代码实现如下所示：

```
import` `requests,json` `url_mul ``=` `'http://httpbin.org/post'``files ``=` `{``'file'``:``open``(``'E://report.txt'``,``'rb'``)}``r ``=` `requests.post(url_mul,files``=``files)``print``(r)``print``(r.text)``print``(r.content)
```

C1:运行结果如下所示：

![img](https://images2018.cnblogs.com/blog/1034663/201808/1034663-20180830104131438-422567808.png)

**注：E://report.txt==自定义，具体根据自己放的目录来定义，内容随意**