1. 解决问题：Unable to use libpcre (libpcre check failed)

    `yum -y install pcre*`

2. 创建zabbix用户

    ```
    groupadd zabbix
    useradd -g zabbix -d /usr/lib/zabbix -s /sbin/nologin -c "Zabbix_agentd System" zabbix
    ```

3. 编译安装

    ```
    ./configure --prefix=/usr/local/zabbix-agent --with-net-snmp --enable-agent
    make
    make install
    ```

4. 添加启动脚本

    ```
    cp misc/init.d/tru64/zabbix_agentd /etc/init.d/
    chmod +x /etc/init.d/zabbix_agentd
    ```

5. 修改启动脚本zabbix_agentd，因为我们安装的zabbix_agen位置在/usr/local/zabbix-agent下

    ```
    vim /etc/init.d/zabbix_agentd 
    24 DAEMON=/usr/local/zabbix-agent/sbin/zabbix_agentd
    ```

6. agent配置文件修改zabbix_agentd.conf

    ```
    Server=127.0.0.1,111.11.208.2  #111.11.208.2是zabbix_server端地址
    ServerActive=111.11.208.2
    Hostname=XZSN-PS-DNS-WEB       #zabbix_zgentd端主机名称，配置主机是和这个名称保存一致
    ListenPort=10050
    ```

7. 启动agentd

    `/etc/init.d/zabbix_agentd start`

8. 检查agentd服务是否启动成功

    ```
    ps -ef|grep zabbix_agen
    netstat -nltup|grep 10050      #zabbix_agentd服务器启动默认使用10050端口，因此也可以这样检查   
    ```