# windows10下安装docker报错：error during connect

详细报错信息如下：

```
C:\Users\zig>docker info
error during connect: Get http://%2F%2F.%2Fpipe%2Fdocker_engine/v1.39/info: open //./pipe/docker_engine: The system cannot find the file specified. In the default daemon configuration on Windows, the docker client must be run elevated to connect. This error may also indicate that the docker daemon is not running.
12
```

修改方法：

```
cd "C:\Program Files\Docker\Docker"
./DockerCli.exe -SwitchDaemon
12
```

原因：Especially on windows machine when you see the above error after a docker update, try the above commands. It appears like the Docker Desktop UI may indicate that you are already using Linux Containers, but the update may have messed up that setting. Running the above commands will set to Linux Containers and there after you can work happily.
大概意思就是默认使用的是Linux Containers，使用这个命令后改为Windows Containers就好了。