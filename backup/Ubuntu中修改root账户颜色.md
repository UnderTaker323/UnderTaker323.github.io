
root账户下编辑/etc/bash.bashrc文件
```
vim /etc/bash.bashrc
```
添加以下内容保存退出
```
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;31m\]\u\[\033[01;33m\]@\h\[\033[00m\]:\[\033[01;32m\]\w\[\033[00m\]\$ '
```
执行后生效
```
source /etc/bash.bashrc
```


