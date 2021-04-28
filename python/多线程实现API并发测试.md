# 多线程实现API并发测试
```python
import requests
import threading
import time
 
data = {
	"times": 20, # 并发量
	#"method": "POST",
	"url": "http://www.baidu.com",
	"header": {
		#header
	},
	"body": {
		#参数
	}
}
 
 
def get_requests():
	global RIGHT_NUM
	global ERROR_NUM
	try:
		r = requests.get(data["url"],headers = data["header"])
		#print(r.status_code)
		if r.status_code == 200:
			RIGHT_NUM += 1
			#print("RIGHT_NUM:",RIGHT_NUM)
		else:
			ERROR_NUM += 1
	except Exception as e:
		print(e)
 
def run1():
	Threads = []
	time1 = time.process_time()
	for i in range(data["times"]):
		t = threading.Thread(target = get_requests)
		t.setDaemon(True)
		Threads.append(t)
 
	for t in Threads:
		t.start()
	for t in Threads:
		t.join()
 
	time2 = time.process_time()
 
	print("===============测试结果===================")
	print("URL:", data["url"])
	print("并发数:", data["times"])
	print("总耗时(秒):", time2 - time1)
	print("每次请求耗时(秒):", (time2 - time1) / data["times"])
	print("正确数量:", RIGHT_NUM)
	print("错误数量:", ERROR_NUM)
 
 
if __name__ == '__main__':
 
	RIGHT_NUM = 0
	ERROR_NUM = 0
	print('测试启动')
 
	run1()
 
	print("执行结束.")
```

