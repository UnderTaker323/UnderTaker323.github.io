**先安装docker**

两张网卡

- 桥接网络
- 内部网络

master配置：
```
vim  /etc/network/interfaces
```

```
auto enp0s3
iface enp0s3 inet dhcp

auto enp0s8
iface enp0s8 inet static
address 192.168.1.10
```
slave配置：
```
vim  /etc/network/interfaces
```

```
auto enp0s3
iface enp0s3 inet dhcp

auto enp0s8
iface enp0s8 inet static
address 192.168.1.20
```
主机名hostname修改
```
vim /etc/hostname
```

/etc/hostname修改
```
127.0.1.1   k8s-master/slave

192.168.1.10 k8s-master
192.168.1.20 k8s-slave
```

添加kubernetes源
```
vim /etc/apt/sources.list
```

添加
```
deb https://mirrors.aliyun.com/kubernetes/apt kubernetes-xenial main
```

保存更新apt
```
apt update
```

安装以下package：
```
apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```

**以上可以直接在一台Ubuntu16.04上安装然后克隆**

安装kubeadm

```
apt-get install kubelet=1.17.4-00 kubeadm=1.17.4-00 kubectl=1.17.4-00 --allow-unauthenticated
```

如果遇到E:无法定位软件包...
则先执行
```
apt-get update
```

*master节点和salve节点都要执行* 

上传Kubernetes所需镜像：
**master节点**
>coredns.tar
etcd.tar
kube-apiserver.tar
kube-controller-manager.tar
kube-proxy.tar
kube-scheduler.tar
pause.tar

**slave节点**
>coredns.tar
kube-proxy.tar
pause.tar

上传镜像
***master**
```
docker load -i coredns.tar
docker load -i etcd.tar
docker load -i kube-apiserver.tar
docker load -i kube-controller-manager.tar
docker load -i kube-proxy.tar
docker load -i kube-scheduler.tar
docker load -i pause.tar
```

**slave**
```
docker load -i coredns.tar
docker load -i kube-proxy.tar
docker load -i pause.tar
```

### master节点初始化
先执行
```
swapoff -a
```

初始化
```
kubeadm init --pod-network-cidr=10.244.0.0/16 --kubernetes-version=v1.17.4
```
***如果初始化失败，重新执行上述命令的时候需重置kubeadm***
*重置kubeadm命令*
```
kubeadm reset
```

配置kubectl
```
mkdir -p $HOME/.kube
```

```
cp -i etc/kubernetes/admin.conf $HOME/.kube/config
```
```
chown $(id -u):$(id -g) $HOME/.kube/config
```
**此条命令因为格式有问题，请直接复制初始化成功后生成的内容**

### slave节点
先执行
```
swapoff -a
```
执行
```
kubeadm join 192.168.1.6:6443 --token pbic2f.ognkfgyirim1xh6f \ --discovery-token-ca-cert-hash sha256:11a12bd4fb38fec71c1f7a06723fdce21fcf0e203b2355bf43759cc3fba88f89
```

***注：上述代码为master节点初始化完成后最后的内容，不要将上述代码直接复制进去，在master节点初始化完成后的最后找到相似内容执行***

**在master节点查看**
```
kubectl get nodes
```

```
kubectl get pods -n kube-system -o wide
```

在**master**与**slave**节点导入以下镜像
>calico-cni.tar
calico-node.tar
calico-pod2daemon-flexvol.tar

上传镜像
```
docker load -i calico-cni.tar
docker load -i calico-node.tar
docker load -i calico-pod2daemon-flexvol.tar
```

部署canal
```
kubectl apply -f https://docs.projectcalico.org/v3.10/getting-started/kubernetes/installation/hosted/canal/canal.yaml
```

**因为上述部署方式可能无法启动对应容器，所以执行下面的方法**
先获取yaml文档
```
wget https://docs.projectcalico.org/v3.10/getting-started/kubernetes/installation/hosted/canal/canal.yaml
```

对该文档进行修改
```
vim canal.yaml
```
将3.10.4修改成3.10.3保存退出

执行
```
kubectl apply -f canal.yaml
```

**在master节点查看**
```
kubectl get nodes
```

```
kubectl get pods -n kube-system -o wide
```