# Python :=海象运算符最简单的解释

背景：python 3.8正式版最近更新了，其中PEP572中的海象运算符获得正式python版本的支持.我看了官网的文档还有其它大神写的这个东西，我感觉在将来的python语句中是非常实用的一个东西，所以写下这篇博客来介绍、介绍，同时也是自己学习新版特性，尽管我司生产环境还停留在 3.6，但并不影响我尝尝鲜.

ps：这是3.8 新特性网址：https://docs.python.org/3/whatsnew/3.8.html

官网介绍：Assignment expressions（赋值表达式）

There is new syntax := that assigns values to variables as part of a larger expression. It is affectionately known as “the walrus operator” due to its resemblance to the eyes and tusks of a walrus.

中译过来就是：咱们这新开发了一种语法，它是一个强大的表达式，会把表达式的一部分赋值给变量，因为很像海象的眼睛和象牙，所以成为海象操纵者(掌控雷电的感觉(~-^)。听起来有点懵，可能是我的翻译水准不够把。

描述之后呢，官网给了一些例子：

1

if (n := len(a)) > 10:
    print(f"List is too long ({n} elements, expected <= 10)")
1
2
在这个例子里面，文档强调通过使用海象表达式，避免len()方法运行两次，从而提高了运行速度.

假如在没有海象运算符的时候，我们会怎么来写这段代码呢？来试一试：

if len(a) > 10:
    print(f"List is to long({len(a)} elements, expected <= 10)")
1
2
或者这样写，避免使用两次len方法，却又多了一次赋值给中间变量的步骤.

n = len(a)
if n > 10:
    print(f"List is to long({n} elements, expected <= 10)")
1
2
3
我觉得一个好处就在于这里，为我们省去了一个赋值中间变量的步骤.

2

接下来，文档又给出了第二个例子，关于海象运算符在正则中的应用

discount = 0.0
if (mo := re.search(r'(\d+)% discount', advertisement)):
    discount = float(mo.group(1)) / 100.0
1
2
3
背景应该是这样的，代码想从一段advertisement的文本中，拿到打折的百分比，然后通过除法得到百分比数. 在这里面，海象运算符干了些什么呢？我们来看哈，虽然正则匹配的语句只用了一次，但它却在两个 地方起了作用，一次是检测匹配是否发生，也就是if控制语句，还有一次是提取分子.同样用python3.8之前的语句来写这个语句：

discount = 0.0
mo = re.search(r'(\d+)% discount', advertisement)
if mo:
    discount = float(mo.group(1)) / 100.0
1
2
3
4
和上面的例子相似，海象运算符替我们省去了中间赋值的步骤，让代码更加整洁了.

3

接下来，文档又给了两个例子，首先是一个 while循环控制，还是省去了赋值的步骤：

# Loop over fixed length blocks
while (block := f.read(256)) != '':
    process(block)
1
2
3
背景：代码读取一个文件，当不为空执行操作,同样看没有海象运算符，我们会怎么写：

while 1:
    block = f.read(256)
    if block != '':
        process(block)
	else:        
		break
1
2
3
4
5
6
同样是 赋值一气呵成，这让我认为海象运算符的作用在于，把计算语句的结果赋值给变量，然后，变量可以在代码块里执行运用.

4

接下来是最后一个例子：

[clean_name.title() for name in names if (clean_name := normalize('NFC', name)) in allowed_names]
1
背景是一个列表推导式里面，需要计算得到符合过滤条件的值，这个可以稍稍简写下，会显得更加通透：

[o.title() for i in names if o:=f(i) in allowed_names]
# 在这里我把: clean_name normalize('NFC', name) 比做了一个方法

更简洁一点：
[y for x in names if (y := f(x))]

1
2
3
4
5
6
同样我们最简洁的方式来改写这段代码：

[f(x) for x in names if f(x)]
1
可以看到f(x)执行了两次，故此可以看到海象运算符的用法了吧，即简化流程，提高运算速度.
