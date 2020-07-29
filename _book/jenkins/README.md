# jenkins -- 前端jenkins部署

### 1.环境搭建
##### 1-1 安装JDK
  - 下载[JDK安装包](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html),下载完成后，一路next即可，也可以自己设置安装路径
  - 配置环境变量
    右键单击“此电脑->属性->高级系统设置->环境变量”  
    <!-- <img src = 'https://ftp.bmp.ovh/imgs/2019/09/81615b1c9ba5f01b.png' />   -->
    在“系统变量”栏下单击“新建”。
    -  新建->变量名：JAVA_HOME 变量值：`C:\Program Files\Java\jdk1.8.0_181` (即JDK的安装路径)
    - 在“系统变量”栏里找到Path,选中后单击编辑,然后单击新建，填入 `%JAVA_HOME%\bin`
    - 新建->变量名：CLASSPATH 变量值  `.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar`  

##### 1-2 安装tomcat
  - 下载[tomact](http://tomcat.apache.org/),进入Tomcat9下载页面,对应着操作系统位数进行下载，下载后会是一个zip压缩包
  - 解压压缩包，放在想要放在的盘符(D盘、E盘...无所谓，记着路径就可以)
  - 配置环境变量
    -  新建->变量名：CATALINA_HOME 变量值：`C:\Program Files\Java\jdk1.8.0_181` (即tomact的解压路径)
    - 在“系统变量”栏里找到Path,选中后单击编辑,然后单击新建，填入 `%CATALINA_HOME%\bin;`  

##### 1-3 下载安装jenkins
  - 下载[jenkins](https://jenkins.io/zh/download/),下载下来的为.war文件
  - 将下载文件拖到tomcat目录下的`...\apache-tomcat-9.0.24\webapps`

### 2.Jenkins安装
  - 运行tomcat，在tomact目录下的 `...\apache-tomcat-9.0.24\bin` 目录下的 startup.bat,双击运行即可
    记住生成的密码，用于Administrator password
  - 打开浏览器`http://localhost:8080/jenkins/`，属于安装时生成的密码，点击`Continue`
    <img src = 'https://persongitbook.oss-cn-beijing.aliyuncs.com/jenkins-getting-started.png' />
  - 进入jenkins插件安装选择页面，有安装推荐的插件和自定义安装插件两个选项，我们选择安装推荐的插件`Install suggested plugins`，之后进入插件安装界面
    <img src = 'https://persongitbook.oss-cn-beijing.aliyuncs.com/jenkins-install-plugin.png' />
  - 等待安装完成之后，如果有些插件没有安装成功，先不用管，点击Continue进入创建用户界面
    <img src = 'https://persongitbook.oss-cn-beijing.aliyuncs.com/jenkins-create-user.png' />
  - 创建完成之后，进入配置Jenkins地址界面，直接点击继续就可以，当所有都配置完成之后，我们就进入到了Jenkins的主页,jenkins安装完成
    <img src='https://persongitbook.oss-cn-beijing.aliyuncs.com/jenkins-welcome.png'>
### 3. jenkins插件安装
  点击Jenkins首页 系统管理 》插件管理 》可选插件 .可以自由搜索相关的插件，进行安装
  <img src = 'https://upload-images.jianshu.io/upload_images/12796932-af318306f3b5a2f8.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp' />

### 4. jenkins 创建自动化项目部署
- 点击首页的New 任务或者create new jobs,进入创建页面，输入项目名字，选择第一个构建一个自由风格的软件项目，输入项目名称，点击确定
<img src = 'https://upload-images.jianshu.io/upload_images/4092152-819e964cabea90f9.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp' />
- 进入项目配置页面
<img src = 'https://user-gold-cdn.xitu.io/2018/9/11/165c77818ed60bb9?imageView2/0/w/1280/h/960/format/webp/ignore-error/1' />
  - General配置
  - 源码管理
    选择代码管理工具 Git/SVN,输入项目目录地址，账号/密码
    <img src = 'https://upload-images.jianshu.io/upload_images/4092152-845c7b4fdccd257a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp' />
  - 构建环境
    选中`Provide Node & npm bin/ folder to PATH`之后，你会发现NodeJS Installation并没有可选项，点击 系统设置 》全局工具配置 找到NodeJS,选择和你开发环境一样的NodeJS版本，然后点击保存
    <img src="https://persongitbook.oss-cn-beijing.aliyuncs.com/jenkins-node-enviornment.png">  
    回到配置页面，刷新一下，就能正确的显示
    <img src="https://persongitbook.oss-cn-beijing.aliyuncs.com/jenkins-environment.png">
  - 构建
    选择 执行`shell 在Command`窗口中填写你平常打包使用的命令，因为打包完成后要上传到服务器，所以添加了压缩文件的命令
    项目打包命令
    ```
    cd app
    npm config set registry https://registry.npm.taobao.org &&
    #npm install @vue/cli &&
    npm i
    npm run build:test
    ```
    项目代码放到服务器代码
    ```
    # for build check, uncomment this line
    #exit 0
    REMOTE_ACCOUNT=root@10.29.192.48

    # package into a tar
    cd $WORKSPACE/app/test
    tar -czf test.tar.gz *

    # remove remote files
    ssh $REMOTE_ACCOUNT rm -rf /usr/share/nginx/html/yiliaoh5/*

    # copy tar file to remote dir
    scp $WORKSPACE/app/test/test.tar.gz $REMOTE_ACCOUNT:/usr/share/nginx/html/yiliaoh5

    # unpack remote files
    ssh $REMOTE_ACCOUNT tar -zxvf /usr/share/nginx/html/yiliaoh5/test.tar.gz -C /usr/share/nginx/html/yiliaoh5
    ```
- 相关基础配置完成后，点击保存即可，返回jenkins首页，找到刚才构建的项目，点后面的图标按钮，开始构建项目
<img src = 'https://ftp.bmp.ovh/imgs/2019/09/4109fe42a8029b4a.png' />
