
#### 二进制安装

1. MySQL下载地址

	http://ftp.ntu.edu.tw/MySQL/Downloads/MySQL-5.7/

	https://dev.mysql.com/downloads/mysql/5.7.html#downloads

2. 创建mysql管理用户

	`useradde -r mysql -s /sbin/nologin -M`

3. 解压

	```
	tar -zxvf mysql-5.7.27-linux-glibc2.12-x86_64.tar.gz -C /usr/local/
	cd /usr/local/
	ls
	ln -sv mysql-5.7.27-linux-glibc2.12-x86_64 mysql
	cd mysql 	
	```

4. 更改mysql目录属主属组

	`chown -R mysql:mysql /usr/local/mysql/*`

5. 执行安装脚本安装mysql

    ```
    ./mysql_install_db --help

	mysql_install_db --no-defaults --user=mysql --basedir=/usr/local/mysql --datadir=/mysqldata/data/ --pid-file=/mysqldata/data/mysql.pid --tmpdir=/tmp
	
	```

6. 安全

	```
	chown -R root /usr/local/mysql/* 
	chkconfig --add mysqld
	```

7. 启动脚本

	`cp support-files/mysql.server /etc/init.d/mysqld`

8. 修改/etc/my.cnf

	```
	[mysql]
	default-character-set=utf8

	[mysqld]
	basedir = /usr/local/mysql
	datadir = /mysqldata/data
	socket = /tmp/mysql.sock
	log-error = /mysqldata/data/error.log
	pid-file = /mysqldata/data/mysql.pid
	user = mysql
	tmpdir = /tmp
	bindir = /usr/local/mysql/bin

	```

参考：

	https://www.cnblogs.com/murenhui/p/9036028.html


#### 源码安装

### rpm包安装

	`yum install mysql-server`




