### 安装NVM、NodeJS
##### 必要库安装
1. 版本管理工具 git
    ``` yum install git ```

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
