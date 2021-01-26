# Docker 安装完成后测试hello-world出现问题：Unable to find image 'hello-world:latest' locally

## 异常原因

  docker在本地没有找到hello-world镜像，也没有从docker仓库中拉取镜像，出项这个问题的原因是因为docker服务器再国外，我们在国内，无法正常拉取镜像，所以就需要我们为docker设置国内阿里云的镜像加速器。

## 三、解决方法

- 1、修改/etc/docker/daemon.json 文件

```json
 "registry-mirrors": [
    "https://alzgoonw.mirror.aliyuncs.com"
  ]
```

- 2、重启docker之后就可以了