# Centos 环境搭建
> 前提: 阿里云ECS服务器  系统： Centos 7.8 64位

### 安装Nginx
##### 必要的库安装
1. gcc 安装
  安装 nginx 需要先将官网下载的源码进行编译，编译依赖 gcc 环境 
    ``` yum install gcc-c++  ```
2. PCRE pcre-devel 安装
  PCRE(Perl Compatible Regular Expressions) 是一个Perl库，包括 perl 兼容的正则表达式库。nginx 的 http 模块使用 pcre 来解析正则表达式，所以需要在 linux 上安装 pcre 库，pcre-devel 是使用 pcre 开发的一个二次开发库。
    ``` yum install -y pcre pcre-devel ```
3. zlib 安装
  zlib 库提供了很多种压缩和解压缩的方式， nginx 使用 zlib 对 http 包的内容进行 gzip 
    ``` yum install -y zlib zlib-devel ```
4. OpenSSL 安装
  OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及 SSL 协议，并提供丰富的应用程序供测试或其它目的使用。
  nginx 不仅支持 http 协议，还支持 https（即在ssl协议上传输http）
    ``` yum install -y openssl openssl-devel ```
5. wget 安装
  用来下载安装包
    ``` yum install wget ```

#### Nginx 编译安装
1. 下载nginx安装包  
    ```
      cd /usr/local                                           // 切换到压缩包下载存放的目录
      wget -c https://nginx.org/download/nginx-1.19.0.tar.gz  // 下载安装包
    ```
2. 解压安装包
    ```
      tar -zvxf nginx-1.19.0.tar.gz   // 解压nginx安装包
      cd nginx-1.19.0                 // 切换到解压目录
      ./congfigure                    // 使用默认配置
    ```
3. 编译安装
    ```
      make
      make install
    ```    
4. 查看安装路径
    ``` 
      whereis nginx 
    ```
5. 启动nginx
    ```
      cd /usr/local/nginx/sbin/   // 切换到nginx启动目录
      ./nginx                     // 启动命令
     ```    
6. 常用命令

    重启nginx
    ``` 
      ./nginx -s reload 
    ```

    停止nginx 
    ``` 
      ./nginx -s stop 
    ```

    检测配置文件是否正确  
    ``` 
      ./nginx -t 
    ```

    查询nginx进程
    ``` 
      ps aux |grep nginx 
    ```

### 安装NVM、NodeJS
##### 必要库安装
1. 版本管理工具 git
    > yum install git
##### 安装 NVM  
1. 下载安装nvm
    ```
      wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
    ```
2. 验证安装
    ```
      command -v nvm
    ```
3. 设置自动安装
    ```
      echo "source ~/nvm/nvm.sh" >> ~/.bashrc
      source ~/.bashrc 
    ```
4. 安装nodejs版本
    ```
      nvm install v10.16.0
    ```
5. 查看安装node版本
    ```
      nvm ls
    ```  
6. 切换node 版本
    ```
      nvm use v10.16.0
    ```


### 安装MongoDB

1. 下载mongoDB安装包
    ```
      cd /usr/local                           // 切换到压缩包下载存放的目录
      wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-4.0.0.tgzwget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-4.0.0.tgz     // 下载安装包
    ```
2. 解压安装包 ,重命名文件夹
    ```
      tar zxvf mongodb-linux-x86_64-4.0.0.tgz
      mv mongodb-linux-x86_64-4.0.0 mongodb
    ```
3. 创建数据库目录
    ```
      cd /usr/local/mongodb         // 切换到安装目录
      touch mongodb.conf      // 创建配置文件  mongodb.conf
      mkdir db                // 创建数据库目录
      mkdir log               // 创建日志存放目录
      cd log                  // 切换日志目录
      touch mongodb.log       // 创建日志文件
    ```
4. 编写 mongodb.conf 配置文件
    ```
      vim /usr/local/mongodb/mongodb.conf
    ```
    添加以下内容
    ```
      port=27017                              #端口
      dbpath= /usr/mongodb/db                 #数据库存文件存放目录
      logpath= /usr/mongodb/log/mongodb.log   #日志文件存放路径
      logappend=true                          #使用追加的方式写日志
      fork=true                               #以守护进程的方式运行，创建服务器进程
      maxConns=100                            #最大同时连接数
      noauth=true                             #不启用验证
      journal=true                            #每次写入会记录一条操作日志（通过journal可以重新构造出写入的数据）
                                              #即使宕机，启动时wiredtiger会先将数据恢复到最近一次的checkpoint点，然后重放后续的journal日志来恢复。
      storageEngine=wiredTiger                #存储引擎有mmapv1、wiretiger、mongorocks
      bind_ip = 0.0.0.0                       #这样就可外部访问了，例如从win10中去连虚拟机中的MongoDB
    ```
5. 配置环境变量
    ```
      vim /etc/profile  // 打开配置文件
    ```
    在 export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL 一行的上面添加如下内容:
    ```
      #Set Mongodb
      export PATH=/usr/mongodb/bin:$PATH
    ```
    保存后，使用命令使环境变量生效
    ```
      source /etc/profile
    ```
6. 启动mongodb
    ```
      cd ~ 
      mongod --config /usr/mongdb/mongpdb.conf
    ```
7. 关闭mongodb
    ```
      ps aux |grep mongodb   // 查询pid
      sudo kill pid
    ```
     <img src = 'https://persongitbook.oss-cn-beijing.aliyuncs.com/searchPid.jpg' />
    