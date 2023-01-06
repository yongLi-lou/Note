# Win11打开PowerShell找不到mscoree.dll

在**管理员：命令提示符**窗口，输入以下命令：

dism /online /enable-feature /featurename:netfx3 /all

dism /online /enable-feature /featurename:WCF-HTTP-Activation

dism /online /enable-feature /featurename:WCF-NonHTTP-Activation

#### 如果执行第二个命令时操作完成，但未启用 WCF-NonHTTP-Activation 功能。在控制面版开启所有.netFramework 3.5功能即可