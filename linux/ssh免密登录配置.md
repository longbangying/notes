# linux ssh免登录配置
### 检查ssh服务状态
	systemctl status sshd
### 配置hostname(非必须)
	hostname                               查看当前主机名
	hostname 新的主机名                     设置新的主机名(这种方式，重启之后就失效了)
	hostnamectl set-hostname 新的主机名     设置新的主机名(永久有效)  
### 配置hosts
	vi /etc/hosts                          加入IP和域名映射   如: 127.0.0.1    xbang

### 生成密钥对
	ssh-keygen -t rsa -P ''                生成密钥对文件，一路回车即可，成功之后会在/root/.ssh/目录下生成两个文件id_rsa和id_rsa.pub,在/root/.ssh/目录下新建文件authorized_keys,并将id_rsa.pub 中的内容复制到里面(要实现免密登录的两台机子上的 authorized_keys 里的内容应该是一样)
	配置完后 ssh  hostname 就不用输入密码了，首次ssh会有个确认；