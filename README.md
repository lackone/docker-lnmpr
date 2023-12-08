# docker-lnmpr

基于docker的 nginx + mysql + php + redis + rabbitmq

### 一、如何使用

```
git clone https://github.com/lackone/docker-lnmpr.git
```

```
cd docker-lnmpr
docker compose up -d
```

### 二、修改.env文件，配置自已需要的软件版本

nginx的版本，任意指定

php的版本，只支持大号版本，如 5.6，7.1，7.2，7.3，7.4，8.0，8.1，8.2

mysql的版本，任意指定，默认密码 123456

redis的版本，任意指定

rabbitmq的版本，任意指定

### 三、常用命令

```shell
创建服务，并在后台运行容器
docker compose up -d
指定创建服务，并在后台运行容器
docker compose up -d 服务名1 服务名2 服务名3
启动服务
docker compose start 服务名
停止服务
docker compose stop 服务名
重启服务
docker compose restart 服务名
重新构建服务
docker compose build 服务名
删除服务
docker compose rm 服务名
停止并删除容器
docker compose down
```

### 四、配置一个虚拟主机

在 nginx/conf.d 目录下创建一个 www.conf 文件，内容如下：

```
server {
    listen 80;
    server_name 192.168.1.113;
    root /wwwroot;

    index index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_pass php:9000;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

server_name 根据自已的IP来配。

然后在 wwwroot 目录下，创建一个 index.php 文件，内容如下：

```php
<?php 
    phpinfo();
?>
```

然后重启容器

```
docker compose restart nginx
```

如果不生效，强制重建容器

```
docker compose up -d --force-recreate nginx
```

### 五、如果出现如下错误：

```
WARNING: Ignoring https:///alpine/v3.17/main: DNS lookup error
WARNING: Ignoring https:///alpine/v3.17/community: DNS lookup error
```

请 vi /etc/docker/daemon.json 添加 DNS

```json
{
  "dns": [
    "8.8.8.8", "114.114.114.114"
  ]
}
```

```
ERROR: unable to select packages error on Alpine Linux
```

请在 php/Dockerfile 文件中打开
```
RUN echo "http://${REPOSITORIES}/alpine/v3.18/main" >> /etc/apk/repositories
RUN echo "http://${REPOSITORIES}/alpine/edge/main" >> /etc/apk/repositories
```