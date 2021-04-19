# [Python从json中提取数据](https://www.cnblogs.com/xiaojiayu/p/5195298.html)

\#json string:
s = json.loads('{"name":"test", "type":{"name":"seq", "parameter":["1", "2"]}}')
print s
print s.keys()
print s["name"]
print s["type"] ["name"] 
print s["type"] ["parameter"] [1]