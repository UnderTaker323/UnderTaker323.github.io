### Ubuntu
更新下载源
>sudo apt update

安装ssh服务
>sudo apt install openssh-server

修改ssh登录配置
>sudo vim /etc/ssh/sshd_config

- 将`Port 22`前面的`#`删除 
- 将`PermitRootLogin `后面修改为`yes`

开启ssh防火墙服务端口
>sudo ufw allow ssh

开启ssh服务
>systemctl start ssh

开启ssh服务
>systemctl stop ssh

查看ssh服务状态
>systemctl status ssh

设置开机自启动
>systemctl enable ssh

关闭开机自启动
>systemctl disable ssh

### Centos
查看是否已经安装ssh
>rpm -qa | grep -E "openssh"

或者
>yum list installed | grep openssh

若已经安装则会输出相应信息
>openssh-7.4p1-11.el7.x86_64
openssh-server-7.4p1-11.el7.x86_64
openssh-clients-7.4p1-11.el7.x86_64

若未安装测开始安装ssh
>yum install openssh-server -y

**-y表示安装过程中自动提示选择全部选择y*

安装完成后
修改ssh文件配置
>vim /etc/ssh/sshd_config

- 将`Port 22`前面的`#`删除 
- 将`PermitRootLogin prohibit-password`后面修改为`yes`
- 将`PubkeyAuthentication `后面修改为`yes`
- 将`ListenAddress 0.0.0.0`前面的`#`删除 
- 将`ListenAddress ::`前面的`#`删除 

修改后重启ssh服务
>systemctl restart sshd

查看sshd是否运行，有结果输出则显示运行正常
>ps -e | grep sshd 

如果无法连接登录，查看端口是否开放：
>firewall-cmd --zone=public --add-port=22/tcp --permanent 
service firewalld restart