# pycharm安装包 pip源提示“not a trusted or secure host” 解决

问题：
The repository located at mirrors.aliyun.com is not a trusted or secure host and is being ignored. If this repository is available via HTTPS we recommend you use HTTPS instead, otherwise you may silence this warning and allow it anyway with '--trusted-host mirrors.aliyun.com'. Could not find a version that satisfies the requirement proxy (from versions: ) No matching distribution found for proxy
答案：
http://mirrors.aliyun.com/pypi/simple/
改成
https://mirrors.aliyun.com/pypi/simple/
