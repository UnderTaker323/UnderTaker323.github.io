卸载老版本Docker
```
apt-get remove docker docker-engine docker.io containerd runc
```

更新下载源
```
apt update
```

安装更新软件包
```
apt upgrade
```

安装docker依赖
```
apt-get install ca-certificates curl gnupg lsb-release
```

添加Docker官方GPG密钥
```
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```

更新docker软件源
```
add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```

安装Docker
```
apt-get install docker-ce docker-ce-cli containerd.io
```

配置用户组（选择配置）
```
usermod -aG docker $USER
```

*默认情况下，只有root用户和docker组的用户才能运行Docker命令。我们可以将当前用户添加到docker组，以避免每次使用Docker时都需要使用sudo。*
###### 注：重新登录后才能使更改生效。

启动Docker
```
systemctl start docker
```

查看Docker状态
```
systemctl status docker
```

安装工具
```
apt-get -y install apt-transport-https ca-certificates curl software-properties-common
```

重启Docker
```
service docker restart
```

验证是否成功
```
docker run hello-world
```