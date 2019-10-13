1. 下载zabbix

    [zabbix下载地址](https://sourceforge.net/projects/zabbix/files/ZABBIX%20Latest%20Stable/4.2.5/zabbix-4.2.5.tar.gz/download)

2. 解压 

    `tar -zxvf zabbix-4.2.5.tar.gz`

2. 创建zabbix用户

    ```
    groupadd zabbix 
    
    useradd -g zabbix -d /usr/lib/zabbix -s /sbin/nologin -c "Zabbix Monitoring System" zabbix
    ```
3. 安装postgresql数据库并创建原始数据

    ```
    create user zabbix with password 'zabbix';
        
    create database zabbix owner zabbix encoding 'UTF8';
    
    cat schema.sql | psql -U zabbix
    cat images.sql | psql -u zabbix 
    cat data.sql | psql -U zabbix
    ```
4. 编译安装

    ```
    ./configure --prefix=/usr/local/zabbix/   --enable-agent --enable-server --with-postgresql --with-net-snmp --with-libcurl --with-libxml2 --with-unixodbc --with-libevent=/opt/libevent/
    
    
    make
    make install
    ```
5. 添加启动脚本

    ```
    cp misc/init.d/tru64/zabbix_server /etc/init.d/
    chmod +x /etc/init.d/zabbix_server
    
    ```
    
6. 启动zabbix服务（此处启动的是zabbix_server）

    `service zabbix_server start`
    
7. Web登录zabbix进行最后安装

    ![image](https://upload-images.jianshu.io/upload_images/1885581-7169023130ed0dfa?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    
8. 检查先决条件
    ![image](https://upload-images.jianshu.io/upload_images/1885581-c61d145ec5141e4b?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    
9. 配置数据库连接
    ![image](https://upload-images.jianshu.io/upload_images/1885581-0627cdc1cd48169a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    
10. Zabbix服务器详细信息
    ![image](https://upload-images.jianshu.io/upload_images/1885581-68e015328a0c6d26?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

11. 安装前摘要
![image](https://upload-images.jianshu.io/upload_images/1885581-1162a61239eab6bd?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
12. 安装
 ![image](https://upload-images.jianshu.io/upload_images/1885581-704f8f510974a875?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    
    **P.s**  cp /usr/local/nginx/html/conf/zabbix.conf.php.example /usr/local/nginx/conf/zabbix.conf.php
    
    把上面这个zabbix.conf.php文件放到zabbix站点下。此配置文件是数据链接配置信息。把此配置文件添加好后重新

13. 把`zabbix.conf.php`配置文件添加好后重新上面步骤就可以成功登陆zabbix了，爽！
 ![image](https://upload-images.jianshu.io/upload_images/1885581-3da3a1d9a9f6d58a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
14. 点击Finish即可登录zabbix管理页面默认用户是：Admin ， 密码是：zabbix 。后面可以在管理页面修改。
 ![zabbix 登陆页面](https://upload-images.jianshu.io/upload_images/1885581-c518827ccdd5194d?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
   

*参考文献：*
  [21运维](http://www.21yunwei.com/archives/2332)
  [官方源码文档](https://www.zabbix.com/documentation/4.2/manual/installation/install#installing_frontend)
  [官方yum安装文档](https://www.zabbix.com/documentation/4.2/manual/installation/install_from_packages/rhel_centos)
  [nagios和zabbix发展趋势](http://index.baidu.com/v2/main/index.html#/trend/nagios?words=nagios,zabbix)