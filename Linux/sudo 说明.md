
- sudo 配置文件 `/etc/sudoer`

- man sudoers   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#查看帮助

- 一个sudo条目构成

    `who   which_hosts=(runas)    command`

    ```
    who : User_Alias
    which_hosts : Host_Alias
    runas : Runas_Alias
    command : Cmd_Alias
    ```
- 别名定义

    * 用户别名  
    
        ```
        User_Alias USEERADMIN = {
        		           用户的用户名
        		           组名，使用%引导
        		           还可以包含其他用户别名
        }
        ```

    * 主机别名
    
        ```
        Host_Alias ： {
        	 	         主机名
        		         IP	
        			网络地址
        			其他主机别名
        }
        ```
    * runas别名
    
        ```
        Runas_Alias  ： {
        	用户名
        	%组名
        	其他的Runas别名
        }
        ```
        
    - 命令别名
        ```
        Cmnd_Alias : {
        	命令路径
        	目录（此目录里的所有命令）
        	其他事先定义的命令别名
        
        }
        ```
    
       
- 问题：jhadmin  ， root  ，useradd ， usermod

    ```
    
    bg：jhadmin ALL=(root) /usr/sbin/useradd,/usr/sbin/usermod
           jhadmin ALL=(root) NOPASSWD:/usr/sbin/useradd,PASSWD:/usr/sbin/usermod
    
           User_Alias USERADMIN = jhadmin ，%hadoop，%useeradmin
           Cmnd_Alias USERADMINCMND = /usr/sbin/useradd,/usr/sbin/usermod,/usr/sbin/userdel/usr/bin/passwd [A-Za-z]* ,!/usr/bin/passwd root
           
           USERADMIN    ALL=（root）NOPASSWD:USERADMINCMND 
    ```

- sudo 命令
    ```
    sudo -k #每次都需要输入认真信息让认证信息失效
    sudo -l  #列出当前用户可以执行的sudo命令
    ```

- 日志信息
`/var/log/secure`


**?能修改eth0网络属性**

**？怎么定义其它用户据用root一样的权限**
