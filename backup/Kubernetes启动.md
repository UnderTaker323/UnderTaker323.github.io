禁用交换空间
***master和slave都需要执行***
```
swapoff -a
```

查看kubelet状态
```
systemctl status kubelet
```

查看节点
```
kubectl get nodes
```

查看pod
```
kubectl get pods
```