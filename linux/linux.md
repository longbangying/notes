# linux 一些常用到的命令
- 查看指定端口是否被占用
`netstat  -anp|grep 端口号                     	`
- 查看服务器当前端口号的占用情况
` netstat  -nultp `
- centos设置yum的源为阿里镜像中心
	wget -O /etc/yum.repos.d/CenOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo  
	或
	curl -O /etc/yum.repos.d/CenOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
	清除旧缓存  yum clean all
	创建新缓存  yum makecache
	安装必要的软件包 yum -y install vim tree nmap sysstat gcc gcc-c++ make telnet
- linux 查看已经安装的软件(rpm 安装的)
`rpm -qa `
- linux 查看指定软件是否安装(rpm)
`rpm -qa|grep wget`
- linux 查看已经安装的软件(yum)
`yum list installed`
- linux 查看指定软件是否安装(yum)
`yum list installed |grep weget`
- 利用 scp 在两服务器间传文件(此种没设置sshd免密登录的话，要输入远程服务器的密码)
`scp 本地文件全路径  远程服务器IP(或者服务器名)：远程服务器路径`
####防火墙
- `systemctl status firewalld.service `   查看防火墙状态
- `systemctl stop firewalld.service` 关闭防火墙
- `systemctl disable firewalld.service` 防止防火墙开机启动

#### selinux
- `sestatus `  查看selinux 状态
- `vi /etc/selinux/config  将SELINUX=ENFORCING 改为 SELINUX=disabled  禁用selinux`