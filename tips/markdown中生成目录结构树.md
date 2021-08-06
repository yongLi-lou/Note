# markdown中生成目录结构树

### windows

😜进入需要生成目录结构的文件主目录

😜在当前目录打开命令行

😜输入命令显示树结构

tree ：按当前目录中的文件夹生成树（只涉及文件夹，不包含txt等子文件）
tree /f ：按当前目录中的全部文件生成树
😜输入命令保存树结构（根据你需要的结构保存）

tree > 文件名.txt ( 文件名任意)
tree /f > 文件名.txt
😜会在当前目录下生成了文件名.txt文件

示例：
	tree > my_tree.txt
	tree /f > my_tree.txt

### mac

Mac下可以使用`brew install tree`进行安装。安装后，在terminal中输入`tree -a`便可以查看某个文件夹下的所有文件。

当然了，我们的需求肯定不止列出所有文件这么简单。下面介绍几个常用的命令
* tree -d 只显示文件夹；
* tree -L n 显示项目的层级。n表示层级数。比如想要显示项目三层结构，可以用tree -l 3；
* tree -I pattern 用于过滤不想要显示的文件或者文件夹。比如你想要过滤项目中的node_modules文件夹，可以使用tree -I "node_modules"；
* tree > tree.md 将项目结构输出到tree.md这个文件。

更多命令的使用可以查看`tree --help`
