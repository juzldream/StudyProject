1. 压缩各种包

    ```
    function ctar(){
        a=($@)
        if [ $# -ge 2 ]
        then
            case $1 in
               *.tar) tar -cvf all.tar ${a[@]:1:$#} ;;
               *.tar.gz) tar -czvf all.tar.gz ${a[@]:1:$#} ;;
               *.tar.bz2) tar -cjvf all.tar.bz2 ${a[@]:1:$#} ;;
               *.tar.Z) tar -cZvf all.tar.Z ${a[@]:1:$#} ;;
               *.zip) zip all.zip ${a[@]:1:$#} ;;
               *.rar) zip all.rar ${a[@]:1:$#} ;;  #需要安装rar工具
               *) echo "'$1' The compression format is incorrect.";;
            esac
        else
            echo "Input error,Please re-enter."
            echo -e "\033[34m Usage: tar [-p format {.tar .tar.gz .tar.bz2 .tar.Z .zip .rar ] [-f filename].\n eg:[ctar .zip file1 file2 ...] \033[0m"         
        fi
    }
    ```
    
2. 解压各种格式包文件
    ```
    function ltar(){
        if [ -f $1 ]; then
        case $1 in
            *.tar.bz2) tar xjvf $1;;
            *.tar.gz) tar zxvf  $1;;
            *.bz2) bunzip2 $1 ;;
            *.rar) unrar e $1 ;;
            *.gz) gunzip $1 ;;
            *.tar) tar xvf $1 ;;
            *.tbz2) tar xjf $1 ;;
            *.tgz) tar xzf $1 ;;
            *.zip) unzip $1 ;;
            *.Z) uncompress $1 ;; #把.Z包解压为.rar包
            # tar -xZvf  $1 ;;    #把.Z包一步到位解压文件
            *.7z)7z x $1 ;;
            *.tar.xz) tar -xvJf $1 ;;
            *) echo "'$1' cannot be extracted";;
        esac
        else
            echo "'$1' is not a valid file"
        fi
    }
    ```
3. 在`/etc/profile.d/`下`touch`一个`.sh`文件，将上面两个脚本*copy*
4. 重新登录系统即可使用，例子：
    - 压缩文件：ctar .zip file1 file2 file2
    - 解压文件：ltar file.tar.gz

5. 查看包文件

    `tar -tvf 包名`
