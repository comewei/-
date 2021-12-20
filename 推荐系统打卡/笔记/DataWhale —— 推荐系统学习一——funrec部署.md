# DataWhale —— 推荐系统学习

### 环境说明





### 1.Ubuntu下安装MySQL

#### 1.1 安装mysql

```shell
# 安装mysql server 和 mysql-client
sudo apt install mysql-server mysql-client

# 查看安装的版本信息
mysql -V

#验证MySQL服务是否正在运行
sudo service mysql status
```

如果在运行的话：

```
● mysql.service - MySQL Community Server
     Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2021-12-13 12:47:29 CST; 2min 38s ago
   Main PID: 81488 (mysqld)
     Status: "Server is operational"
      Tasks: 37 (limit: 4482)
     Memory: 352.2M
     CGroup: /system.slice/mysql.service
             └─81488 /usr/sbin/mysqld

Dec 13 12:47:28 iZwz90pegu9budyf79q3ieZ systemd[1]: Starting MySQL Community Server...
Dec 13 12:47:29 iZwz90pegu9budyf79q3ieZ systemd[1]: Started MySQL Community Server.
```

#### 1.2配置MySQL的安全性

1. 首先，运行命令`mysql_secure_installation`：

   ```bash
   sudo mysql_secure_installationCopy to clipboardErrorCopied
   ```

2. `VALIDATE PASSWORD COMPONENT`

   设置验证密码插件。它被用来测试`MySQL`用户的密码强度，并且提高安全性。如果想设置验证密码插件，请输入`y`：

   ```bash
   Connecting to MySQL using a blank password.
   
   VALIDATE PASSWORD COMPONENT can be used to test passwords
   and improve security. It checks the strength of password
   and allows the users to set only those passwords which are
   secure enough. Would you like to setup VALIDATE PASSWORD component?
   
   Press y|Y for Yes, any other key for No: yCopy to clipboardErrorCopied
   ```

   接下来，将进行密码验证等级设置，根据数字设置对应等级，这里设置为0：

   ```bash
   There are three levels of password validation policy:
   
   LOW    Length >= 8
   MEDIUM Length >= 8, numeric, mixed case, and special characters
   STRONG Length >= 8, numeric, mixed case, special characters and dictionary file
   
   Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 0Copy to clipboardErrorCopied
   ```

3. **设置密码**

   **为MySQL root用户设置密码，设置过程中密码不会显示。如果设置了验证密码插件，将会显示密码的强度。**

   ```
   Please set the password for root here.
   New password: 
   
   Re-enter new password: 
   
   Estimated strength of the password: 25 
   Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : yCopy to clipboardErrorCopied
   ```

4. 移除匿名用户

   默认情况下，MySQL安装有一个匿名用户，允许任何人登录MySQL，而不必为他们创建用户帐户。输入`y`进行删除：

   ```
   By default, a MySQL installation has an anonymous user,
   allowing anyone to log into MySQL without having to have
   a user account created for them. This is intended only for
   testing, and to make the installation go a bit smoother.
   You should remove them before moving into a production
   environment.
   
   Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
   Success.Copy to clipboardErrorCopied
   ```

5. 禁止远程root用户登录

   输入`y`后按`enter`，将会禁止`root`用户登录。

   ```
   Normally, root should only be allowed to connect from
   'localhost'. This ensures that someone cannot guess at
   the root password from the network.
   
   Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
   Success.Copy to clipboardErrorCopied
   ```

6. 删除测试库

   输入`y`后按`enter`，将会删除测试库。

   ```
   By default, MySQL comes with a database named 'test' that
   anyone can access. This is also intended only for testing,
   and should be removed before moving into a production
   environment.
   
   
   Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
    - Dropping test database...
   Success.Copy to clipboardErrorCopied
   ```

7. 重新加载特权表

   输入`y`后按`enter`，将会重新加载特权表。

   ```
    - Removing privileges on test database...
   Success.
   
   Reloading the privilege tables will ensure that all changes
   made so far will take effect immediately.
   
   Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
   Success.
   
   All done! 
   ```

#### 1.3 以root用户登录

在MySQL 8.0上，root 用户默认通过`auth_socket`插件授权。`auth_socket`插件通过 Unix socket 文件来验证所有连接到`localhost`的用户。

这意味着你不能通过提供密码，验证为 root。此时，输入`mysql -uroot -p`可能会被拒绝访问：

