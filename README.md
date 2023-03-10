# docker-lnmpr

基于docker的nginx+mysql+php+redis

### 一、如何使用

```
git clone https://github.com/lackone/docker-lnmpr.git
```

```
cd docker-lnmpr
docker compose up -d
```

### 二、修改.env文件，配置自已需要的软件版本

注意，php的版本，只支持大号版本，如 5.6，7.1，7.2，7.3，7.4，8.0，8.1，8.2

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

### 四、如果出现如下错误：

```
WARNING: Ignoring https:///alpine/v3.17/main: DNS lookup error
WARNING: Ignoring https:///alpine/v3.17/community: DNS lookup error
```

请 vi /etc/docker/daemon.json 添加 DNS

```json
{
  "dns": [
    "8.8.8.8"
  ]
}
```