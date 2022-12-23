# ReSharper常用快捷键

Alt + F7：查找引用

Ctrl + N：Go To Everything 定位到任何，非常强大

Ctrl + Shift + N：Go To File 定位到文件

Ctrl + F12：Go To File Member 在当前类中查找

F2：重命名任何东西，重构利器

Ctrl + Tab：活动文件之间切换，当前打开的所有文件

Ctrl + Shift + Alt +向上/向下：上下行代码交换位置

Ctrl + W：快速选中整个/一块单词

Ctrl + Alt + F：Clean Code

Ctrl + Alt + J：Sorround with Template，快速添加语句块，如if,for,try catch,using,#region

Ctrl + Q：快速文档

Alt + F12：显示下一个Error

Ctrl + E：显示最近编辑的文件

![img](https://pic2.zhimg.com/80/v2-1f4486a295fdd938aacb918afc8f3925_1440w.webp)

### 导航和搜索

ReSharper提供了很多导航和搜索功能。

### 跳到声明

按Ctrl键，将鼠标悬停在你的代码上。我们将看到，其他位置定义的所有相同的类名在焦点时都会出现下划线。同时你可以按住Ctrl键并点击任何类名直接导航到它的声明。如果变量或类在当前解决方案中定义，ReSharper将打开相应的文件并将光标带到声明处。如果类名在编译库中定义，ReSharper将根据你的设定打开它。

![img](https://pic2.zhimg.com/80/v2-07d3a83bab24dc2afbfbe56668401fe5_1440w.webp)

### 查找用法

要反向导航，即找到解决方案中的所有使用该成员或变量的地方，请按VS快捷键：Shift+F12，Resharper快捷键：Alt+F7，Resharper将快速找到并显示成员或变量的所有用法。

### 检查可用的导航操作

另一个方便的导航快捷键VS快捷键 ：Alt+~，Resharper快捷键：Ctrl+Shift+G。Resharper将显示所有可用的导航选项：

![img](https://pic2.zhimg.com/80/v2-e15cfcfaf59fe5ea04efee045089b879_1440w.webp)

### 在解决方案中查找

如果你需要在解决方案中查找，按VS快捷键 ：Ctrl+T，Resharper快捷键：Ctrl+N，调用此功能，将展示包括最近的文件，导航的项目，建议列表。我们可以开始键入以查找类型，变量，文件，最近编辑，最近文件和最近查看的方法。

### 在解决方案树中定位当前文件

当导航命令将你带到一个新文件时，我们可能希望看到它在解决方案资源管理器中的位置。只需按下Shift+Alt+L，解决方案资源管理器将定位到当前文件。

### 在编辑器中编码

当你在编辑器中工作时，代码编辑助手是你的帮手。

### 代码自动完成（IntelliSense智能感知）

所有完成功能支持CamelHumps-也就是说，你可以只通过输入其大写字符完成任何项目。

ReSharper补充和扩展了Visual Studio的本地代码自动完成（IntelliSense）与更高级的功能。例如，它根据你的输入精确缩小建议列表，并自动导入所选类型和扩展方法，在完成方法名称时添加括号，根据类型建议变量和字段名称等。

![img](https://pic1.zhimg.com/80/v2-3d7c0c8323062a1651f90f2af3e7429c_1440w.webp)

如果需要，你可以随时通过选择相应的选项回到Visual Studio自带的IntelliSense，ReSharper->Options–>Environment->IntelliSense->General。

无论你是否喜欢，你都可以在键入某些内容之后，或者甚至不需要输入，显式调用ReSharper的代码完成功能。任何有意义的代码都可以自动完成。

你还可以按几次完成快捷方式获得更高级的完成建议：

按VS快捷键 ：Ctrl+Alt+Space，Resharper快捷键：Ctrl+Shift+Space调用智能补全，提供基于预期类型的表达建议。

快捷键：Ctrl+Alt+Space调用导入命名空间自动完成，不管他们属于哪个命名空间，都将显示匹配给定前缀的所有类型，在必要时导入适当的命名空间到当前文件。

### 选择和移动代码块

尝试按下VS快捷键 ：Ctrl+Alt+Right Resharper快捷键：Ctrl+W/VS快捷键 ：Ctrl+Alt+Left Resharper快捷键：Ctrl+Shift+W。这些快捷方式允许我们选择连续的单词，行或代码块，以便我们可以轻松地选择任何所需的表达式进行复制，剪切或移动。

如果你需要移动选定的代码块，按Ctrl+Shift+Alt，然后使用箭头键块移动到任何允许的位置。

### 万能的 Alt + Enter

如果我们使用了 Reshaper 的快捷键设置，那么，在每个会出现提示的地方，按下 Alt + Enter 组合键，就会弹出 Resharper 建议你要进行的操作，比如:

![img](https://pic4.zhimg.com/80/v2-aaf9f19aab89c8384838d61911a90327_1440w.webp)

在这个提示里，Reshaper 告诉我们没有引用 System.Text 这个命名空间，这个时候，按下 Alt + Enter 就会自动帮我们 using 该命名空间了；

### 自动完成语法糖

再比如：

![img](https://pic3.zhimg.com/80/v2-7e5685d7776b40b2dfde8d036518da02_1440w.webp)

在 StringBuilder 上按下 Alt+Enter 组合键，就会提示你此处可以用 var来隐式声明变量。

或者，又比如在 if 上使用组合键，就会提示你用三元运算符：

![img](https://pic2.zhimg.com/80/v2-7034c56e415638d595704db8501b2be5_1440w.webp)

总之，Alt + Enter 是万能神键，看到任何的灯泡提示或者波浪线，就使用它，你常常会得到很有意义的帮助来提升你代码的质量。比如一些复杂的 LINQ 你可能不会写，使用 Alt+Enter 神键就会自动帮你将一些代码转换成很牛叉的 LINQ，看上你好像是个 LINQ 高手一样；

### 灯泡提示

![img](https://pic2.zhimg.com/80/v2-f294ebb0378bc1f9859c744869a83101_1440w.webp)

这个小灯就是提示，如果不想使用 Alt + Enter，就用鼠标猛戳这里，也会出现 Resharper 的建议。

### 这里有几个例子：

如果你看到一个红色的灯泡

![img](https://pic4.zhimg.com/80/v2-9663fb157aa977fef0596077463224cf_1440w.webp)

或黄色的灯泡

![img](https://pic2.zhimg.com/80/v2-4089476d3880abd1110da0cb1cbb4399_1440w.webp)

图标，按下Alt+Enter，因为这些提示都会告诉你，Resharper的已检测到错误或其他的改善代码的问题，而且它知道如何解决它。

如果你看到一个锤子

![img](https://pic1.zhimg.com/80/v2-d528a6751b0061e33f903a23391d41cc_1440w.webp)

的图标，可以忽略，也可以修改代码。如果你想修改代码，按Alt+Enter显示帮助。Resharper可以提供数以百计的建议，例如，改变成员可见性，添加遍历集合，使用using进行IDisposeable的释放等等。

总之在任何地方按下Alt+Enter都将能够快速找到并执行任何可用的ReSharper建议动作，代码不会写了？按下 Alt+Enter 帮你写代码：

![img](https://pic2.zhimg.com/80/v2-1cc7717c308eb4e0b0617d320d699141_1440w.webp)

万能的Alt+Enter能够帮你完成很多编写代码过程中的dirtywork，总结起来大概是这么些：

> 帮你实现某个接口或抽象基类的方法；
> 提供你处理当前警告的一些建议；
> 为你提供处理当前错误的一些建议（不一定是真的错误）；
> 为你简化当前的臃肿代码；

### Alt+F7 Find Usage查找引用

Alt+F7将光标所在位置的变量的所有使用以列表的方式显示出来，显示结果的窗体可以像其他窗体那样停靠。

它的优点包括：

> 可以从所有使用中挑选只显示read usage或者write usage，有时我们只是想知道某个变量在哪里被改变了。找到的位置前的图标也告诉你这点。
> 可以在下方预览，即使我们列出所有使用，也不想跳转到每个使用它的地方，这时预览可以帮你大忙。
> 当你在代码编辑器中改动了某些使用时，比如删除了某行，那么在查找结果的窗体中，会用删除线表示出来。
> 默认的是寻找解决方案中所有的使用，并且按照命名空间来组织，非常便于选择。

我现在已经记不起来在没有Alt+F7之前我是怎么查找的。反正现在我几乎不怎么样Ctrl+F了，除非我忘记了某个变量的名字。如果是这样，多半这个名字需要refactor，那也是Resharper的另一大块功能所在。也许有人对这个功能嗤之以鼻，但是用过CAP的人都知道，订阅和发布某个事件的签名，完全是字符串，如果你不用搜索来找到它的话，你都不知道这个控件的鼠标点下去，到底有多少个处理程序在背后开始工作了。用了Alt+F7来搜索这个字符串，等于在查找背后所有的调用者。

不过温馨提示一下，当光标停留在一个类型上时，要慎用Alt+F7，假设是一个string，你应该能想象到得找到多少个使用

### Find Results

在某个类，或者变量，或者方法上点 Find Usage ，或者戳快捷键 Alt+F7，就是把你选中类或变量或方法全部被引用到的地方以列表形式显示出来。VS2012之后的版本的查找和查找引用功能简直弱爆了。总之，这个功能也是 Reshaper 的一个亮点。

![img](https://pic3.zhimg.com/80/v2-52ae05a25fd9b7a09e2aeb83da7f514e_1440w.webp)

尤其，我们注意到图中处，它将你多次查找用页签的形式给你保留了起来，我们在分析代码的时候，往往可能会一次性查找多个变量的引用，在这个时候，就特别有帮助。总之，这个功能很有必要而且很程序员！

### 查找赋值

假如我们想查找某个属性在那些地方被赋值，这个功能就相当实用。建议将快捷键设成了 Alt+F8 - Value Origin，或者你可以打开Resharper的菜单或在变量上鼠标右键，选择 Inspect – Value Origin

![img](https://pic1.zhimg.com/80/v2-de95b2ba978bc556b9339479538826f8_1440w.webp)

如下图查找结果，共有1处地方对它进行过赋值

![img](https://pic1.zhimg.com/80/v2-c903bf049f336647cdc9a8918f30aec0_1440w.webp)

### 文档结构

我不知道你是否恼怒每次查看类的结构要去戳这个下拉框：

![img](https://pic4.zhimg.com/80/v2-467972fe680fc3f820c483b7be60a013_1440w.webp)

这个时候，你按组合键 Ctrl + F11或者Resharper->Windows->File Structure，就会出现 Resharper 的 File Structure 代码地图窗口：

![img](https://pic2.zhimg.com/80/v2-6b0f773ec76726fdab15840858564685_1440w.webp)

它很方便的让你更直观清晰地看到你的整个成员变量窗口，成员类型以及访问权限一目了然。

当我们看别人的代码，或者是看自己的代码的时候，总是觉得代码太多，于是我们就用 region来把代码进行了折叠注释，可是这样之后别人看代码就很郁闷，而Resharper的 File Structure功能，就可以把region和你的方法都展示出来。

说了这么多，其实就是把对象浏览器和region的长处结合起来，既可以清晰的分类，又能一目了然的找到需要的方法。Resharper这时帮上你的大忙了。用Ctrl+F11，就弹出一个像右边这样的窗口来。

![img](https://pic1.zhimg.com/80/v2-af09a86d1bb86bc8c2c14d273f1b2184_1440w.webp)

这里面，按照你的region来显示，这样读你的代码的人也受益了。每个方法的参数，返回值都如UML一样列出来。

> 如果需要浏览到某个方法，直接双击它的名字；
> 如果要把某几个方法装进一个新的region，则可以选中方法，点工具栏上的像框的那个图标；点叉则会删除这个region并把相应的方法移到外面来。
> 如果要调整某个方法的位置，比如把它移到别的region里面去，只需要在这里拖动这个方法即可。
> 更可喜的是，你想要的从这里浏览、找到所有使用和重构的功能也在这里提供了，在某个方法上右键你就能开始操作。

### Ctrl+Shift+R万般兼重构

重构是一种精神，证明你在致力于提供高效的、精炼的、健壮的代码，而不是凌乱的、晦涩的、漏洞百出的代码。

在Visual Studio中，微软虽然提供了重构工具，但是不够，远远不够！我们需要的重构是非常广义的，我们想要对代码进行快速的调整，快到我在想什么我的工具就能做什么这样的程度。这才是追求重构的最高境界。所以在这个意义上，只有Resharper为你提供了巨大的生产力。

一个写出完美代码的程序员永远只存在于一个白痴Leader的头脑里，作为码畜的我们都知道，代码是重构出来的，永远不是设计出来的。所以，你永远需要 Ctrl + Shift + R：

![img](https://pic3.zhimg.com/80/v2-bdd80c90d15d0a353b799cc08dc0e62e_1440w.webp)

Resharper 把你可能用到的重构方法都列出来了，动动键盘或者鼠标，你即刻就可以完成一次重构。

Visual Studio自身提供的重构包括了如下：

> 封装字段
> 提取方法
> 提取接口
> 提升局部变量
> 移除参数
> 重命名
> 重新排列参数

这些方法在Resharper中全部都支持（但Resharper的重构远不止这些），它们对应的变成了：

> 封装字段 —— Introduce Field
> 提取方法 —— Extract Method
> 提取接口 —— Extract Interface（另增加了Extract Superclass提取为基类）
> 提升局部变量 —— Introduce Variable
> 移除参数 —— 移到Change Signature（改变方法签名）中
> 重命名 —— Rename（Resharper会根据对象的类型名称，提供你几个可选的最合适的名称）
> 重新排列参数 —— 移到Change Signature（改变方法签名）中

我知道很多人都声称自己E文不好，但是，这确实都是很简单的单词，难不倒任何人的。这些重构的功能是人所共知的，下面就告诉大家一些Resharper特有的，首先，重构的快捷键是Ctrl+Shift+R：
1、对于类，除了提取接口、基类，你还可以移动它到其他的命名空间和移动到别的文件里，这是一个实用的功能，也许你不信，但是我真的遇到有个人，把所有的business entity都写在一个DataObject.cs里面。你难以想象，我打开它时嘴张了多大。
2、对于字段，提供了：

> Safe Delete，会检测所有使用到的地方，并询问如何删除；
> Pull Member Up和Push Member Down，可以把这个字段在基类和继承类中移动；
> Use base type where possible，如果可能的话，尽可能的使用基类，比如interface以及父类；
> Encapsulate Field，封装字段，但是这个功能远没有另一个提供同样功能的操作有用。我可以在后文中来讲。

3、对于方法，提供了：

与字段类似的功能；

> Change Signature，更改函数签名，包括更改名称，返回值类型，参数的各种信息，添加和删除参数，相当实用。如果你是在重写方法上操作，会提示你是否到基类中更改。
> Make Static，如果Resharper检测到这个方法并没有与非静态成员相关联的话，往往会自动地提示你（以黄色横杠的形式出现）可以改为static，如果你自作主张的对一些方法进行修改也无不妥，但后果自负。
> Extract class from parameter，如果你的参数有七个八个，那是否考虑用一个类来封装这些参数呢，于是这个功能应运而生。
> Method to Property，顾名思义，如果还在使用GetField()或者SetField(..)的话，你一定是从非.net星来的。

4、在方法体内部：

Extract Method，提取方法，不用介绍了吧。
Introduce Variable/Parameter/Field，取决于你光标所在的对象，可以提供转化的功能。
Inline Variable：内联临时变量，就是把：

IPoint point = new PointClass();
point.PutCoords(_point.X, _point.Y);

变成这样子：

new PointClass().PutCoords(_point.X, _point.Y); //这是个糟糕的例子

５、重命名：

为什么重命名值得挑出来讲，因为Resharper提供了命名建议这一金子般的功能。于是，想改名为易读性强的名字，不是那么费脑子的事情了。Resharper会根据这个变量的类型，为你提供几个备选名字，名字列表是列在光标位置上的（对方法重命名会弹出对话框），你只需要用方向键选择并敲回车即可，这种名字多是将类型的名字首字母改为小写得来的，甚至刨根到基类的类型名，你还可以在此基础上加以改进。如果你还在用i,j这种晦涩的名称，请迅速的把他们改为outIndex, pointCount之类可读的名称。

Resharper其实提供了更先进的功能，在你命名一个变量时，就有快捷键为你提供备选名字。
还有一些更广义上的，帮助你对代码进行调整的功能，我另写一篇吧，不然太长了。

### **Alt+Insert自动生成**代码

ReSharper可以通过提供自动生成样板代码帮助您集中精力于编程。例如，您可以调用不存在的方法，ReSharper将基于用法创建此方法，同时考虑返回类型和参数类型。

Resharper的Alt+Insert快捷键提供给你插入代码的功能。由于这两个键非常难按（因为离得太远，这是我的感受），真正在使用的时候，我用的是Alt-R-C-G，意指打开Resharper菜单——Code——Generate，都只需要你的左手，这样你可以右手一边比划，一边还在写代码，多酷啊。

生成的代码中最常用的是构造函数和属性，当你没有私有字段的时候，只会生成一个空的默认构造函数，而且没有生成属性的功能。在你有私有字段的情况下，生成之前会让你选择哪些私有字段需要作为构造函数的参数，并生成初始化的代码，这样编写重载极其方便。生成属性也类似。

再次常用的就是重写基类或者接口的方法了。选择Implement Interface Member或者Override Inheritate Member，Resharper会查找当前类的基类或接口，然后按继承层次列出来，根据你的选择重写或实现这些方法。

不是太常用的是生成Equals和GetHashCode方法，在我的应用场景中很少重写它们。但是根据《.NET设计规范》，不管是值类型还是引用类型的Equals都建议重写，并且应该重写GetHaseCode方法，因为它们相互依赖。如果你有这个需求，那么生成这三个函数一定能够帮你的大忙。

### 生成类型成员

当您插入的变量是类型声明，在任何地方按Alt+Insert。在打开的弹出菜单中，您可以选择要生成的类型。ReSharper可以创建构造函数，属性，重写成员等。

![img](https://pic1.zhimg.com/80/v2-87875ec80b046e6929aca12aff31dbac_1440w.webp)

### 快速完成代码

使用 VS 提供的智能感知和 Tab 键，能够快速完成代码，比如输入代码 prop，然后按 Tab 键，就会自动创建一个属性，不过 Resharper 提供了更多的选择，可以自动完成更多的代码。

按组合键 Alt + Insert，出现如下窗口：

![img](https://pic2.zhimg.com/80/v2-c3ee1a227de9276ad8f18147c3b5c401_1440w.webp)

话说我从来没有使用 VS 的方法自动完成过创建一个构造器，而使用 Resharper 就可以轻松实现。

### Ctrl+Alt+J包围代码

Visual Studio也提供了外侧代码这个功能，你可以按Ctrl+K,Ctrl+S来激活这个功能，虽然我并没有任何鄙视Visual Studio的意思，但是Resharper的快捷键确实更加合理（我在按下Ctrl的时候真的很难按下S），条目也更加清晰。Resharper中这个功能的快捷键是Ctrl+Alt+J，然后你就可以选择将当前行的代码包围到try-catch块或者using中了。这是很高效的方法，我们倾向于在开发的早期尽量不捕获异常，而在中后期才加入异常处理机制。于是你某一个时期有大量的工作是把他们扩到try-catch块中。而你要使用支持dispose对象时，最好的方法是使用using块。（MMP的，当我知道我的代码不是最优的时候，我总是寝食难安），这里自然也有把代码扩到region块中的功能，也是常用功能之一。

### Clean Code

当我们写了一个shit的类之后，什么是最愉快的，就是让它瞬间变干净以及变规范，这个时候，我们需要右键 Cleanup Code （Ctrl + Alt + F）：

![img](https://pic3.zhimg.com/80/v2-9457e96c8e5bc67034a258a82d5eff06_1440w.webp)

Resharper 提供了更多的选择，让我们可以自己设置 Cleanup Code 的规范，当然，也可以引入StyleCop插件，配合这个规范可以设置起来非常方便。

### 其他快捷键

### 调整方法的位置

前面我曾说过，如果要调整方法的位置，可以在File Structure窗口中拖放操作。如果你觉得只是把一个方法移动到前面去，却不得不打开代码结构窗口太过重量级，那么有轻量级的方法：当光标位于方法的名称上时，用Ctrl+Shift+上下键就可以移动方法的位置，包括方法的xml注释，但如果你用的不是三个/的xml注释而是两个/的，那么就对不起了！

### 其他琐碎的功能

你肯定常常会复制粘贴当前行的代码，例如在使用StringBuilder.Append的时候，Ctrl+D可以简化你Ctrl+C,Ctrl+V的工作。

关于Resharper的重构功能就是这些，我可能天真地把很多额外功能都算在重构里了，但是它确实能够帮助你快速的对代码进行调整和优化。所以，请不要深究我对重构的概念认识是不是混乱。

### 增强的浏览功能

浏览参数的方式：

输入方法的时候，我们已经习惯了由IDE提供给我们的参数提示，极大了方便了我们选择重载方法。在没有Resharper的环境下，Visual Studio已经做到了。那么为什么Resharper还要增强这个功能并大获好评的。试问，Visual Studio那窄窄的一行参数提示有没有让你觉得憋屈。我们有24寸乃至32寸的大屏幕，1080P/2k/4k的分辨率，却不得不盯着那窄条条，小心翼翼的按着上下键寻找我们需要的重载。至少，我觉得开发Resharper的家伙是受不了这种憋屈的，于是大开大阖版的参数列表出现了，长长的参数重载被以列表的形式展现出来，当你在使用GDI+方法，看到巨大的参数重载时，你会从心底里发出感叹：MMP的。

同时，Resharper展示参数的快捷键变成了Ctrl+P，如果你觉得屏蔽了打印的快捷键简直是在开玩笑的话，那么问问你自己有多少次打印过自己的代码。

### 浏览打开过的文档：

我窃以为你已经知道了在Visual Studio中切换文档的方式，它们包括：

Ctrl+Alt+上下方向键，可以在打开的文档中切换；
Ctrl+Tab，不仅可以在文档之前切换，并可以切换到解决方案文件夹，属性视图去，需要按左右键。

但是怎么样打开最近编辑后关闭的文件呢，Visual Studio很客气的又没有提供此功能，于是留给了Resharper。在我这里这个快捷键是Ctrl+E。如果你的不是，那么在Resharper-View-Recent Files菜单下看看它是什么。因为你会时常用到。打开一个文件的列表，用方向键选择并回车就会在编辑器中打开。

### 正则表达式

ReSharper提供了一套相当丰富的工具来处理.NET正则表达式。你可以快速分析现有正则表达式，查找并修复错误。当输入新的表达式时，ReSharper帮助自动完成和验证。

### 字符串文字中的正则表达式

默认情况下，ReSharper仅在Regex 类的方法中处理pattern参数中的正则表达式。但是，包含正则表达式的字符串可以在不同的地方定义：字符串常量，字段，其他方法的参数等。如果你希望ReSharper将字符串作为正则表达式处理，有三种不同的选择：

使用上下文操作：Alt+Enter 在插入状态时按下并选择Mark as injected language or reference。

![img](https://pic3.zhimg.com/80/v2-145d182fd409d9feaf74dddf65311eb2_1440w.webp)



ReSharper会将与该字符串对应的字符串范围标记为正则表达式，将该范围保存在其配置数据库中，并将随着包含文件的更改而跟踪该范围。这种方式非常快速简单，但有两个缺点：在外部文件更改（例如VCS合并）后范围可能会丢失，并且以此方式标记的注入将仅在本地进行跟踪。

如果稍后决定禁用将该字符串作为正则表达式处理，则可以使用" Remove .NET Regular Expression injection mark"操作。

![img](https://pic2.zhimg.com/80/v2-2e9c9f678f4e3c4c54cd0680bf9b446d_1440w.webp)

另一种方法是注释的自己的方法是使用 [JetBrains.Annotations](https://link.zhihu.com/?target=https%3A//www.jetbrains.com/help/resharper/2018.1/Code_Analysis__Code_Annotations.html) nuget包的正则表达式的参数 [RegexPatternAttribute] 。这是参数中正则表达式的推荐方式。

ReSharper会将方法调用中的相应参数处理为正则表达式：

![img](https://pic1.zhimg.com/80/v2-32269e62f5245124cc751248d585bf50_1440w.webp)

第三种方法是/*language=regexp|jsregexp*/在字符串文字之前的注释 。这些注释需要一些打字，可能会污染你的代码，但另一方面，它会让读者明白你的意图，他们不会失去意识，任何人使用ReSharper打开你的代码将获得相同的功能标有字符串。顺便说一句，注释的格式与[基于](https://link.zhihu.com/?target=http%3A//www.jetbrains.org/pages/viewpage.action%3FpageId%3D983889)IntelliJ平台的IDE兼容 。

![img](https://pic2.zhimg.com/80/v2-efbc323524c81b05c5cd168864815b65_1440w.webp)

### 正则语法高亮显示

ReSharper强调正则表达式中的语法结构以及错误和冗余：

![img](https://pic4.zhimg.com/80/v2-a04800add7391fdb84239d42ee19e373_1440w.webp)

突出显示颜色具有以下含义：

> 浅蓝色 - 字符类，锚点和量词
> 浅绿色 - 分组结构
> 橙色 - 设置结构
> 粉红色和浅粉红色 - 转义序列
> 绿色 - 注释
> 红色与卷曲下划线 - 错误
> 蓝色的卷曲下划线 - 警告

当你将光标移动到其中一个字符时，会突出显示分组中的括号，分组名称和集合。你可以使用Environment->Editor->Highlight matching delimiter when caret is来切换和调整编辑器主题。

![img](https://pic4.zhimg.com/80/v2-58487d0102f1b0278f073ac9d04cbcd3_1440w.webp)

默认情况下，ReSharper在所有非逐字字符串中突出显示正确和不正确的转义序列：

![img](https://pic1.zhimg.com/80/v2-554e602a30b88e9ef577f6af64b8c2ac_1440w.webp)

如果不需要，可以通过禁用Code Inspection->Settings->Highlight special character in string literals 来关闭突出显示。

![img](https://pic1.zhimg.com/80/v2-5aca338db31751694046d3d96a90b5a0_1440w.webp)

### 修复错误

要修正正则表达式中的错误，将光标定位到突出显示的字符串上，然后按 Alt+Enter，然后选择相应的快速修复建议。

正则表达式错误最常见的例子是滥用转义字符。

![img](https://pic2.zhimg.com/80/v2-322f10f5c35a6204ab5600a33c480685_1440w.webp)

ReSharper可帮助自动修复错误：

![img](https://pic4.zhimg.com/80/v2-c897dd2e68bac5eb2c7d6c95bdd727d7_1440w.webp)

### 验证和测试

ReSharper允许你在设计时或调试时验证并测试正则表达式。在"Validate Regular Expression"对话框中，可以输入各种待测字符串，并查看正则表达式与这些字符串的匹配程度。该对话框在主菜单中可用 ReSharper->Tools->Validate Regular Expression这个对话框，来修复你的正则表达式，并确保你得到所需的正确匹配。

![img](https://pic2.zhimg.com/80/v2-589655d5feb35baf20c14529ded06549_1440w.webp)

ReSharper使用标准的.NET正则表达式引擎来处理表达式，其运行方式与运行时完全相同。以上示例字符串中的所有匹配都会突出显示，

此外，通过匹配，组中的匹配和组中的所有捕获（如果它们中有两个以上）匹配显示在树视图中。可以选择树中的节点以突出显示正则表达式中样本字符串和组的相应部分（如果选择了组或捕获）。

### 验证代码中的正则表达式

按下Alt+Enter或单击左侧的指示器以打开建议列表。

选择Validate Regular Expression上下文操作。

在打开的"Validate Regular Expression"对话框中，在"Test Input" 区域中提供一些示例字符串 。
要同时测试多个样本字符串，请用新行分隔字符串，然后选中 SingleLine 检查行复选框。请注意，在这种情况下，样本应该是单行字符串。

如有不需要，我们可以在Option下拉列表中更改引擎的正则表达式选项 。如果正则表达式按预期工作，便可将其复制重新插入代码中。

### 正则表达式的智能感知

ReSharper为几乎所有的.NET正则表达式构造提供了IntelliSense支持。在自动完成列表中，每个结构都以简要说明显示。

![img](https://pic1.zhimg.com/80/v2-27abeadc6f3b51472b134653ae6c40e0_1440w.webp)

在正则表达式中，Resharper提供了四种类型的IntelliSense：

> 自动完成 - 触发后 \， ( 和 [ 字符
> 基本完成 （Ctrl+Space） - 显示可用于当前范围的元素
> 智能完成 （Ctrl+Alt+Space） - 显示当前范围的最可能元素
> 双完成 （Ctrl+Space 两次） - 显示所有可能的元素

使用Match.Groups属性时，你还可以从ReSharper的智能感知中受益 。ReSharper检测表达式中的组名并在自动完成列表中建议：

![img](https://pic3.zhimg.com/80/v2-320709877c785fe9f4f9db11740a1836_1440w.webp)

### 提取预编译的正则表达式

如果你需要重用正则表达式（在Regex 该类的静态方法中使用该正则表达式），则可以将其提取到预编译的正则表达式。

要提取正则表达式，将光标移动到需要提取的正则表达式上，然后按下 Alt+Enter 并选择"To precompiled Regex" 上下文操作。

![img](https://pic4.zhimg.com/80/v2-0d961e8fd17325289303d83797bb3fa3_1440w.webp)

例如，我们可以从IsMatch 方法的pattern 参数中提取正则表达式 ：

```csharp
public void Bar()
{
    var result = Regex.IsMatch("Input", "Pattern");
}
```

在应用上下文动作之后，模式被提取到静态字段中：

```csharp
private static readonly Regex Regex1 = new Regex("Pattern");
public void Bar()
{
    var result = Regex1.IsMatch("Input");
}
```

### 语言注入

如果字符串文字（以及类似XML的语言中的标签或属性）包含其他一些正式语言，如正则表达式，HTML等，ReSharper可以提供代码检查， 快速修复， 代码完成， 上下文动作，以及语言的许多其他功能。

在C＃，JavaScript和TypeScript字符串中，ReSharper支持以下嵌入式语言：

> 常用表达式
> JavaScript
> HTML
> CSS
> JSON
> XML

ReSharper可以处理两种类型的语言注入：

### 自动语言注入

在某些情况下，可以明确检测到另一个语言文件中的语言摘录，例如<script></script> 标记内的JavaScript 或style HTML 中属性的CSS 。在这些情况下，ReSharper会自动检测嵌入式语言。

### 手动语言注入

当字符串文字中的形式语言不能自动检测到时，ReSharper允许你手动将文字标记为包含特定语言的方式如下：

使用上下文操作，它实际上告诉ReSharper标记与字符串对应的范围，将此范围保存在其配置的内部数据库中，并在包含文件更改时对其进行跟踪。这种方式非常快速和直接，但有两个缺点：在外部文件更改（如git合并）后范围可能会丢失，并且标记为注释的注入将仅在本地进行跟踪：

![img](https://pic2.zhimg.com/80/v2-9eb8a4ce23f60a42ac433c467728db59_1440w.webp)

/*language=javascript|html|regexp|jsregexp|json|css|xml*/ 在字符串文字之前放置注释 。当然，这些注释需要耗费时间打字，你甚至可以认为他们污染你的代码。但是，他们的目的是让你的同伴清楚你的意图，让他们不会迷路，任何人用ReSharper打开代码都会在标记的字符串中获得相同的功能。
顺便说一下，注释的格式与[基于](https://link.zhihu.com/?target=http%3A//www.jetbrains.org/pages/viewpage.action%3FpageId%3D983889)JetBrains Rider 和 [IntelliJ](https://link.zhihu.com/?target=http%3A//www.jetbrains.org/pages/viewpage.action%3FpageId%3D983889)平台的IDE兼容 。

![img](https://pic2.zhimg.com/80/v2-c00f7360eeb7ee99aa9f6b036a8663a5_1440w.webp)

你也可以在注释中使用 prefix= 和 postfix=参数。例如，如果一个字符串只包含一个CSS属性列表，则可以在它之前添加以下注释： //language=css prefix=body{ postfix=}。这将使ReSharper将字符串解析为有效的CSS。

### 代码模板

当你要写一个典型的代码块，如"for"或"foreach"循环，一个安全的类型转换，断言等，按VS快捷键 ：Ctrl+E,L Resharper快捷键：Ctrl+J，然后选择相应的现场模板。

![img](https://pic3.zhimg.com/80/v2-245254fa518e95435aea79cac4fff6ce_1440w.webp)

类似的，可以围绕现有的代码块创建典型的代码结构，如"if ... else‘，‘try ... catch"，等等。在这种情况下，按VS快捷键 ：Ctrl+E,U Resharper快捷键：Ctrl+Alt+J或Alt+Enter。

![img](https://pic1.zhimg.com/80/v2-fb8b5bf786e6844feeb522b5d5d29394_1440w.webp)

如果你发现ReSharper的代码模板有用，你可以添加新文件表单模板和创建自己的代码模板。

### 创建自己的代码模版

你可以在Resharper中创建自己的代码模版，比如m1代表生成一个参数的实例方法，可以这样创建：

打开代码模版管理器Templates Explorer

![img](https://pic1.zhimg.com/80/v2-97038e569cc9967ac9621c56cb014c3c_1440w.webp)

进入到模板管理器

![img](https://pic4.zhimg.com/80/v2-edbb6c651793bce87a2bdd52ea136917_1440w.webp)

然后选择左边的代码类型，比如我要创建C#的代码模版，就选择C#，现在你能看到这里面有很多自带的代码模版，然后点击创建模版按钮。

![img](https://pic1.zhimg.com/80/v2-92471d34599dd104adc8c8bbdda1ac90_1440w.webp)

然后在VS中打开模板编辑器窗口进行创建模版：

![img](https://pic3.zhimg.com/80/v2-d93ca11ea2e2930ca7692407f43ca96e_1440w.webp)

模板编辑完成后，直接Ctrl+S即可保存到自己的代码模版里。

模板说明：模版变量用$符号包裹起来，如果包裹的变量是代码的一些关键字，则会在代码生成时作为首选的关键字填充到相应的位置，如果不是，则会以相应的变量名填充到相应位置，$END$是内置模版变量，代表模板生成后鼠标光标最后停留的位置。

多看一下Resharper自带的其他代码模版，你就知道怎样去编写自己更多的代码模版了。

![img](https://pic4.zhimg.com/80/v2-efa068cf1c93fe78b89b910935e8745b_1440w.webp)

![img](https://pic3.zhimg.com/80/v2-0064c07b0dbd0997f6feae2d1603dfe6_1440w.webp)

### 代码风格问题

使用ReSharper，您可以控制代码中的大多数风格：命名标准，格式规则，文件类型布局，文件头风格，和许多其他小东西（比如改变顺序或使用"var"关键字）。

您即可以用ReSharper代码样式功能 - 所有默认值都是根据Microsoft准则和众多最佳做法选择的。同时，代码风格也都可以更改，以适应您的个人或企业偏好。

要应用代码风格规则，按VS快捷键 ：Ctrl+E,C Resharper快捷键：Ctrl+Alt+F。ReSharper会提示您选择两个默认值代码清理配置文件之一：1.重新设置代码风格。2在选定范围内应用多个代码样式规则。

### Resharper和VisualStudio性能优化建议

我相信很多人吐槽Resharper的性能问题，我想，一个可能的原因是打开的文档太多了，如果你有时刻关闭不需要的文档的习惯，性能或许不会那么差，并且你可以随时打开这些你关闭了的文档，就像在已经打开的文档中切换一样的方便。

我的团队中没有用到敏捷开发那些高级的东西，但是我们还是保持着每次改动都仅涉及两三个文件的好习惯，并且频繁的commit到源代码服务器上去。所以，我每次真正要编辑的文件不多，性能不是问题。

和大家分享了很多Resharper使用的技巧，点点滴滴都已经融入我日常的开发工作中了。当然很不全面，也很主观，我觉得它好，你可能觉得它不好，萝卜青菜各有所爱。再说，它也不是没有白痴的地方，在文档上点右键增加的一个Close All功能，可以关闭所有打开的文档，关闭了干什么，对着一个空白的屏幕发呆么？我觉得原生的“除此之外全部关闭”就够了。还有一个定位的功能（Locate in Solution Explorer），真是没用，如果你在VS选项中设置了，在解决方案管理器中跟踪活动项，那么VS自动就给你定位了。

不管怎么说，它带给我更快更方便的开发体验，把我从一些琐碎的，不人性化的功能中解放出来。从这一点上来说，我很希望越来越多的人喜欢上它，开始用它，并帮助它更好的发展。

### 禁用不必要的Resharper的功能

![img](https://pic1.zhimg.com/80/v2-82f5a1bed80e262c28db06517a093c38_1440w.webp)

我相信其实你也讨厌Resharper的拼写检查功能，以及国际化i18n你可能大部分项目都用不到，你也许仍然习惯用VS自带的编译器...，所以，把这些你认为不必要的功能禁用了吧。

### 禁用代码分析当前文件

你可以按暂时禁用代码分析当前文件的Ctrl + Alt + Shift + 8。再次按下该快捷方式将重新启用代码分析。你可以发现当前文件的状态指示灯代码分析的状态：

![img](https://pic4.zhimg.com/80/v2-1ac95a4ded012ca75e9674e890e51573_1440w.webp)

如果你要绑定一个不同的快捷方式进行此操作，请在VS中查找ReSharper_EnableDaemon命令。

### 禁用代码分析特定的文件

你可以告诉ReSharper跳过分析某些文件，而无需打开它们。例如，你可以跳过包含行之有效的cs类文件，比如算法相关的类，因为它不会经常发生大的变化。到ReSharper|Options，然后选择Code Inspection | Settings，点击Project to ignore下的Add，并使用弹出的对话框中挑中的文件和文件夹跳过。你也可以跳过指定的文件的文件掩码、最有可能的，你会发现，所有的文件，你禁用了代码分析的Ctrl + Alt + Shift + 8已经在那里。

### 关闭解决方案范围的分析

如果是非常大的项目，打开[解决方案范围的分析](https://link.zhihu.com/?target=http%3A//www.jetbrains.com/resharper/webhelp/Code_Analysis__Solution-Wide_Analysis.html)可能会导致性能急剧下降，特别是在配置不够高的机器上(如果你是[骨灰盒](https://link.zhihu.com/?target=https%3A//item.jd.com/33775402117.html)+[镶钻内存条](https://link.zhihu.com/?target=https%3A//item.jd.com/100003226984.html)的土豪用户，你可以尽情的玩)。如果你觉得这个代码分析功能占用了太多的硬件资源，只需将其关闭：右击Visual Studio的右下角，选择Analyze Errors in Solution或Pause Analysis。

一个对话框会弹出询问你是否要关闭提示。点'Yes'，你就大功告成了。

![img](https://pic3.zhimg.com/80/v2-87e832d1f0523b30aec8f63722a8ddaa_1440w.webp)



### 禁用上下文编辑动作

在ReSharper的选项，进入CodeEditing|ContextAction和CodeEditing| [语言] |上下文的动作，然后取消选中不那么对你有所帮助的选项。

### 禁止自动格式化

为了加快打字，你还可以禁用下自动格式化选项的ReSharper | Options | Environment | General ，以避免代码打字时被自动格式化：

![img](https://pic2.zhimg.com/80/v2-7d9850fff4e77f2e23c2a9c169e65af1_1440w.webp)

### 加快代码模板

为加快代码模板能快速地生成代码，你可以关掉Reformat和Shorten qualified Reference：

![img](https://pic1.zhimg.com/80/v2-a096fd16db901576987499f603c335e0_1440w.webp)

### 禁用单元测试

如果你不使用ReSharper的单元测试运行，可以通过关闭它节省处理时间。ReSharper|Options|Tools|UnitTesting，并取消相应的复选框：

![img](https://pic2.zhimg.com/80/v2-965bab882e69a0541749445bf024382d_1440w.webp)

### 关闭导航栏

如果您使用了File Structure窗口，那么你很有可能不再使用VS顶上的自带的导航栏。如果是这样，你可以通过取消选中相应的复选框以禁用 工具|选项|文本编辑器| C＃。

### 禁用VS的一些不必要的功能

从Resharper2018开始多了个PerformanceGuide选项，可以借助它调整VS的性能，根据自己的需求将某些功能给fix掉吧。

![img](https://pic4.zhimg.com/80/v2-cc34a3b37c6af1ebd1ce2c10f9bdb103_1440w.webp)

### 加快滚动编辑

用编辑器滚动的问题就出现了，由于硬件加速渲染的编辑器。如果您遇到这个问题，尝试下关闭下列选项工具|选项|环境|常规：

- 基于客户端性能自动调整视觉体验
- 如果可用，请使用硬件图形加速

![img](https://pic1.zhimg.com/80/v2-808a28d2ecea25a5aa8b1c49cbb9e538_1440w.webp)

### 节省时间启动

关闭起始页和新闻频道可能会节省一些时间启动。到工具|选项|环境并选择开机时显示空环境。

![img](https://pic4.zhimg.com/80/v2-59e136270b9015b0680660ccabb6df57_1440w.webp)

### 清除Web缓存

如果你在开发.NET Framework的Web项目，Web缓存可能会放缓的Visual Studio。清理，删除文件夹：％LOCALAPPDATA％\ MICROSOFT \ WebSiteCache。

### 禁用未使用的扩展

转到工具|扩展和更新，检查是否真的需要它们。您可以卸载或禁用未使用的一些扩展工具。

### 卸载未使用的项目

如果你不工作的一些项目，你可以从Visual Studio卸载他们，并在需要时重新加载他们。对项目或解决方案资源管理器解决方案文件夹，右键单击并选择卸载项目，或在解决方案文件夹卸载项目-这将同时加快Visual Studio和ReSharper。

### 禁用XAML可视化编辑器

在大型项目中，编辑XAML文件中可以感受到，即使在良好的硬件环境。如果你不使用可视化XAML编辑器，你可以部分通过禁用它解决问题。

在Solution Explorer中的XAML文件单击鼠标右键，然后选择打开方式。在出现的对话框中，选择源代码（文本）编辑器，然后单击设为默认值。

或者，去工具|选项|文本编辑器| XAML |杂项，然后取消选择总是完全XAML视图中打开的文档。

### 如果以上方法没有效果

如果你已经试过了上述的一切，表现仍下跌，你可以暂时禁用ReSharper，并检查是否有放缓的原因。禁用/启用ReSharper，到工具|扩展和更新| ReSharper ，点击禁用/启用。

如果禁用ReSharper有助于提高性能，但你还是要偶尔使用它的代码清理，格式化或分析，你可能想有一个快速切换ReSharper的开启和关闭的快捷方式：
转到工具|选项|环境|键盘并找到ReSharper_ToggleSuspended命令，然后按一些快捷键，然后单击分配：

![img](https://pic3.zhimg.com/80/v2-ce4dd3e6294b3d51f8a1460f2d4e05fa_1440w.webp)

或者，低配用户还是放弃Resharper吧。

### 遍历代码

您可以按Ctrl+T快速查找类型，方法或基本所有内容，同时Ctrl+Shift+T让您找到文件。

将您的光标放在using指令并按下Shift+F12。Resharper将显示这个命名空间的使用位置（查找变量的用法）。

忘记你刚才在编辑的地方？转到最后编辑位置用Ctrl+Shift+Backspace。

想要定位当前变量的真实位置？按F12或右键单击该变量名。

转到包含声明（Ctrl+[）可与被用于Shift以选择整个声明。

在定位CustomerServicesTest使用Ctrl+T或任何其他导航命令时，您不需要键入完整的类名。只需使用CamelHumps并键入cst。

Alt+Home将您引导至基本类型并将Alt+End您引导至当前类型的继承者。

你想转到班级中的下一个成员吗？Alt+Down会带你到那里;Alt+Up会带你回来。

搜索任何东西（用法，实现，范围外部的代码等）都会提取到查找结果窗口。然后使用它在搜索结果与F8/Shift+F8之间导航。

在源代码中，Shift+Alt+L在解决方案资源管理器中选择当前文件;在反编译的源代码中，它会打开关注当前类型的AssemblyExplorer窗口。

要浏览当前在剪贴板中的堆栈跟踪，只需按Ctrl+E,T。

开始在ReSharper工具窗口中输入，内容将缩小到匹配的项目。CamelHumps匹配工作在那里。

使用GoToFile（Ctrl+Shift+T）在解决方案资源管理器中查找特定项目-只需选择一个.csproj文件。

在查找类型时Ctrl+T，可以使用通配符。想要所有的ViewModels？键入*ViewModel。

### 单元测试

使用Ctrl+U,L解决方案中的运行所有的单元测试。

想要运行一些特定的测试？在编辑器中选择它们，右键单击并选择运行单元测试。

开始在单元测试资源管理器窗口中输入，按名称过滤测试。

在单元测试会话窗口中运行时筛选失败的测试，以便在它们通过时看到它们愉快地消失。

![img](https://pic4.zhimg.com/80/v2-799c1c48ee6bf377466308db9921026b_1440w.webp)

![img](https://pic4.zhimg.com/80/v2-9f4dee25aaa21e14286ebaab0d42b0d3_1440w.webp)

![img](https://pic1.zhimg.com/80/v2-d4b9d2a279a4fe11890cff546009b094_1440w.webp)

选中不需要参与单元测试覆盖率计算的类可以进行排除。

![img](https://pic2.zhimg.com/80/v2-592cdbabedeb13d21ee560284aaffd51_1440w.webp)

### Resharper插件管理器

Resharper还提供了属于Resharper的扩展插件，这么说来它就是VS中插件中的插件了，Resharper的插件管理器也提供了额外的Resharper强化插件，比如计算代码圈复杂度的、StyleCop、异常检查等插件。打开Resharper->Extension Manager；

![img](https://pic3.zhimg.com/80/v2-281a9f9595981b7e78012fd2813f5bfa_1440w.webp)

### 协同编译

Resharper提供了项目的协同编译，课取代VisualStudio自带的编译器，协同编译的好处在于，它能够并行的编译解决方案，可以实时看到编译进度以及项目的健康状态，编译结果背景色的不同代表项目的不同健康状态，如红色代表编译失败，黄色代表项目中有警告，绿色代表项目完美。

Resharper默认是不会使用协同编译的，需要在Resharper->Options->Tools->Build->General中启用ResharperBuild

![img](https://pic3.zhimg.com/80/v2-de1f80016fbc4940cc68edd70b1302ae_1440w.webp)

启用后，编译项目时便会使用Resharper的协同编译进行项目的编译，

![img](https://pic4.zhimg.com/80/v2-76dac37c2d0e8c38e09d3273182e492f_1440w.webp)

若后期需要使用VisualStudio自带的编译器，则在Build&Run窗口将Use“ResharperBuild”取消状态即可，

![img](https://pic3.zhimg.com/80/v2-8faaf8730219a15ab3f890d58bfb768e_1440w.webp)

### todo管理器(Ctrl+Alt+.)

Resharper中还能更直观的管理项目中的todo标签，方便你在项目研发时能更及时的定位todo标记，虽然VisualStudio也自带有这样的功能，但相比Resharper的，Resharper提供了更多的操作选项，比如可以将各种todo标记按你想要的规则进行分组排序等，包含一些异常标记也能以列表形式呈现给你；

使用菜单Resharper->Windows->Todo Explorer或快捷键Ctrl+Alt+>可打开todo管理器；

![img](https://pic2.zhimg.com/80/v2-ecaedd3ac1fce67485d18e3cc0f92c99_1440w.webp)

### 调试器实时变量视图

当你在调试代码的时候，是否经常去右键变量，添加快速监视？而Resharper则提供了在你调试代码的时候，可以直接看到你走每一步时，变量的结果是什么，这样你便不需要频繁的去添加快速监视；

![img](https://pic2.zhimg.com/80/v2-4d5eb6e99d5032d30ccde3209cc3fd1d_1440w.webp)

如果你想查看某个对象的变量详细信息时，点击变量视图，即可展开；

![img](https://pic2.zhimg.com/80/v2-fb65e74c397347a709f6895ae7dc5245_1440w.webp)

怎么样，是不是很方便？

### 反编译

总会有时候你需要反编译，曾经你肯定用过ILSpy、Reflector、dnSpy等反编译工具，Resharper也给你提供了反编译，很方便，能方便到什么程度？还记得VisualStudio自带的F12转到定义么？当有了Resharper，转到定义便可变成反编译直接看到源代码，管他是不是开源项目！Resharper默认也是关闭了反编译功能的，需要到Resharper->Options->Tools->External Source开启，如果你还勾选了Allow Downloading from remote locations，Resharper还会帮你去网上查找源代码并下载下来。

![img](https://pic2.zhimg.com/80/v2-de2b3b3f31669d3b29f578b890c7e7b9_1440w.webp)

![img](https://pic4.zhimg.com/80/v2-4fe8e95d78c87741e9118d75d84cd7cb_1440w.webp)

反编译结果：

![img](https://pic4.zhimg.com/80/v2-ed8938b636cf7e709cdc2ba74d6d6743_1440w.webp)

### [http://ASP.NET](https://link.zhihu.com/?target=http%3A//ASP.NET)和ASP.NETMVC

在ASP.NETMVC应用程序中，输入return View("并按下Ctrl+Space。智能感知会列出所有可用的视图。

键入rta并按下TAB。填入控制器，然后填入动作参数。现在它应该按照IntelliSense正确的顺序！

想要检查ASP.NETMVC中丢失的视图？打开解决方案范围的分析。View("Login")如果Login.cshtml不存在，将显示为红色。

您也可以在ASPX/Config文件中使用"转到文件成员"命令。按下Alt+\并查找它！

### 转换代码

您可以在ReSharper中定义要使用的上下文操作，Resharper->Options->Code Editing->Context Actions->[语言]。

![img](https://pic3.zhimg.com/80/v2-0bea86c96d7aa175067505b53203070e_1440w.webp)

你在同一个文件中有多个类吗？快速修复它。按下Ctrl+Shift+R解决方案资源管理器中的文件，然后选择将类型移入匹配文件。

随时随地重命名任何东西F2。即使用更少的步骤也可以做到-只需输入一个新名称并点击即可Alt+Enter。

您可以使用一段代码提取方法Ctrl+Alt+M。

想要将字符串文字移动到资源文件？按Ctrl+Shift+R字符串上的任意位置并选择移动到资源。

输入新的方法签名（更改参数的数量或类型，更改返回类型），签名用灰色框突出显示时，点击Alt+Enter应用更改签名重构。

把你的光标放在一个属性上，你可以按Alt+Enter它将它从自动属性改变为具有后台字段的属性，反之亦然。

按Ctrl+R,S以更改签名的方法，看到一个预览应用之前。ReSharper会完成剩下的工作！

认为你的代码需要一次清理？使用Ctrl+Alt+F并运行完整清理配置文件。

### 分析代码

![img](https://pic3.zhimg.com/80/v2-195e6e8084a4b21c3a16eec85fea51ea_1440w.webp)

甚至在运行代码之前使用[NotNull]和[CanBeNull]属性可以帮助您找到NullReferenceException。

右键单击解决方案资源管理器中的文件，项目，解决方案文件夹或整个解决方案，然后选择查找代码问题以查看所选项目的错误，警告和建议。

为突出的代码问题而感到困扰？Alt+Enter在突出显示的代码处按下并选择检查[Check Name]，然后您可以选择使用注释或属性来抑制问题，或者禁用相应的代码检查。

你可以使用单个注释标记代码来抑制所有检查-标记代码与//ReSharperdisableAllReSharper不会抱怨什么，直到它符合相应的//ReSharperrestoreAll。

ReSharper的解决方案范围分析解决了可见性问题：您会看到是否在组件外部使用内部成员，并且永远不会错过单个未使用的非私有成员。

您可以在ReSharper|中的代码分析中通过掩码排除文件选项|代码检查|设置|跳过的元素。

按/可以转到文件中的下一个/上一个代码问题。Alt+PageDown，Alt+PageUp

要在您的解决方案中查找所有可本地化的字符串，请在相关项目的属性中设置Localizable=Yes和LocalizableInspection=Pessimistic，然后找到任何此类sting，用下划线突出显示，按下Alt+Enter并选择Inspection'ElementisLocalizable'|查找类似的问题。

### 处理不同的语言版本

随着编程语言的发展，用新的语言特性改进代码是很自然的。另一方面，可能有些因素会阻止您升级到最新的语言版本。

ReSharper支持不同的编程语言。它分析代码并根据当前语言版本应用其自己的功能。每种语言都会自动检测语言版本，但您可以按照以下说明为某些语言手动设置版本。

### C＃

Resharper完全支持所有C＃版本，默认情况下，ReSharper根据关联的编译器自动检测C＃版本。但是，您可以在VisualStudio的"属性"窗口中使用C＃语言级别属性来显式指定目标C＃版本。

### [http://VB.NET](https://link.zhihu.com/?target=http%3A//VB.NET)

Resharper完全支持所有[http://VB.NET](https://link.zhihu.com/?target=http%3A//VB.NET)版本，默认情况下，ReSharper会根据关联的编译器自动检测[http://VB.NET](https://link.zhihu.com/?target=http%3A//VB.NET)版本。但是，您可以通过在解决方案资源管理器中选择项目并使用VisualStudio的"属性"窗口中的[http://VB.NET](https://link.zhihu.com/?target=http%3A//VB.NET)语言级别属性来显式指定目标[http://VB.NET](https://link.zhihu.com/?target=http%3A//VB.NET)版本。

### TypeScript

如果TypeScript版本是自动检测的（默认情况下是这样），并且在您的解决方案中有几个不同TypeScript版本的项目，ReSharper将使用整个解决方案的最高版本。

Resharper完全支持所有TypeScript版本。ReSharper<TypeScriptToolsVersion>根据VisualStudio项目文件中的属性自动检测TypeScript版本。但是您可以使用CodeEditing|TypeScript|Inspections上的TypeScript语言级别选择器明确指定目标TypeScript版本

### JavaScript

Resharper完全支持JavaScript到ECMAScript，包括实验性功能，如async/await，指数运算符和对象文字/解构literals/destructuring，rest/spread。jQuery和JSX语法也被支持。

默认情况下，代码检查和其他ReSharper功能根据通用支持的ECMAScript5标准分析JavaScript代码。如果您在项目中使用更高级的JavaScript代码，则可以在CodeEditing|JavaScript|Inspections。

### C++

C++支持可以通过ReSharperC++获得-这是一种专用产品，您可以单独安装，也可以与ReSharper或ReSharperUltimate一起安装。

Resharper支持C，C++，ReSharper根据平台工具集（项目属性中的General|PlatformToolset）/std开关

### CSS

Resharper支持CSS。实际上，CSS版本远不如由不同web浏览器支持的CSS功能集重要。因此，ReSharper可以让您针对特定Web浏览器的特定版本调整其CSS代码检查。您可以在CodeEditing|CSS|Inspections。

### 已知的兼容性问题

### 其他Visual Studio扩展

主要的兼容性问题已经观察到了以下产品：

- DevExpress CodeRush/Refactor Pro (incompatible)
- Telerik JustCode (incompatible)
- Whole Tomato Visual Assist

性能下降已经观察到了以下产品

- Some versions of the StyleCop ReSharper plug-in
- PowerCommands for Visual Studio

### 运行Parallels Desktop的Mac

如果你正在运行在Mac上使用的Parallels Desktop的Windows虚拟机上装的Visual Studio，ReSharper的智能感知列表可能会呈现很慢。

如果这种情况发生在你的设置中，考虑从相干模式切换到全屏模式。用于在两个模式之间进行切换的准则，请参阅[Parallels的知识库条目](https://link.zhihu.com/?target=http%3A//kb.parallels.com/en/115171)。

### 总结

关于性能问题，很多人说大内存+SSD的Resharper性能问题依然很卡，我想，一个可能的原因是打开的文档太多了，如果你有时刻关闭不需要的文档的习惯，性能或许不会那么差，并且你可以随时打开这些你关闭了的文档，就像在已经打开的文档中切换一样的方便。

我们必须保持每次改动都仅涉及两三个文件的好习惯，并且频繁的commit到源代码服务器上去。这样我们每次真正要编辑的文件不多，性能不是问题。

和大家分享了很多Resharper使用的技巧，点点滴滴都已经融入我日常的开发工作中了。这篇文章很主观，我觉得它好，你可能觉得它不好，萝卜青菜各有所爱。再说，它也不是没有白痴的地方，在文档上点右键增加的一个CloseAll功能，可以关闭所有打开的文档，关闭了干什么，对着一个空白的屏幕发呆么？我觉得原生的“除此之外全部关闭”就够了。还有一个定位的功能（Locatein Solution Explorer），真是没用，如果你在VS选项中设置了，在解决方案管理器中跟踪活动项，那么VS自动就给你定位了。
不管怎么说，它带给我更快更方便的开发体验，把我从一些琐碎的，不人性化的功能中解放出来。从这一点上来说，我很希望越来越多的人喜欢上它，开始用它，并帮助它更好的发展。

### 下一步

你可以签出在GitHub上的Resharper工作集。这是一个Visual Studio解决方案，提供一步一步的代码练习导航，编辑，检查，重构等等。

[https://github.com/JetBrains/resharper-rider-samples](https://link.zhihu.com/?target=https%3A//github.com/JetBrains/resharper-rider-samples)

### 分享一个自己已经定制好的Resharper首选项配置

Resharper->Manage Options->Import from File,选择配置文件导入即可。

![img](https://pic1.zhimg.com/80/v2-cb867f4929b3711aef134871a9894148_1440w.webp)

![img](https://pic2.zhimg.com/80/v2-702c37886bf3ec0ed45438008b5a6f15_1440w.webp)