```bash
lyons@ubuntu:~$ mysql -uroot -p
mysql: [Warning] Using a password on the command line interface can be insecure.
ERROR 1698 (28000): Access denied for user 'root'@'localhost'Copy to clipboardErrorCopied
```

若要以 root 用户身份登录 MySQL服务器，输入`sudo mysql`，如下：

```bash
# 登录密码为linux系统用户的root密码
admin@ubuntu:~$ sudo mysql
[sudo] admin 的密码： 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 55
Server version: 8.0.27-0ubuntu0.20.04.1 (Ubuntu)

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> Copy to clipboardErrorCopied
```

退出MySQL，请输入`exit`命令：

```mysql
mysql> exit
Bye
lyons@ubuntu:~$ Copy to clipboardErrorCopied
```

如果你想以 root 身份登录 MySQL 服务器，便于使用其他的程序。可以将验证方法从`auth_socket`修改成`mysql_native_password`。

- **方式2**（用这个！！）

  推荐的选项，就是创建一个新的独立管理用户，拥有所有数据库的访问权限：

```mysql
# 创建用户
CREATE USER '用户名'@'localhost' identified by '你的密码'

# 赋予admin用户全部的权限，你也可以只授予部分权限
GRANT ALL PRIVILEGES ON *.* TO '用户名'@'localhost';Copy to clipboardErrorCopied
```

 示例：

```mysql
# 创建名为admin的用户，密码为mysql123
mysql> create user 'admin'@'localhost' identified by 'mysql123';
Query OK, 0 rows affected (0.01 sec)

# 将访问所有database以及表的权利授权用户admin
#with gran option表示该用户可给其它用户赋予权限，但不可能超过该用户已有的权限
mysql> grant all privileges on *.* to 'admin'@'localhost' with grant option;
Query OK, 0 rows affected (0.00 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)

# 查看已有的用户
mysql> select user, host from mysql.user;
+------------------+-----------+
| user             | host      |
+------------------+-----------+
| admin            | localhost |
| debian-sys-maint | localhost |
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| root             | localhost |
+------------------+-----------+
6 rows in set (0.00 sec)

# 退出root用户登录
mysql> exit
Bye

# 登录admin用户，输入密码mysql123即可登录成功
lyons@ubuntu:~$ mysql -uadmin -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 16
Server version: 8.0.27-0ubuntu0.20.04.1 (Ubuntu)

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> Copy to clipboardErrorCopied
```

说明：`'admin'@'localhost'`中，`localhost`指本地才可连接，可以将其换成`%`指任意`ip`都能连接，也可以指定`ip`连接。

#### 1.4 使用root可以远程登录

mysql8真的是太恶心了！！因为网上很多教程都是教mysql5的

首先你要去改/etc/mysql/my.cnf下mysql的配置文件

然后你会发现是空的

```
#
# The MySQL database server configuration file.
#
# You can copy this to one of:
# - "/etc/mysql/my.cnf" to set global options,
# - "~/.my.cnf" to set user-specific options.
# 
# One can use all long options that the program supports.
# Run program with --help to get a list of available options and with
# --print-defaults to see which it would actually understand and use.
#
# For explanations see
# http://dev.mysql.com/doc/mysql/en/server-system-variables.html

#
# * IMPORTANT: Additional settings that can override those from this file!
#   The files must end with '.cnf', otherwise they'll be ignored.
#

!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mysql.conf.d/Copy to clipboardErrorCopied
```

但其实不然

其实文件不是空的。它有两个相当重要的指令，即

```
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mysql.conf.d/Copy to clipboardErrorCopied
```

这些行表示可以在列出的目录中找到**其他**配置文件（在这种情况下为.cnf）：

```
/etc/mysql/conf.d/
/etc/mysql/mysql.conf.d/Copy to clipboardErrorCopied
```

这两个目录中的后一个目录应包含**mysqld.cnf**。换句话说，适当的配置文件应为：

```
/etc/mysql/mysql.conf.d/mysqld.cnfCopy to clipboardErrorCopied
```

也就是原来mysql5里面mysqld的配置

```
xxxx
[mysqld]
xxxxxxxx
xxxxx
xxCopy to clipboardErrorCopied
```

这些内容放到了/etc/mysql/mysql.conf.d/mysqld.cnf中

然后修改这个文件里面的bind_address = 127.0.0.1修改为bind_address = 0.0.0.0

然后

```bash
mysql -uroot -p

use mysql
alter user 'root'@'localhost' identified with mysql_native_password by '你的密码';
select host,user from user;

#发现root的host是localhost，只允许本地访问
update user set host='%' where user='root';
flush privileges;
```

