#### Samba 之 AD 域控环境搭建

1. 准备工作：
  *  Linux 服务器（redhat 6.8） 用于安装 samba AD
  IP：192.168.88.10
  *  Windows 客户端 用于加域测试
  *  Linux 端需要配置YUM源

2.下载samba软件包，本次采用源码安装方式

  * 下载地址：https://www.samba.org/samba/download/
 
3.安装依赖库文件
* yum install -y perl gcc libacl-devel libblkid-devel gnutls-devel readline-devel python-devel gdb pkgconfig krb5-workstation zlib-devel setroubleshoot-server libaio-devel setroubleshoot-plugins policycoreutils-python libsemanage-python setools-libs-python setools-libs popt-devel libpcap-devel sqlite-devel libidn-devel libxml2-devel libsepol-devel libattr-devel keyutils-libs-devel cyrus-sasl-devel cups-devel bind-utils libxslt docbook-style-xsl openldap-devel


4.开始编译安装samba
* ./configure --prefix=/usr/local/samba  --enable-selftest

* make 
  
* make test
  
* make install
4.环境变量

* vim /root/.bash_profile 

	PATH=$PATH:$HOME/bin:/usr/local/samba/bin:/usr/local/samba/sbin

	export PATH
	
5.配置Samba AD

* samba-tool domain provision --server-role=dc --use-rfc2307 --dns-backend=SAMBA_INTERNAL --realm=xadev.com --domain=xadev --adminpass=JHadmin999 

* 
    samba-tool domain passwordsettings set --complexity=off

    samba-tool domain passwordsettings set --history-length=0
    
    samba-tool domain passwordsettings set --min-pwd-age=0
    
    samba-tool domain passwordsettings set --max-pwd-age=0 
        
6. 配置DNS

* /etc/resolv.conf
    
    domain xadev.com

    nameserver 10.99.0.1

7. 配置kerberos

* /etc/krb5.conf
    
    rm /etc/krb5.conf
    ln -sf /usr/local/samba/private/krb5.conf /etc/krb5.conf


8. 启动Samba AD

samba

6.把客户端加入AD环境中

 * 安装 samba 、krb5
 
```
yum install samba

yum install krb5-devel krb5-libs pam_krb5 krb5-workstation
```
 * 运行setup 加域
 
    ![image](https://mmbiz.qlogo.cn/mmbiz_jpg/4iaE7bB4HCjdXwqpgfoaBVGJVcU2aicOjGvGF0ZqvNGn1GrJwoTvS9mqPCCQn2fgSsrqKUQcTuIMgzXM3rgyyWicg/0?wx_fmt=jpeg)

    ![image](https://mmbiz.qlogo.cn/mmbiz_jpg/4iaE7bB4HCjdXwqpgfoaBVGJVcU2aicOjGgRX6jWxp66RoQTIwlYkjqiaeRTh6GL8CrBogSINTRWU7UIcJW2oR3Cg/0?wx_fmt=jpeg)
    
    ![image](https://mmbiz.qlogo.cn/mmbiz_jpg/4iaE7bB4HCjdXwqpgfoaBVGJVcU2aicOjGs4OBbSWNvx6hz9u9kATTUIW2scSE3mFicZkx4Zu8TEFEaZSria0icrZCg/0?wx_fmt=jpeg)


 * 脚步配置加域
    * /etc/nsswitch.conf
    ```
    passwd:     files winbind
    shadow:     files winbind
    group:      files winbind

    ```
    * /etc/krb5.conf
    ```
    [logging]
     default = FILE:/var/log/krb5libs.log
     kdc = FILE:/var/log/krb5kdc.log
     admin_server = FILE:/var/log/kadmind.log
    
    [libdefaults]
     default_realm = XADEV.COM
     dns_lookup_realm = true
     dns_lookup_kdc = true
     ticket_lifetime = 24h
     renew_lifetime = 7d
     forwardable = true
    
    [realms]
     EXAMPLE.COM = {
      kdc = kerberos.example.com
      admin_server = kerberos.example.com
     }
    
    
    
     XADEV.COM = {
      kdc = jhserver.xadev.com
      default_domain = xadev.com
      admin_server = jhserver.xadev.com____
      kdc = jhserver.xadev.com
     }
    
    [domain_realm]
     xadev.com = XADEV.COM
     .xadev.com = XADEV.COM

    ```
    * /etc/samba/smb.conf
    ```
    [global]
       workgroup = XADEV
       password server = jhserver.xadev.com
       realm = XADEV.COM
       security = ads
       idmap config * : range = 16777216-33554431
       winbind separator = /
       template homedir = /home/%U
       template shell = /bin/bash
       winbind use default domain = true
       winbind offline logon = true
    
    [homes]
            comment = Home Directories
            browseable = no
            writable = yes
    ;       valid users = %S
    ;       valid users = MYDOMAIN\%S
            path = /home/%U
            valid users = XADEV.COM/%U
            read only = No
            browseable = No
            root preexec = /root/mkhome.sh %U %G
            create mode = 0777
            directory mode = 0777
    [printers]
            comment = All Printers
            path = /var/spool/samba
            browseable = no
            guest ok = no
            writable = no
            printable = yes
    ```
    * 加域
    ```
    /usr/bin/net join -w XADEV -S jhserver.xadev.com -U Administrator
    ```
    * 自动创建用户目录 /etc/pam.d/sysconf-auth
    
    ```
    session     required      pam_mkhomedir.so silent skel=/etc/skel umask=0077
    ```
[WiKi Samba](https://wiki.samba.org/index.php/Setting_up_Samba_as_an_Active_Directory_Domain_Controller)

