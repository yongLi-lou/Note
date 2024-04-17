# 在Linux中使用nginx发布前端项目

```bash
cd /usr/local
mkdir nginx
cd nginx
```

- 安装依赖包：`yum -y install gcc zlib zlib-devel pcre-devel openssl openssl-devel`

- 下载并解压Nginx安装包：`wget https://nginx.org/download/nginx-1.25.1.tar.gz && tar -xvf nginx-1.25.1.tar.gz`

- 进入Nginx目录并安装：`cd nginx-1.25.1 && ./configure --with-http_stub_status_module --with-http_ssl_module && make && make install`

- 启动Nginx服务：`systemctl start nginx`
- 设置Nginx开机自启：`systemctl enable nginx`
- 查看Nginx启动状态：`systemctl status nginx`
- 备份默认配置文件：`cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak`
- 编辑新的配置文件：`vim /etc/nginx/nginx.conf`

例：要将位于`/usr/local/mes/web`目录下的`index.html`文件发布到`8862`端口，并设置域名为`cyerp.cicadayun.com`

- 找到`nginx.conf`文件的位置，并将其用`vi`命令打开。

- 在

  ```
  http
  ```

  部分中添加一个新的

  ```
  server
  ```

  块，如下所示：

  ```bash
  server {
      listen 8862;
      server_name cyerp.cicadayun.com;
      
      location / {
          root /usr/local/mes/web;
          index index.html;
      }
  }
  ```

- 保存并退出。