### 

- 安装 Redis  Mongo  PyMongo

- 前端安装

  - 解压 `/fun-rec/codes/news_recsys/news_rec_web`  `node-v8.17.0-linux-x64.tar`压缩文件

    - ```shell
      tar -xvf node-v8.17.0-linux-x64.tar
      ```

  - 换cnpm

    - ```
      npm install -g cnpm --registry=https://registry.npm.taobao.org cnpm install
      
      npm install --production --unsafe-perm=true --allow-root
      ```

    - ```
      npm cache clean --force
      ```

    - 删除 rm -rf package-lock.json  https://blog.csdn.net/u013075460/article/details/117331326

    - ```npm config set sass_binary_site=https://npm.taobao.org/mirrors/node-sass```  pm ERR! code ELIFECYCLE npm ERR! errno 1 npm ERR! node-sass@4.14.1 postinstall: `node scripts/build

      https://blog.csdn.net/lilyheart1/article/details/108254112

  - 想要把阿里云端口打开
  
  - 查看端口是否开放
  
    ```shell
    netstat -nlp | grep 5000
    ```
  
    



### 2.安装 anaconda

1. 本人习惯把软件下载放到`/home/software`中

   ```
   cd /home/software
   ```

2. 下载 https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2021.05-Linux-x86_64.s 的Anaconda包：

   ```Vscode
   wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2021.05-Linux-x86_64.sh --no-check-certificate
   ```

3. 下载完成之后，进行安装：

   ```
   bash Anaconda3-2021.05-Linux-x86_64.sh
   ```

4. 安装之后就是配置的过程，可以无脑yes，我选择默认的位置/root/anaconda3(**这个大家不要选择/root/anaconda3**)，因为我这是一个人使用的.

5. 安装成功之后会出现以下界面：

   ![image-20211105112329905](E:\Typora笔记\程序员杂记\云服务器搭建深度学习环境.assets\image-20211105112329905.png)

6. 配置Anaconda 的环境变量

   ```
   conda config --set show_channel_urls yes
   ```

   注：如果不知道condarc在哪里的同学，可以使用Linux的`find`命令查找

   ```
   find / -name '.condarc'
   ```

   编辑 `condarc` 文件 修改为：

   ```bash
   channels:
     - defaults
   show_channel_urls: true
   default_channels:
     - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
     - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
     - http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
   custom_channels:
     conda-forge: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     msys2: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     bioconda: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     menpo: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     pytorch: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     simpleitk: http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
   ```

   source .condarc

   ```
   i
   
   export PATH="/root/anaconda3/bin:$PATH"
   
   esc
   
   :wq
   
   source ~/.bashrc
   ```

6. 查看一下安装的python版本，会进入到anaconda中的python

   ```
   python --version
   ```

7. anaconda 速度较慢，切换一下清华的anaconda库（有时候清华源也会出错，后面会介绍如果因为Conda HTTP出错如何解决）：

   ```
   conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
   conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
   
   conda config --show  #查看conda的配置
   ```

### 3. mongodb安装

#### 3.1安装mongodb

```bash
# 安装依赖包
sudo apt-get install libcurl4 openssl
# 关闭和卸载原有的mongodb
service mongodb stop
sudo apt-get remove mongodb
```

- [下载mongodb网址](https://www.mongodb.com/try/download/community)

  ![image-20211215111653502](E:\研究生\工作\推荐系统\DataWhale —— 推荐系统学习.assets\image-20211215111653502.png)

- ```
  cd /home/software
  
  # 下载
  wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-ubuntu2004-4.4.10.tgz
  # 解压到/usr/local这个目录
  tar -zxvf mongodb-linux-x86_64-ubuntu2004-4.4.10.tgz --directory /usr/local
  ```

- 修改`mongodb-linux-x86_64-ubuntu2004-4.4.10.tgz` 为 `mongodb`

  ```bash
  export PATH="/usr/local/mongodb/bin:$PATH"
  ```

- ```bash
  source ~/.bashrc
  ```

#### 3.2创建数据库目录

默认情况下 MongoDB 启动后会初始化以下两个目录：

- 数据存储目录：/var/lib/mongodb
- 日志文件目录：/var/log/mongodb

我们在启动前可以先创建这两个目录：

```bash
sudo mkdir -p /var/lib/mongo
sudo mkdir -p /var/log/mongodb
```

接下来启动 Mongodb 服务：

```bash
mongod --dbpath /var/lib/mongo --logpath /var/log/mongodb/mongod.log --fork
mongo
```

#### 3.3 外部连接

如果需要外部连接mongo，加上bind_ip 0.0.0.0来启动

```
mongod --dbpath /var/lib/mongo --logpath /var/log/mongodb/mongod.log --bind_ip 0.0.0.0 --forkCopy to clipboardErrorCopied
```

关闭mongodb服务

```
mongo --shutdown /var/lib/mongo

