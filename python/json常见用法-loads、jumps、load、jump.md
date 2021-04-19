# [json常见用法-loads、jumps、load、jump](https://www.cnblogs.com/fawaikuangtu123/p/9728021.html)

 这一篇博客的目的主要是想说明一个问题：干什么事情要抓住重点，不要力求完美，不要追求那种'大而全'的办事方式，因为时间是有限的，而客观事物(这里主要指技术方面的知识)是无限的，so，anyway!

1.json.dumps()函数是将字典转化为字符串

```
import` `json``dict1 = {``"age"``: ``"12"``}``json_info = json.dumps(dict1) json_info此时就被转换成一个字符串了
```

 2.json.loads()函数是将字符串转化为字典

```
json_info = ``'{"age": "12"}'``dict1 = json.loads(json_info) dict1由字符串被转换为一个字典
```

 3.json.dump()函数的使用，将json信息写进文件

```
import` `json``json_msg = {``"install"``: {``"install_date"``: ``"2018/09/26"``,``"install_result"``: ``"success"``}}``file` `= ``open``(``'1.json'``, ``'w'``)``json.dump(json_msg, ``file``)
```

 4.json.load()函数的使用，将读取json信息

```
file` `= ``open``(``'1.json'``,``'r'``,encoding=``'utf-8'``)``info = json.load(``file``)``with ``open``(``'1.json'``, ``'r'``) as f:``  ``data = json.load(f)``还是用第二种比较好，不用手动去关文件了
```

 5.但是这样讲个知识点比较干瘪，实际应用如下：

客户端要向服务器端发送一个json字符串，服务器端要接收并处理。下面演示正确的代码：

```
import` `json``with ``open``(``'1.json'``, ``'r'``) as f:``  ``data = json.load(f)``data = {``"company_data"``: json.dumps(data)}` `# urlopen（）的data参数默认为None，当data参数不为空的时候，urlopen（）提交方式为Post``from urllib ``import` `request, parse``url = r``'http://192.168.165.4:8000/show/report/'``company_data = parse.urlencode(data).encode(``'utf-8'``)``req = request.Request(url, data=company_data)``print(company_data)``page = request.urlopen(req).``read``()``page = page.decode(``'utf-8'``)` `print(``'page是什么'``, page, ``type``(page))
```

 接收的话，下面的代码只是接收并打印，并没有进行一系列的判断然后去入库之类的，因为我还没做：

```
def receive_data(request):``  ``if` `request.method == ``'POST'``:``  ``print(request.get_full_path())``  ``print(request.body)``  ``data = request.POST.get(``"company_data"``)``  ``if` `data:``    ``try:``      ``data = json.loads(data)``      ``print(``"企业数据"``, ``type``(data), data)``      ``return` `HttpResponse(data)``    ``except ValueError as e:``      ``print(str(e))``  ``else``:``    ``return` `HttpResponse(``"no data"``)
```