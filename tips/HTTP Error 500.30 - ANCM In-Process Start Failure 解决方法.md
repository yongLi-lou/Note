# HTTP Error 500.30 - ANCM In-Process Start Failure 解决方法

出现问题图



解决方法，

先检查发布的文件是否正确，版本是否选对。



 

第二步，检查应用程序池是否正确



第三步，还是应用程序池配置，和发布的版本对应上。



第四步，检查，是否缺少模块



第五步，检查程序是否正常，直接运行发布文件的XXX.exe。 然后访问http://localhost:5000, 若能访问，则正常。



 

第六步，检查web.config文件，删除 hostingModel="InProcess"



 

以上步骤能解除90%以上的问题。

原文链接：https://blog.csdn.net/qq_25042791/article/details/103055914