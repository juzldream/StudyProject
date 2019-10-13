cobbler 批量安装系统

### ansible 简单配置之inventory(清单)

- 参考：

	https://docs.ansible.com/ansible/2.4/intro_inventory.html

1. 免秘钥登录系统

	```
	ssh-keygen
	ssh-copy-id -i id_rsa.pub xzdnsaipso@111.11.252.132
	iptables -I INPUT -s 111.11.208.2  -p tcp --dport 22 -j ACCEPT
	```

- 配置文件

	`vim /etc/ansible/hosts`

### ansible modules (模块)

1. 参考：

	https://docs.ansible.com/ansible/latest/modules/modules_by_category.html

2. commad  （命令）
	```
	/usr/local/bin/ansible DNS-cache -m command -a "df -h"
	/usr/local/bin/ansible DNS-cache -m command -a "free -g"
	/usr/local/bin/ansible all -m command -a "ifconfig eth0"
	```
3. shell  （管道）
	```
	/usr/local/bin/ansible DNS -m shell -a "ifconfig |grep eth*"
	/usr/local/bin/ansible DNS -m shell -a "ifconfig eth0 | awk 'NR==2'"    
	/usr/local/bin/ansible DNS -m shell -a "ifconfig eth0 | awk 'NR==2';df -h;free -g;w"
	```
4. yum 安装软件，仅限root用户
	```
	/usr/local/bin/ansible other -m yum -a "name=httpd state=installed"
	/usr/local/bin/ansible other -m shell -a "rpm -qa|grep httpd"
	/usr/local/bin/ansible other -m yum -a "name=httpd state=removed"
	```
5. copy (copy文件 向文件写东西)
	```
	ansible-doc copy  #查看copy模块用法
	/usr/local/bin/ansible other -m copy -a "src=/etc/hosts dest=/tmp/test"
	/usr/local/bin/ansible other -m command -a "ls /tmp/test -l"
	/usr/local/bin/ansible other -m command -a "cat /tmp/test"
	/usr/local/bin/ansible other -m copy -a "src=/etc/hosts dest=/tmp/test.txt owner=alipms group=alipms mode=0600"
	/usr/local/bin/ansible other -m copy -a "src=/etc/hosts dest=/tmp/test backup=yes"  
	/usr/local/bin/ansible other -m copy -a "content='ansible 123 com' dest=/tmp/test backup=yes" 
	```
6. service 启动服务
	```
	/usr/local/bin/ansible other -m service -a "name=httpd state=started|stopped|restarted|reloaded enabled=yes" 
	enabled  是否让服务器开启自动启动
	/usr/local/bin/ansible other -m copy -a "content='This is ansible test.' dest=/var/www/html/index.html"
	/usr/local/bin/ansible other -m service -a "name=httpd state=restarted"
	```
7. script (直接执行shell脚本，但是少用)
	`/usr/local/bin/ansible other -m script -a "script.sh"`

8. file 创建文件和目录
	```
	/usr/local/bin/ansible other -m file -a "path=/tmp/file_test state=directory"
	/usr/local/bin/ansible other -m file -a "path=/tmp/file_test/test.txt state=touch"
	path 
	recurse 递归
	state
		directory 在远端创建目录
		touch     在远端创建文件
		link      创建软连接文件
		hard      创建硬连接文件
		mode      设置文件或目录券商
		owner
		group
	```
9. group（root用户）
	`/usr/local/bin/ansible other -m group -a "name=gps gid=888"`
10. user（root用户）
	```
	/usr/local/bin/ansible other -m user -a "name=test uid=999 group=888 shell=/sbin/nologin create_home=no"
	/usr/local/bin/ansible other -m user -a "name=test uid=999 group=888 shell=/sbin/nologin state=absen"   删除用户
	uid
	group
	groups       指定附加组
	password     给用户添加密码
	shell        指定用户登录的shell
	create_home  是否创建家目录
	-----------------------------------------------------------------------
	echo 123.com | openssl passwd -salt 'ok.' -stdin ; echo 123.com | openssl passwd -salt 'no.' -stdin
	$1$123456$ubeM.omJvWFVdZGIzK6BV.
	/usr/local/bin/ansible other -m user -a "name=xlm password='$1$NWCVGKb/$uozMg09Bsz2FNGjW72HGA.'"  创建xlm用户并设定密码为：123.com
	```
11. cron
	```
	ansible-doc cron 查看帮助
	/usr/local/bin/ansible other -m cron -a "minute=* hour=* day=* month=* weekday=* job='/bin/sh test.sh'"
	/usr/local/bin/ansible other -m cron -a "job='/bin/sh test.sh'"
	/usr/local/bin/ansible other -m cron -a "name='ansible add crontd.' job='/bin/sh test.sh'"
	/usr/local/bin/ansible other -m cron -a "name='ansible add crontd.' job='/bin/sh test.sh' state=absent" 
	/usr/local/bin/ansible other -m cron -a "name='ansible add crontd.' job='/bin/sh test.sh' disabled=yes" 注释定时任务
	```
12. mount （挂载目录设备仅限root用户）
	```
	/usr/local/bin/ansible other -m mount -a "path=/backup src=111.11.252.86:/data fstype=nfs opts=defaults state=present"  只写配置文件 /etc/fstab，并不会挂载
	/usr/local/bin/ansible other -m mount -a "path=/backup src=111.11.252.86:/data fstype=nfs opts=defaults state=mounted"
	state
		absent     卸载设备并清理写入到/etc/fstab配置
		mounted    挂载设备并将配置写入/etc/fstab配置
		present    仅将挂载配置写入到/etc/fstab配置，并不会挂载（鸡肋，不用）
		unmounted  临时卸载，不会清理/etc/fstab配置
	```


13. 检查主机是否ping
	```
	/usr/local/bin/ansible all -m ping
	/usr/local/bin/ansible DNS-cache -m ping
	/usr/local/bin/ansible 111.11.252.86 -m ping
	```