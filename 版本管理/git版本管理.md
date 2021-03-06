
### git
##### 环境配置
- 本地下载安装 [git](https://git-scm.com/downloads)  
- 注册 [github](https://github.com/join?source=header-home) 账号  

##### 创建 SSH KEY
-  检查C盘用户目录下有没有.ssh目录，有的话看下里面有没有id_rsa和id_rsa.pub这两个文件，没有就通过命令创建  
>` $ ssh-keygen -t rsa -C "youremail@example.com"`   

  然后一路回车。这时你就会在用户下的.ssh目录里找到id_rsa和id_rsa.pub这两个文件   
-  登录Github,找到右上角的图标，打开点进里面的Settings，再选中里面的SSH and GPG KEYS，点击右上角的New SSH key，然后Title里面随便填，再把刚才id_rsa.pub里面的内容复制到Title下面的Key内容框里面，最后点击Add SSH key，这样就完成了SSH Key的加密。

##### 本地项目上传至github
- 创建本地项目目录，执行 `git init`
- 然后执行 `git add .` 将项目目录下所有文件添加到本地git仓库，可以使用 `git status`查看当前状态
- 把项目提交到本地仓库  
   `git commit -m "提交描述信息"`
- 将在Github上Git仓库之后我们就可以和本地仓库进行关联了  
   `git remote add origin https://github.com/guyibang/TEST2.git(github仓库地址)`
- 关联好之后我们就可以把本地库的所有内容推送到远程仓库（也就是Github）上了  
   `git push -u origin master`  
  由于新建的远程仓库是空的，所以要加上-u这个参数
##### 常用命令
 <img src="https://persongitbook.oss-cn-beijing.aliyuncs.com/git.jpg">