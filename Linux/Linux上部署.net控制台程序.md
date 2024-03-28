# Linux上部署.net控制台程序

在 Linux 上部署 .NET 6 控制台项目的步骤如下：

1. [**配置 .NET 环境**](https://blog.csdn.net/dawfwafaew/article/details/133944475)[1](https://blog.csdn.net/dawfwafaew/article/details/133944475)[2](https://www.cnblogs.com/yuluowuhun/p/16095258.html)：

   - 添加 Microsoft 包存储库：运行以下命令，将 Microsoft 包签名密钥添加到受信任密钥列表，并添加 Microsoft 包存储库。

     ```bash
     sudo rpm -Uvh https://packages.microsoft.com/config/centos/7/packages-microsoft-prod.rpm
     ```

     

   - 安装 SDK：.NET SDK 使你可以通过 .NET 开发应用。如果安装 .NET SDK，则无需安装相应的运行时。要安装 .NET SDK，请运行以下命令：

     ```bash
     sudo yum install dotnet-sdk-6.0
     ```

     

   - 安装运行时：通过 ASP.NET Core 运行时，可以运行使用 .NET 开发且未提供运行时的应用。以下命令将安装 ASP.NET Core 运行时，这是与 .NET 最兼容的运行时。在终端中，运行以下命令：

     ```bash
     sudo yum install aspnetcore-runtime-6.0
     ```

     

   - 检查 SDK 版本：可使用终端查看当前安装的 .NET SDK 版本。打开终端并运行以下命令。

     ```bash
     dotnet --list-sdks
     ```
     2. **创建文件夹** ：
     
        在 Linux 服务器上，你可以将发布的项目文件放在以下几个推荐的目录中：
     
        1. [**/opt**](https://zhuanlan.zhihu.com/p/145270349)[1](https://zhuanlan.zhihu.com/p/145270349)：这个目录通常用于存放第三方的或额外的软件和应用。你可以在这个目录下为你的项目创建一个新的子目录。
        2. [**/usr/local**](https://zhuanlan.zhihu.com/p/145270349)[1](https://zhuanlan.zhihu.com/p/145270349)：这个目录通常用于存放系统管理员安装的本地应用程序。你也可以在这个目录下为你的项目创建一个新的子目录。
        3. [**/home**](https://zhuanlan.zhihu.com/p/145270349)[2](https://www.zhihu.com/question/298683851)：这个目录是用户的主目录，你可以在你的用户目录下创建一个新的子目录来存放你的项目。
     
     3. **上传文件**：然后，你可以使用 `scp` 命令将你的本地文件上传到刚刚创建的目录。假设你的服务器 IP 地址是 `server_ip`，你的服务器用户名是 `username`，你可以在本地电脑的命令行中运行以下命令：
     
     ```bash
     scp -r C:\Users\LYL\IIS\sapmes username@server_ip:/usr/api
     ```
     
     [`scp` 命令的 `-r` 选项会递归地复制整个目录及其内容](https://www.runoob.com/linux/linux-comm-scp.html)[1](https://www.runoob.com/linux/linux-comm-scp.html)[2](https://blog.csdn.net/qq_34374664/article/details/81289540)[3](https://bing.com/search?q=scp命令复制文件夹的行为)。如果你只想复制目录中的文件，而不是整个目录，你可以在源目录路径的末尾添加 `/*`。例如：
     
     ```bash
     scp -r /path/to/local/folder/* user@remote:/path/to/destination
     ```
     
     
     
     [这个命令会将 `folder` 目录下的所有文件和子目录复制到远程服务器的 `destination` 目录，而不会复制 `folder` 目录本身](https://www.runoob.com/linux/linux-comm-scp.html)
     
     这个命令会将 `C:\Users\LYL\IIS\sapmes` 目录下的所有文件和子目录复制到服务器的 `/root/api` 目录下
     
     4. **运行**：
     
        ```bash
        nohup dotnet cy_mes_erp_api.dll --urls="http://*:5000" &
        ```
     
        
     
        这个命令会启动你的应用程序，并监听 5000 端口。
     
        1. [**查找进程 ID**：首先，你需要找到你的应用程序的进程 ID。你可以使用 `ps -aux | grep "cy_mes_erp.dll"` 命令来查找你的应用程序的进程 ID](https://www.cnblogs.com/jayjiang/p/12610588.html)[1](https://www.cnblogs.com/jayjiang/p/12610588.html)[2](https://blog.csdn.net/weixin_39777626/article/details/103292882)。
     
        2. [**关闭进程**：然后，你可以使用 `kill` 命令和你的进程 ID 来关闭你的应用程序。例如，如果你的进程 ID 是 35520，你可以使用以下命令来关闭你的应用程序](https://www.cnblogs.com/jayjiang/p/12610588.html)[1](https://www.cnblogs.com/jayjiang/p/12610588.html)：
     
           ```bash
           kill 35520
           ```
     
           
     
        这个命令会发送一个信号给你的应用程序，告诉它需要关闭。
     
     5. **查看输出**：
     
        ``` shell
        [root@iZbp19q4ng9lionubf5awsZ sapmes]# nohup dotnet cy_mes_erp_api.dll --urls="http://*:5000" &
        [1] 1534
        [root@iZbp19q4ng9lionubf5awsZ sapmes]# nohup: ignoring input and appending output to ‘nohup.out’
        ```
     
        已经成功地在后台启动了你的 .NET 应用程序，并且它正在监听 5000 端口。`nohup` 命令会忽略输入并将输出追加到 `nohup.out` 文件中。你可以通过查看 `nohup.out` 文件来查看你的应用程序的输出。例如，你可以使用 `cat nohup.out` 命令来查看输出。

