1. git 工具设置



Option
	Looks
		colours Cursor
		Block
	Text
		设置文字大小	小四	
	Windos 窗口大小


git config --global user.name "racher"


git config --global user.email "1576768715@qq.com"


git config --list




2.命令
	新建代码创库

    # 在当前目录新建一个Git创库
	git init

	# 下载一个项目
	git clone [url]

	#添加文件到暂存区
	git add [file1] [file2]

	#删除工作区文件，并且将这次删除放入暂存区
	git rm [file1] [file2]

	#改名文件，并且将这个改名放入暂存区
	git mv [file-origin] [file-renamed]

	代码提交

	#提交暂存区仓库
	git commit -m [message]

	#直接从工作区提交到仓库
	git commit -a -m [message]

	查看信息

	#显示变更信息
	git status

	#显示当前分支的历史版本
	git log
	git log --oneline

	gitl show [7fdcbd7b3c5e8437af10399a9b832753662a3863]


	同步远程仓库

	#增加远程仓库，并命名
	git remote add [shortname] [url]

	#将本地的提交推送到远程仓库
	git push [remote] [branch]


	#将远程仓库提交拉下到本地
	git pull [remote] [branch]



3. git to github
	git remote add origin https://github.com/juzldream/demo.git
 	git remote -v
	git push -u origin master
	git push origin master
    
    git pull origin master

    git clone http://github.com/juzldream/hello-git

4.git 练习
  https://try.github.io/levels/1/challenges/1

  Git-it
  	安装nodeJS
  	npm install git-it-g
  	

5. 完整流程

	git add file
	git commit -m ""
	git push

	