#or
ps -ef | grep mongo
kill 进程
```

#### 3.4 表操作

>  mongo

- 查看已有数据库

  ```
  show dbs;
  ```

- 查看特定数据库数据表

  ```
  use SinaNews
  ```

  ```
  db.表名.drop()
  ```





### 4. redis 安装

#### 4.1安装Redis服务器

```sudo apt-get install redis-server```

 **启动Redis服务：**

  一般来说，当安装完成后，Redis服务器会自动启动，可以通过以下命令检查是否启动成功。（ps：如果Active显示为 active(running) 状态：表示redis已在运行，启动成功）

  ```shell
  service redis-server status
  ```

- 发现以下bug:

  - ![图片image-20211030164432589](E:\研究生\工作\推荐系统\DataWhale —— 推荐系统学习.assets\图片image-20211030164432589.png)

  - :question:redis-server.service: Can't open PID file /run/redis/redis-server.pid (yet?) after start: Operation not permitted

```shell
# 找到redis.service所在目录
find / -name 'redis.service'
# 编辑文件
vim /etc/systemd/system/redis.service
```

- ​	`ExecStartPost=/bin/sh -c "echo $MAINPID > /run/redis/redis.pid"`

![image-20211218113807065](E:\研究生\工作\推荐系统\DataWhale —— 推荐系统学习.assets\image-20211218113807065.png)

- **重启服务**

```shell
sudo systemctl daemon-reload
sudo systemctl enable redis-server
sudo systemctl restart redis.service
```

bug解决



- 检查当前进程，查看redis是否启动。（ps: 可以看到redis服务正在监听6379端口）

  ```shell
  ps -aux | grep redis-server
  ```

  ![image-20211215112254592](E:\研究生\工作\推荐系统\DataWhale —— 推荐系统学习.assets\image-20211215112254592.png)

- 或者进入redis客户端，与服务器进行通信，当输入ping命令，如果返回 PONG 表示Redis已成功安装

  ```shell
  redis-cli
  ```

  ```bash
  (base) root@iZwz90pegu9budyf79q3ieZ:~# redis-cli
  127.0.0.1:6379> ping
  PONG
  127.0.0.1:6379> 

#### 4.2使用远程连接Redis

