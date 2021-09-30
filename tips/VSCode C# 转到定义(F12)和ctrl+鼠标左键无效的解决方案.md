# VSCode C# 转到定义(F12)和ctrl+鼠标左键无效的解决方案

使用VsCode开发.net5 服务时碰到C#的类、方法等使用ctrl+鼠标左键都无法跳转的问题，度了一下终于找到解决方法：

文件夹中可能有多个"项目",VSCode选择了"错误"项目.

使用ctrl-shift-P并选择"OmniSharp:选择项目"以选择正确的项目(.sln文件).

如果选择"OmniSharp Logs"打开"输出"窗口,您将看到它正在读取您的csproj.完成后,您的goto定义将开始起作用
