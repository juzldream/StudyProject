1. 编译安装

	```
	./configure --prefix=/usr/local/src/php --with-config-file-path=/usr/local/src/php/ --with-gd --with-png-dir=/usr/local/libpng --with-jpeg-dir=/usr/local/jpeg --with-freetype-dir=/usr/local/freetype --with-xpm-dir=/usr/ --with-vpx-dir=/usr/local/libvpx/ --with-zlib-dir=/usr/local/zlib --with-iconv --enable-libxml --enable-xml --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline-optimization --enable-opcache --enable-mbregex --enable-fpm --enable-mbstring --enable-ftp --enable-gd-native-ttf --with-openssl --enable-pcntl --enable-sockets --with-xmlrpc --enable-zip --enable-soap --without-pear --with-gettext --enable-session --with-mcrypt --with-curl --enable-ctype  --with-pdo-pgsql  --with-pgsql=/opt/PostgreSQL --with-ldap
	make 
	make install	
	```
2. php.ini 文件修改参数如下：

	```
	372 max_execution_time = 300
	393 memory_limit = 128M
	660 post_max_size = 16M
	792 upload_max_filesize = 2M
	382 max_input_time = 300
	910 date.timezone = PRC
	```
3. 启动php服务

	`service php-fpm status`

4. php-fpm 参数学习

	[PHP-fpm参数学习](https://www.cnblogs.com/doseoer/p/10319330.html)