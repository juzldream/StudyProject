[【apache下载地址】](http://httpd.apache.org/download.cgi#apache24) [【安装参考】](https://www.cnblogs.com/wcwnina/p/8029156.html)

1. 系统编译工具安装

    ```
    yum -y groupinstall "Development tools" 
    
    yum -y groupinstall "Development Libraries" 
    ```

2. 解决依赖包：arp、apr-util、pcre 

    [包介绍官网](http://apr.apache.org/projects.html)

1. apr 安装

    ```
    tar -zxvf apr-1.7.0.tar.gz
    cd apr-1.7.0
    ./configure --prefix=/usr/local/apr
    make
    make install
    ```

2. apr-util 安装

    ```
    tar -zxvf apr-util-1.6.1.tar.gz
    cd apr-util-1.6.1
    ./configure --help | less
    ./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr
    make
    make install
    ```

3. 安装pcre

    ```
    unzip pcre-8.43.zip
    cd pcre-8.43
     ./configure --prefix=/usr/local/pcre --with-apr=/usr/local/apr/bin/apr-1-config
    make
    make install
    ```

3. 安装httpd

    ```
    tar -jxvf httpd-2.4.41.tar.bz2
    cd httpd-2.4.41
    ./configure --prefix=/usr/local/apache --sysconfdir=/etc/httpd --enable-so --enable-rewrite --enable-ssl --enable-cgi --enable-cgid --enable-modules=most --enable-mods-shared=most --enable-mods-shared=all --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr-util --with-pcre=/usr/local/pcre --with-mpm=event
    make
    make install
    ```
4. 关闭selinux

    ```
    setenforce 0
    getenforce
    vim /etc/selinux/config
    7 SELINUX=disable
    ```

8. apache 启动

    ```
    apachectl start  #启动
    apachectl -L
    apachectl -M #查看模块
    ```

9. httpd 启动报错

    ```
    [root@DNSweb bin]# ./apachectl stop
    AH00557: httpd: apr_sockaddr_info_get() failed for DNSweb
    
    解决办法：文件添加
    /etc/hosts   
    127.0.0.1    HOSTNAME
    
    
    AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 127.0.0.1. Set the 'ServerName' directive globally to suppress this message
    
    
    httpd.conf 文件取消注释
    215 ServerName localhost:80
    ```