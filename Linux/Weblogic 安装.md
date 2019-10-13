### 一、`jdk` 环境

1. 下载 `jdk-7u79-linux-x64.rpm ` , [jdk下载地址](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
2. 创建安装 jdk 目录
	`mkdir -p /usr/java`
3. 安装`jdk`
	`rpm -ivh jdk-7u79-linux-x64.rpm`

4. 配置环境变量

	```
	vi /etc/profile.d/jdk.sh
	#最文本最后添加：
	export JAVA_HOME=/usr/java/jdk1.7.0_79
	export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
	export PATH=$PATH:$JAVA_HOME/bin
	#并存并退出后，使配置生效
	source /etc/profile.d/jdk.sh
	#查看是否安装成功：
	java -version
	```

### 二、`weblogic` 安装

1. weblogic 下载，weblogic 下载地址: [weblogic 下载地址](http://www.oracle.com/technetwork/middleware/weblogic/downloads/index.html)

2. 建Weblogic用户和组
	```
	groupadd weblogic #创建weblogic组
	useradd -g weblogic weblogic #创建weblogic用户
	password weblogic #创建weblogic密码
	```
3. 运行weblogice jar包，我的jar放到了/usr/weblogic
    
    ```
    java -jar fmw_12.1.3.0.0_wls.jar
    ```

4. 图像界面
5. 启动Weblogic服务
    ```
    ./startWebLogic.sh
    #启动过程中要输入weblogic的用户名和密码
    ```
6. 启动完成，成功访问

    `http://IP+Port（7001）/console`
