### Linux 系统下安装步骤

#### 一 、安装java环境

1. 下载jdk，下载地址：[jdk-9.0.4](http://www.oracle.com/technetwork/java/javase/downloads/jdk9-downloads-3848520.html)


2. 解压安装`jdk`
    ```
    tar -zxvf jdk-9.0.4_linux-x64_bin.tar.gz  -C /usr/local/
    cd /usr/local/
    ln -sf jdk-9.0.4 jdk
    
    ```
3. 配置环境变量
    ```
    vim /etc/profile.d/jdk.sh
    --------------------------------------------------->
    JAVA_HOME=/usr/local/jdk
    PATH=$JAVA_HOME/bin:$PATH
    export JAVA_HOME PATH
    <---------------------------------------------------
    . /etc/profile.d/jdk.sh        //重读此文件，让变量生效
    
    ```

4. 检查`jdk`是否安装成功

    `java -version`
#### 二 、安装Tomcat

1. 下载`Tomcat`,下载地址: [tomcat](https://tomcat.apache.org/download-90.cgi)
2. 解压安装`Tomcat`
    ```
    tar -zxvf apache-tomcat-9.0.5 -C /usr/local/
    cd /usr/local/
    ln -sf apache-tomcat-9.0.5 tomcat
    ```
3. 配置环境变量
    ```
    vim /etc/profile.d/tomcat.sh
    --------------------------------------------------->
    CATALINA_BASE=/usr/local/tomcat
    PATH=$CATALINA_BASE/bin:$PATH
    export PATH CATALINA_BASE
    <---------------------------------------------------
    . /etc/profile.d/tomcat.sh
    ```
4. 查看`Tomcat`版本

    `catalina.sh version`
5. 配置`Tomcat`

    `vim /usr/local/tomcat/conf/tomcat-users.xml 
    `
    ```
    <role rolename="manager-gui"/>
    <role rolename="manager-script"/>
    <role rolename="manager-jmx"/>
    <role rolename="manager-status"/>
    <role rolename="admin-gui"/>
    <role rolename="admin-script"/>
    <user username="admin" password="admin" roles="manager-gui,manager-script,manager-jmx,manager-status,admin-gui,admin-script"/>
    
    <user username="deploy" password="deploy" roles="manager-script"/>
    
    </tomcat-users>
    "tomcat-users.xml" 54L, 2610C      
    ```
    `vim /usr/local/tomcat/conf/Catalina/localhost/manager.xml 
    `
    ```
    <Context privileged="true" antiResourceLocking="false" docBase="${catalina.home}/webapps/manager">
            <Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="^.*$" />
    </Context>
    
    ```
6. 创建测试页面

    ```
    mkdir -pv /usr/local/tomcat/webapps/test/WEB-INF/{classes,lib}
    vim /usr/local/tomcat/webapps/test/index.jsp
    --------------------------------------------------------------->
    <%@ page language="java" %>
    <%@ page import="java.util.*" %>
    <html>
        <head>
            <title>test</title>
        </head>
        <body>
            <%
                out.println("Hello World!");      //嵌入java语言
            %>
        </body>
    </html>
    ```
7. 启动、停止、`tomcat`、以及查看日志

    ```
    /usr/local/tomcat/bin/{startup.sh,shutdown.sh}

    startup.sh 
    shutdown.sh 
    
    
    ps -ef | grep tomcat
    kill -9 ID
    
    catalina.out
    ```

8. 浏览器预览

    `http://192.168.19.74`

### Windows 系统下安装步骤