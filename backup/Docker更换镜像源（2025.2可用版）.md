创建目录（已有则忽略）
```
mkdir -p /etc/docker
```

打开配置文件
```
vim /etc/docker/daemon.json
```
***若`daemon.json`文件不存在，可以直接使用`vim`命令创建*** 
粘贴以下内容
```
{
    "registry-mirrors": [
    	"https://docker.m.daocloud.io",
    	"https://docker.imgdb.de",
    	"https://docker-0.unsee.tech",
    	"https://docker.hlmirror.com",
    	"https://docker.1ms.run",
    	"https://func.ink",
    	"https://lispy.org",
    	"https://docker.xiaogenban1993.com"
    ]
}
```

重启Docker服务
```
systemctl daemon-reload
```
```
systemctl restart docker
```