Linux 常用命令总结

1. RPM（红帽软件包管理器）

|           |                     |
|----       |---                  |
|安装软件命令|rpm -ivh filename.rpm|
|升级软件命令|rpm -Uvh filename.rpm|
|卸载软件命令|rpm -e filename.rpm|



2. Yum 软件仓库

|           |                     |
|----       |---                  |
|安装软件包|yum install 软件包名称|
|移除软件包|yum remove 软件包名称|
|清除所有仓库缓存|yum clean all|

### 常用系统工作命令

3. echo 打印字符串

```
[root@linuxprobe ~]# echo welcome to juzldream.
welcome to juzldream.
```

```
[root@linuxprobe ~]# echo $PATH
/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:/sbin:/root/bin
```

4. date 显示及设计系统时间日历

```
[root@linuxprobe ~]# date "+%Y-%m-%d %H:%M:%S %j"
2018-01-03 13:30:42 003
```

5. reboot 重启系统

6. poweroff 关闭系统


7. wget 下载网络问卷


8. ps 查看系统中的进程状态


9. top 动态监控进程活动与系统负载

10. pidof 查询某个服务进程的PID值

11. kill 终止某个进程

12. killall 终止某个指定名称的服务所对应的全部进程


### 系统状态检查命令


1. ifconfig 获取网卡配置与网络状态等信息


2. uname 查看系统内核与系统版本等信息

3. uptime 查看系统负载信息

4. free 内存使用情况


5. who 登录主机的用户终端信息

6. last 查看系统的所有登录记录


7. history 历史执行过得命令


8. sosreport 收集系统配置及架构信息并输出诊断文件



### 工作目录切换命令

1. pwd 显示用户当前所处的工作目录


2. cd 切换工作路径

3. ls 显示目录中的文件信息


### 文本文件编辑命令


1. cat 查看纯文本文件


2. more 查看纯文本文件


3. head 查看纯文本文件的前N行



4. tail 查看纯文本文件后N行或持续刷新内容



5. tr 替换、压缩或删除文本文件中的字符
```
[root@linuxprobe ~]# echo "HELLOE WORLD" | tr [A-Z] [a-z]
helloe world
[root@linuxprobe ~]# echo "hello 123 world 456" | tr -d [0-9]
hello  world 

```

6. wc 统计文本的行数、字数、字节数


7. stat 查看文件的存储信息和时间

```
[root@linuxprobe ~]# stat anaconda-ks.cfg 
  File: ‘anaconda-ks.cfg’
  Size: 1542      	Blocks: 8          IO Block: 4096   regular file
Device: fd00h/64768d	Inode: 72061124    Links: 1
Access: (0600/-rw-------)  Uid: (    0/    root)   Gid: (    0/    root)
Context: system_u:object_r:admin_home_t:s0
Access: 2018-01-04 15:20:30.199114008 +0800
Modify: 2018-01-03 02:33:19.166958908 +0800
Change: 2018-01-03 02:33:19.166958908 +0800
 Birth: -
```

8. cut 按‘列’提前文本字符


9. diff 比较多个文本文件的差异


### 文件目录管理命令


1. touch 创建空文件或设置文件时间

2. mkdir 创建目录

3. cp 复制文件

4. mv 剪切或者重命名文件

5. rm 删除文件

6. dd 安装指定大小和个数的数据块来复制文件或转换文件
```
[root@linuxprobe ~]# dd if=/dev/zero of=dd_test_file bs=10M count=10
10+0 records in
10+0 records out
104857600 bytes (105 MB) copied, 1.83617 s, 57.1 MB/s
[root@linuxprobe ~]# ls -l dd_test_file 
-rw-r--r--. 1 root root 104857600 Jan  4 15:42 dd_test_file

```
7. file 查看文件类型
```
[root@linuxprobe ~]# file /dev/sda anaconda-ks.cfg /bin/passwd 
/dev/sda:        block special
anaconda-ks.cfg: ASCII text
/bin/passwd:     setuid ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=0a16a7915f7f9b01d96442755257e22067ce5b2c, stripped
```

### 打包压缩与搜索命令

1. tar 对文件进行打包压缩或解压

```
[root@linuxprobe ~]# tar -czvf etc.tar.gz /etc/

[root@linuxprobe ~]# ls etc.tar.gz -l
-rw-r--r--. 1 root root 9083904 Jan  4 15:30 etc.tar.gz

```
`[root@linuxprobe ~]# tar -zxvf etc.tar.gz -C /root/etc
`

2. grep 搜索关键字

```
[root@linuxprobe ~]# grep -n -v "/sbin/nologin" /etc/passwd
1:root:x:0:0:root:/root:/bin/bash
6:sync:x:5:0:sync:/sbin:/bin/sync
7:shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
8:halt:x:7:0:halt:/sbin:/sbin/halt
43:linuxprobe:x:1000:1000:linuxprobe:/home/linuxprobe:/bin/bash
```

3. find 按照指定条件查找文件

```
[root@linuxprobe ~]# find /etc/ -name "host*" 
/etc/selinux/targeted/modules/active/modules/hostname.pp
/etc/host.conf
/etc/hosts
/etc/hosts.allow
/etc/hosts.deny
/etc/avahi/hosts
/etc/hostname
```




