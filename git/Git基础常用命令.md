# Git基础常用命令

* git init . 初始化
* git clone [url] 克隆
* git add . 添加所有至待发送区
* git commit -am "[message]" 添加标记
* git push 更新
* git status 查看状态
* git shortlog -sn x 显示所有提交过的用户
* git brach 查看本地分支
* git brach -r 查看远程分支
* git brach -a 查看本地加远程所有分支
* git fetch origin master 取回远程的master分支

------

第一步在本地 仓库中 右键 打开git bash
然后git init 初始化
然后git add . + 网络git仓库路径
然后git status查看操作 状态 修改的什么
git commit "做了什么修改"
git push 上传

前端Git地址：https://github.com/Dylanjor/GsfCoffeeVue.git
后端Git地址：https://github.com/Dylanjor/GsfCoffee.git

###  本地ssh设置 

$ ssh-keygen -t rsa -C '2545664565@qq.com'

### 使用命令分别创建两个平台的公钥

ssh-keygen -t rsa -C "xxxxxx@xxx.com" -f "id_rsa_gitee"
ssh-keygen -t rsa -C "xxxxxx@xxx.com" -f "id_rsa_github"

```备注：若提示ssh-keygen 不是内部或外部命令，主要是没有找到ssh-keygen.exe，所以我们要将ssh-keygen.exe文件所在的目录配置到全局变量中去。```

第一步：找到`C:\Program Files\Git\usr\bin`目录下的ssh-keygen.exe （就是其目录，不一定是C盘，具体看安装在哪个盘），然后复制其所在的路径地址

第二步：找到桌面计算机->右击属性–>高级系统设置–>环境变量–>系统变量,找到Path变量，进行编辑，End到最后，输入分号，粘贴复制的ssh-keygen所在的路径，保存；

### 配置不同的公钥

Host github.com  
    HostName github.com  
    PreferredAuthentications publickey 
    IdentityFile ~/.ssh/id_rsa

Host gitee.com
    HostName gitee.com
    PreferredAuthentications publickey 
    IdentityFile ~/.ssh/id_rsa_gitee