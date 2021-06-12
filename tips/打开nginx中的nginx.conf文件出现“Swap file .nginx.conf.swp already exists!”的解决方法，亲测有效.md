# 打开nginx中的nginx.conf文件出现“Swap file ".nginx.conf.swp" already exists!”的解决方法，亲测有效

首先，这个方法也是我博客园的兄弟那里看到的，如果你上次未正常关闭nginx.conf，那么你下次启动的时候很有可能会出现以下的错误，
E325: ATTENTION
Found a swap file by the name “.nginx.conf.swp”
owned by: root dated: Tue Nov 5 15:21:29 2019
file name: /usr/local/nginx/conf/nginx.conf
modified: YES
user name: root host name: localhost.localdomain
process ID: 7762
While opening file “nginx.conf”
dated: Tue Nov 5 14:47:29 2019

(1) Another program may be editing the same file. If this is the case,
be careful not to end up with two different instances of the same
file when making changes. Quit, or continue with caution.
(2) An edit session for this file crashed.
If this is the case, use “:recover” or “vim -r nginx.conf”
to recover the changes (see “:help recovery”).
If you did this already, delete the swap file “.nginx.conf.swp”
to avoid this message.
**Swap file “.nginx.conf.swp” already exists! Interrupt: Press ENTER or type command to continue**
好了，废话不多说，直接上方法，亲测有效奥！
方法：
在你的conf目录下执行ls -a，你会看到一个以.swp结尾的文件，输入rm -rf 以.swp结尾的文件名，然后回车删除即可，你在一次打开文件就没有这个错误信息了

