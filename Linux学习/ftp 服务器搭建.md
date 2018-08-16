* 本次实验环境为：CentOS 6.5
#### ftp 服务器简介
> ##### FTP 是File Transfer Protocol（文件传输协议）的英文简称，而中文简称为“文传协议”。用于Internet上的控制文件的双向传输。同时，它也是一个应用程序（Application）。基于不同的操作系统有不同的FTP应用程序，而所有这些应用程序都遵守同一种协议以传输文件。在FTP的使用当中，用户经常遇到两个概念："下载"（Download）和"上传"（Upload）。
##### 安装步骤

1. 检查vsftp 软件是否安装
```
[root@testplan ~]# rpm -qa | grep vsftp

```
2. 安装ftp软件
```
[root@testplan ~]# yum install vsftp

```
3. 启动服务

```
[root@localhost ~]# service vsftpd start
Starting vsftpd for vsftpd:                                [  OK  ]

```

4. 相关配置

    * 简单配置
    
    ```
    anonymous_enable=NO         //非匿名
    anon_upload_enable=YES      //上传
    anon_mkdir_write_enable=YES //创建
    
    [root@localhost ftp]# passwd ftp
    Changing password for user ftp.
                                //设置ftp用户密码
    ftp：//192.168.88.88                   //登录
    
    ```
    * 限制重要系统用户不能登录ftp
    
        ```
           [root@localhost ~]# cat /etc/vsftpd/ftpusers 
        # Users that are not allowed to login via ftp
        root
        bin
        daemon
        nobody
        ftp

        ```
    * 限制系统用户锁定在家目录
        
        ```
        chroot_list_enable=YES
        # (default follows)
        chroot_list_file=/etc/vsftpd/chroot_list

         cut -d : -f 1 /etc/passwd >> /etc/vsftpd/chroot_list
        ```
     
    * 利用ftp用户策略允许登录ftp的系统用户 
    
        ```
        userlist_enable=YES
        serlist_deny=NO
        userlist_file=/etc/vsftpd/user_list

        ```
5. 访问测试

    ftp://192.168.88.88


[参考文献](http://www.linuxidc.com/Linux/2015-06/118442.htm)

