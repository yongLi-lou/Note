# python 文件时间操作

# 一 按时间创建文件

**源码**

```python
# 截图方式二
# coding=utf-8
import os
import time
# 当前年月日时分秒时间 2020-01-16-10_11_49
picture_time = time.strftime("%Y-%m-%d-%H_%M_%S", time.localtime(time.time()))
#当前年月日 2020-01-16
directory_time = time.strftime("%Y-%m-%d", time.localtime(time.time()))
print('当前年月日时分秒时间：'+ picture_time)
print("当前年月日:"+directory_time)
# 获取当前文件目录
print('当前文件目录:'+os.getcwd())
# 获取到当前文件的目录，并检查是否有 directory_time 文件夹，如果不存在则自动新建 directory_time 文件
try:
    File_Path = os.getcwd() + '\\' + directory_time + '\\'
    print(os.path) 
    #exists判断文件路径是否存在
    if not os.path.exists(File_Path):
        os.makedirs(File_Path)
        print("目录新建成功：%s" % File_Path)
    else:
        print("目录已存在！！！")
except BaseException as msg:
    print("新建目录失败：%s" % msg)

#切换目录
os.chdir("D:/git")
print('切换后的目录位置:'+os.getcwd())
```

**源码执行控制台打印：**

> 当前年月日时分秒时间：2020-01-16-11_19_12
> 当前年月日:2020-01-16
> 当前文件目录:D:\git\gongcheng
> <module 'ntpath' from 'D:\Python36\lib\ntpath.py'>
> 目录新建成功：D:\git\gongcheng\2020-01-16
> 切换后的目录位置:D:\git

# 二 获取环境变量、进程、父进程

**源代码**

```python
import os
#获取系统环境变量
print("环境变量是："+os.environ["CLASSPATH"])
#获取当前进程ID
print(os.getpid())
#获取父进程ID
print(os.getppid()) 
```

**源码执行控制台打印：**

> 环境变量是：.;C:\Program Files\Java\jdk1.8.0_101\lib\dt.jar;C:\Program >Files\Java\jdk1.8.0_101\lib\tools.jar;
> 10760
> 11224

# 三、获取当前文件的创建、修改、访问时间

**源码**

```python
import time
import os

filepath = 'D:\gongcheng'
#获取文件的创建时间 get create time
ctime = os.path.getctime(filepath)
print("创建时间是："+time.ctime(ctime))
#获取文件的修改时间 get modify time
utime = os.path.getmtime(filepath)
print("修改时间是："+time.ctime(utime))
#获取文件的访问时间 get active time
atime = os.path.getatime(filepath)
print("访问时间是："+time.ctime(atime))
```

**源码执行控制台打印：**

> 创建时间是：Fri Jul 5 19:13:27 2019
> 修改时间是：Mon Jan 13 18:27:26 2020
> 访问时间是：Mon Jan 13 18:27:26 2020