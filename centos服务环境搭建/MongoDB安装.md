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
    