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

### 如果出现如下错误：

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