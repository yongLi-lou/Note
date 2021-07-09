# Windows类似HomeBrew的宝管理工具--Scoop

```shell
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')

# or shorter
iwr -useb get.scoop.sh | iex
```

Note: if you get an error you might need to change the execution policy (i.e. enable Powershell) with

```shell
Set-ExecutionPolicy RemoteSigned -scope CurrentUser
```



### Demo

>
>
>```shell
>scoop install vim
>scoop help
>```
>
>

