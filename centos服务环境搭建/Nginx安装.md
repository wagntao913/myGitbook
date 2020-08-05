### 安装Nginx
##### 必要的库安装
1. gcc 安装
  安装 nginx 需要先将官网下载的源码进行编译，编译依赖 gcc 环境   
    >``` yum install gcc-c++  ```
2. PCRE pcre-devel 安装
  PCRE(Perl Compatible Regular Expressions) 是一个Perl库，包括 perl 兼容的正则表达式库。nginx 的 http 模块使用 pcre 来解析正则表达式，所以需要在 linux 上安装 pcre 库，pcre-devel 是使用 pcre 开发的一个二次开发库。
    >``` yum install -y pcre pcre-devel ```
3. zlib 安装
  zlib 库提供了很多种压缩和解压缩的方式， nginx 使用 zlib 对 http 包的内容进行 gzip 
    >``` yum install -y zlib zlib-devel ```
4. OpenSSL 安装
  OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及 SSL 协议，并提供丰富的应用程序供测试或其它目的使用。
  nginx 不仅支持 http 协议，还支持 https（即在ssl协议上传输http）
    >``` yum install -y openssl openssl-devel ```
5. wget 安装
  用来下载安装包
    >``` yum install wget ```

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
      ps -ef |grep nginx
    ```

