#### 简介

每次登陆*Linux*终端都会有一段小提示，如当前登陆时间等。现在想试下每次登陆提示如我们在*Windows*下，提示一段名言警句。而*fortune*小程序帮我们实现了这个功能，*cowsay*可以打印一个小动物。*fortune*和*cowsay*配合起来实现每次登陆*Linux*(注意：~~是Linux terminal~~)操作系统都会有个小动物说一句话。

#### 准备工作

* nmp 安装 （待会儿会使用npm安装cowsay）
    ```
    # 配置nodejs本地yum源
    curl -sL -o /etc/yum.repos.d/khara-nodejs.repo https://copr.fedoraproject.org/coprs/khara/nodejs/repo/epel-7/khara-nodejs-epel-7.repo

    yum install -y nodejs nodejs-npm
    ```
* git 安装（使用git工具clone fortune中文字库）
    ` yum install git`
* 下载 fortune-mod-1.99.1-17.sdl7.x86_64.rpm包
    [fortune下载地址](https://centos.pkgs.org/7/puias-x86_64/fortune-mod-1.99.1-17.sdl7.x86_64.rpm.html)

#### cowsay和fortune安装配置

1. fortune安装
    `yum install fortune-mod-1.99.1-17.sdl7.x86_64.rpm `

2. cowsay 安装
    `npm install -g cowsay`
3. fortune中文化
    ```
    git clone https://github.com/ruanyf/fortunes.git
    # /usr/share/games/fortune/ 这个路径为fortune字库路径，可以使用which fortune查询
    \cp -rf fortunes/data/* /usr/share/games/fortune/
    ```
4. cowsay 使用
    * 指定说话内容
   ```
    root@testplan:~ # cowsay welcome to juzldream.
    < welcome to juzldream. >
     -----------------------
            \   ^__^
             \  (oo)\_______
                (__)\       )\/\
                    ||----w |
                    ||     ||
    ```
    * 查看可选动物
    ```
    root@testplan:~ # cowsay -l
    beavis.zen  bong  bud-frogs  bunny  cheese  cower  daemon  default  doge  dragon-and-cow  dragon  elephant-in-snake  elephant  eyes  flaming-sheep  ghostbusters  goat  hedgehog  hellokitty  kiss  kitty  koala  kosh  luke-koala  mech-and-cow  meow  milk  moofasa  moose  mutilated  ren  satanic  sheep  skeleton  small  squirrel  stegosaurus  stimpy  supermilker  surgery  telebears  turkey  turtle  tux  vader-koala  vader  whale  www
    ```
    * 使用其他动物
    ```
    root@testplan:~ # cowsay -f sheep welcome to juzldream.
     _______________________
    < welcome to juzldream. >
     -----------------------
      \
       \
           __     
          UooU\.'@@@@@@`.
          \__/(@@@@@@@@@@)
               (@@@@@@@@)
               `YY~~~~YY'
                ||    ||

    ```
    * 随机显示动物
    ```
    root@testplan:~ # cowsay -r welcome to juzldream.
     _______________________
    < welcome to juzldream. >
     -----------------------
       \
        \
            .--.
           |o_o |
           |:_/ |
          //   \ \
         (|     | )
        /'\_   _/`\
        \___)=(___/
    ```
    * 更多命令请 man cowsay 查看
5. fortune 使用
    * fortune 命令
    ```
    root@testplan:~ # fortune 
        
    结庐在人境，而无车马喧，问君何能尔？心远地自偏
    
            -  晋·陶渊明
    ```
    * 指定显示某一个库话语
    ```
    root@testplan:~ # fortune -e tang300
    《怨情》
    作者：李白
    美人卷珠帘，深坐蹙蛾眉。
    但见泪痕湿，不知心恨谁。
    ```
6. 增加格言包
    
    `root@testplan:~ # vim personalquotaions`
    ![](https://mmbiz.qpic.cn/mmbiz_png/4iaE7bB4HCjfqYY5WojYELH6qobSibToibDUOzr0ZZVS4Kic53plhEjg4KP9f98AtIFlys54SZP3qMkWVibzZAdNC7g/0?wx_fmt=png)    

    ```
    root@testplan:~ # strfile personalquotaions personalquotaions.dat
    
    root@testplan:~ # cp personalquotaions personalquotaions.dat //usr/share/games/fortune/
    # 测试
    root@testplan:~ # fortune personalquotaions
    让读书成为一种生活方式。就像吃喝拉撒每天必须要干的事，终有一天你的举止、言谈、气质会不一样的嘛！
                                                —-蝉溪一梦
    ```
7. 设置开机自动打印提示语
    ```
    # 在/etc/profile.d目录下见一个sh文件，内容如下。这样没次登录终端都会有欢迎语，哈哈！
    root@testplan:~ # cat /etc/profile.d/cowsay.sh 
    #!/bin/bash
    # date:2018-09-06
    # author:racher
    
    echo
    echo "=============== Quote Of The Day ==============="
    echo
    fortune | cowsay -r
    echo
    echo "================================================"
    echo
    
    ```