[参考](https://blog.csdn.net/qq_19260029/article/details/77920423)

首先修改配置文件

在`/etc/redis/redis.conf`

- 远程连接

  - 打开vim /etc/redis/redis.conf

    ```bind  0.0.0.0 ```

关闭redis

```bash
ps -ef | grep redis
redis-cli shutdown
/etc/init.d/redis-server stop
```

打开服务

/etc/redis# /etc/init.d/redis-server start

redis 以配置信息启动登录设置密码

redis-server /etc/redis/redis.conf

- redis 这一块

  ![image-20211217085308412](E:\研究生\工作\推荐系统\DataWhale —— 推荐系统学习.assets\image-20211217085308412.png)





### 5. node 安装

- Github项目中存放了`fun-rec/codes/news_recsys/news_rec_web/node-v8.17.0-linux-x64.tar**`

  - 解压压缩包

  - 安装之后存放在`/opt`
  
  - ```shell
    #环境变量配置
    vim ~/.bashrc
    -----
    👇最后一行（i）
    export PATH="/opt/node-v8.17.0-linux-x64/bin:$PATH"
    -----esc :wq
    
    #check success
    node -v
    npm -v
    ```

#### 源码导入

看了下`readme`

源码是放在`\home`下面新建了`\fun-rec`下的，参照这个上传

注意

`fun-rec-master\codes\news_recsys\news_rec_web\Vue-newsinfo\src\api`

里面有个乱码的api文档，我把它删了才能顺利ftp穿上阿里云

### 6. 环境配置

- 前端环境配置

  - 进入 `/home/LaiXiangWei/fun-rec/codes/news_recsys/news_rec_web/Vue-newsinfo`中

    ```shell
    npm install -g cnpm --registry=https://registry.npm.taobao.org
    cnpm install
    ```

  - 然后删掉modules

    - ```
      npm install rimraf -g 
      rimraf node_modules
      ```

  - 然后root用户的话

    ```bash
    npm install --production --unsafe-perm=true --allow-root
    ```

    非root用户

    ```bash
    sudo npm install
    ```

- 前端配置文件修改

  - ```\home\fun-rec\codes\news_recsys\news_rec_web\Vue-newsinfo\src\main.js``` 修改为：

    ```js
    // Vue.prototype.$http = axios
    Vue.use(VueAxios, axios);
    // axios公共基路径，以后所有的请求都会在前面加上这个路径
    // axios.defaults.baseURL = "http://10.170.4.60:3000";
    //axios.defaults.baseURL = "http://47.108.56.188:3000";
    axios.defaults.baseURL = "http://107.47.207:5000"
    ```

  - `\home\fun-rec\codes\news_recsys\news_rec_web\Vue-newsinfo\package.json`

    ```json
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "dev": "webpack-dev-server --open --port 8686 --contentBase src --hot --host 0.0.0.0",
        "start": "nodemon src/main.js"
      },
    ```

- python环境的安装

  在安装完Anaconda之后，创建一个新闻推荐的虚拟环境，我这边将其命名为news_rec_py3，**这个环境将会在整个新闻推荐项目中使用。**

  ```C++
  conda create -n news_rec_py3 python==3.8
  ```

- ##### Scrapy的简介与安装

  Ubuntu下安装Scrapy，需要先安装依赖Linux依赖

  ```bash
  sudo apt-get install python3 python3-dev python3-pip libxml2-dev libxslt1-dev zlib1g-dev libffi-dev libssl-dev
  ```

- ###### python3依赖安装

  激活虚拟环境，安装依赖

  ```
  conda activate news_rec_py3
  
  #切换目录
  cd /home/fun-rec/codes/news_rec_server/
  pip install -r requirements.txt
  ```

  - 安装pysnowflake

  ```
  pip install pysnowflake
  ```

- 后端配置文件修改
  - \home\fun-rec\codes\news_recsys\news_rec_server\conf
  
    dao_config.py
  
    ```python
    # 修改mysql账号密码
    mysql_username = "xxxxx"
    mysql_passwd = "xxxxxxxxx"
    mysql_hostname = "localhost"
    mysql_port = "3306"
    ```
  
    proj_path.py
  
    ```python
    #修改home_path
    #home_path = os.environ['home']
    home_path = "/home/"
    proj_path = home_path + "/fun-rec/codes/news_recsys/news_rec_server/"
    ```
  
    \home\fun-rec\codes\news_recsys\news_rec_server\dao
  
    mysql_server.py
  
    ```python
    # 修改mysql账号密码
    class MysqlServer(object):
        def __init__(self, username="xxxxx", passwd="xxxxxxx", hostname="localhost", port="3306",
            user_info_db_name=user_info_db_name, loginfo_db_name=loginfo_db_name):
    
            self.username = username
            self.passwd = passwd
            self.hostname = hostname
            self.port = port
            self.user_info_db_name = user_info_db_name
            self.loginfo_db_name = loginfo_db_name
    ```
  
    \home\fun-rec\codes\news_recsys\news_rec_server
  
    后端还需要在news_rec_server下新建一个`logs`文件夹
  
    ```bash
    mkdir logs
    ```

### 7. 前后端环境启动

顺序是后端环境、pysnowflake、前端环境

开三个terminal

如果没开news_rec_py3的虚拟环境，就是命令行前面没有(news_rec_py3)，前两个terminal要先输

```bash
conda activate news_rec_py3
```

- 后端环境启动

  - 进入mysql里面

    ```
    mysql -uroot -p
    create database userinfo;
    create database loginfo;
    ```

    \home\fun-rec\codes\news_recsys\news_rec_server

    ```bash
    python server.py
    ```

  - snowflake 服务启动

    ```
    snowflake_start_server --address=127.0.0.1 --port=8910 --dc=1 --worker=1
    ```

  - 前后端环境启动

    `/fun-rec/codes/news_recsys/news_rec_web/Vue-newsinfo`

    ```shell
    # 运行以下命令
    npm run dev
    ```

    这次遇到过报错

    sh: 1: webpack-dev-server: not found

    cnpm install一下之后再npm run dev就好了

- 打开阿里云防火墙

  - ![image-20211216150250334](E:\研究生\工作\推荐系统\DataWhale —— 推荐系统学习.assets\image-20211216150250334.png)

- 访问

  接下来访问http://服务器公网ip:8686应该就能到首页了

### 8. Scrapy 爬虫设置

- 手动爬取新浪新闻

  `/fun-rec/codes/news_recsys/news_rec_server/materials/news_scrapy/sinanews`

  setting.py需要修改

  ```python
  from typing import Collection
  import sys
  # 以下两行需要修改
  # from conf.proj_path import proj_path
  sys.path.append("/home/LaiXiangWei/fun-rec/codes/news_recsys/news_rec_server")
  from conf.dao_config import mongo_hostname, mongo_port, sina_db_name, sina_collection_name_prefix
  ```

  terminal，激活环境

  ```bash
  #暂时先爬30页
  python run.py --page=30
  ```

- 更新物料画像

  - `/home/LaiXiangWei/fun-rec/codes/news_recsys/news_rec_server/materials`

    ```shell
    python process_material.py
    ```

- 更新用户画像

  - `/home/LaiXiangWei/fun-rec/codes/news_recsys/news_rec_server/materials`

    ```shell
    python process_user.py
    ```

  - 遇到bug

    ```
    pymysql.err.ProgrammingError: (1146, "Table 'userinfo.register_user' doesn't exist")
    ```

    - 进入mysql

    - ```mysql
      show databases; -- 查看已有的数据库
      use userinfo -- 使用userinfo数据库
      -- 创建register_user数据库
      ```

    - ![image-20211216232204041](E:\研究生\工作\推荐系统\DataWhale —— 推荐系统学习.assets\image-20211216232204041.png)
  
    - 注册表之后出现![image-20211216233932204](E:\研究生\工作\推荐系统\DataWhale —— 推荐系统学习.assets\image-20211216233932204.png)
  
    - 原因，前端main.js中没有映射到位
  
      ![image-20211216235912709](E:\研究生\工作\推荐系统\DataWhale —— 推荐系统学习.assets\image-20211216235912709.png)
  
- 清除前一天的`redis`中数据,更新今天最新的数据

  `/home/LaiXiangWei/fun-rec/codes/news_recsys/news_rec_server/materials`

  ```shell
  python update_redis.py
  ```

- 离线将推荐列表和热门列表初入redis

  `/home/LaiXiangWei/fun-rec/codes/news_recsys/news_rec_server/recprocess`

  ```shell
  python offline.py
  ```

- 登录用户查看新闻

  发现没有数据![image-20211217085331310](E:\研究生\工作\推荐系统\DataWhale —— 推荐系统学习.assets\image-20211217085331310.png)
  

- bug

  - ![image-20211216145105955](E:\研究生\工作\推荐系统\DataWhale —— 推荐系统学习.assets\image-20211216145105955.png)

  端口被占用，使用以下命令查看端口

  ```shell
   (base) root@iZwz90pegu9budyf79q3ieZ:~# netstat -nlp | grep 8686
  tcp        0      0 0.0.0.0:8686            0.0.0.0:*               LISTEN      109606/python       
  (base) root@iZwz90pegu9budyf79q3ieZ:~# kill -9 109606
  ```


> 参考

[C同学DataWhale配置](https://seanlocked.github.io/#/news_rec_sys/task0/)





> 感悟

配置了Mongo Redis还有爬虫的代码整整一两天，十分感谢锋哥、程哥、还有队伍小伙伴的帮忙。发现配置环境本身就是一个比较复杂，需要交流的过程。对于自己部署过程的细节要有所记录，做好笔记，这样才不容易忘记。笔记也要做的一模一样，不然白学。linux放置配置文件的习惯要有所改进，对文件要及时存储和备份。readme很重要，这个也是部署的关键，之前做的笔记有点样子了，可以刚好的复习

部署环境和服务器两者是区分开来的





注意：登录时不能连接外网，外网连接不能使用





部署项目环境过程中，发现这个项目前端后端交互方面自己很是缺乏，对于计算机网络学习知识忘记了一大半。爬虫问题使用自己的电脑爬取网络有些问题，导致卡了很久。一个很多的收获就是要做好笔记，这样才可以备查。之前安装anaconda的知识一下就用上来了，感觉真的很不错。很感谢锋哥的分享还有几个小时帮我debug，队友们也很热情。不知不觉两三天卡在部署就很久了。

这次告诉 对于Linux文件放置要有条例，做好笔记，善于分享，多多交流。
