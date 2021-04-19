# Windows下将Python文件打包成.EXE可执行文件的方法

在使用Python做开发的时候，时不时会给自己编写了一些小工具辅助自己的工作，但是由于开发依赖环境问题，多数只能在自己电脑上运行，拿到其它电脑后就没法运行了。这显得很不方便，不符合我们的初衷，那么有没有一种什么办法可以使我们编写好的程序，可以直接在各种windows下运行的呢?

　　答案是：有的，说到windows大家都能想到( .exe )这个东西吧!没错，就是把Python编写的代码打包成可执行的 exe 文件，直接在系统上运行，这个问题不久完美解决了吗?

　　**下面就来讲讲如何实现，具体如下：**

　　**安装pyinstaller库**

　　在实现exe之前，我们需要安装一个第三方的 pyinstaller 依赖库，通过这个库将py文件打包成可执行的.exe文件。

　　**windows下使用 pip 工具安装：**　

```
pip install pyinstaller ``# pip 工具``　　``# 或者``　　pip3 install pyinstaller ``# pip3 工具
```

　　linux 下安装：

```
sudo apt``-``get install pyinstaller ``# ubuntu 或 linux ...系统``　yum install pyinstaller ``# centos 系统
```

　　打包演示

安装好 pyinstaller 库之后，可以使用 pyinstaller –help 指令获得该库的使用说明，这里介绍最简单的打包方法：

　　1)创建 test.py 文件

　　2)将 test.py 文件打包成 ( .exe ) 文件，指令如下：　

```
pyinstaller ``-``F test.py
```

　　程序执行完毕后，会在当前目录下生成4个文件：dist 、 __pycache__ 、build 、test.spec，其中可以执行文件存放在 dist 文件夹当中。

　　这时只需将这3个文件打包在一个文件夹内，直接拿到其它windows平台上就可以运行了。是不是简单方便呢…..