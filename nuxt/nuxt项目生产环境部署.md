**Nuxt项目部署在Linux环境(CentOS7)**
==========

## **安装NodeJS**<br/>
### （1） wget命令下载Node.js安装包。
``` bash
$ wget https://nodejs.org/dist/v6.9.5/node-v10.11.0-linux-x64.tar.xz
```
### （2） 解压安装包。
``` bash
$ tar xvf node-v10.11.0-linux-x64.tar.xz
```
### （3）查看node.js和npm版本：
``` bash
$ node -v
$ npm -v
```

## **安装MongoDB**<br/>
### （1） wget命令下载MongoDB安装包。
``` bash
$ cd /usr/local
$ wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.6.9.tgz
```
### （2） 解压安装包，并重命名文件夹为mongodb。
``` bash
$ tar zxvf mongodb-linux-x86_64-3.6.9.tgz
$ mv mongodb-linux-x86_64-3.6.9.tgz
```
### （3） 创建数据和日志存放目录。
``` bash
$ mkdir /var/mongodb
$ mkdir /var/mongodb/data
$ mkdir /var/mongodb/logs
```
### （4） 设置开机启动项。
``` bash
$ vim /etc/rc.d/rc.local
```
打开文件后输入‘i’启用编辑。将mongodb启动命令追加到本文件中，让mongodb开机自启动：
``` bash
/opt/mongodb/bin/mongod --dbpath=/var/mongodb/data --logpath /var/mongodb/logs/log.log -fork
```
### （5） 手动启动MongoDB。
``` bash
$ /opt/mongodb/bin/mongod --dbpath=/var/mongodb/data --logpath /var/mongodb/logs/log.log -fork
```

## **安装Redis**<br/>
### （1） wget命令下载Redis安装包。
``` bash
$ cd /usr/local
$ wget http://download.redis.io/releases/redis-5.0.2.tar.gz
```
### （2） 解压安装包，并重命名文件夹为mongodb。
``` bash
$ tar tar xzvf redis-5.0.2.tar.gz
```
### （3） 安装Redis。
``` bash
$ cd redis-5.0.2
$ make
$ cd src
$ make install PREFIX=/usr/local/redis
```
### （4） 移动配置文件到安装目录下。
``` bash
$ cd ../
$ mkdir /usr/local/redis/etc
$ mv redis.conf /usr/local/redis/etc
```
### （5） 配置Redis为后台启动。
``` bash
#将daemonize no 改成daemonize yes
$ vim /usr/local/redis/etc/redis.conf
```
### （6） 将redis加入到开机启动。
``` bash
$ vim vi /etc/rc.local

#将里面添加内容:
/usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf
```
### （7） 启动Redis。
``` bash
$ /usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf 
```
## **安装Nginx**<br/>
### （1） 安装Nginx依赖环境。
``` bash
$ #gcc 安装
$ yum install gcc-c++

$ #PCRE pcre-devel 安装
$ yum install -y pcre pcre-devel

$ #zlib 安装
$ yum install -y zlib zlib-devel

$ #OpenSSL 安装
$ yum install -y openssl openssl-devel
```
### （2） wget命令下载Nginx安装包。
``` bash
$ wget -c https://nginx.org/download/nginx-1.10.1.tar.gz
```
### （3） 解压安装包。
``` bash
$ tar -zxvf nginx-1.10.1.tar.gz
$ cd nginx-1.10.1
```
### （4） 配置Nginx(默认配置)。
``` bash
$ ./configure
```
### （4） 编译安装Nginx。
``` bash
$ make
$ make install
```
### （5） 配置Nginx开机启动。
``` bash
$ vim /etc/rc.local
$ #添加如下一行
$ /usr/local/nginx/sbin/nginx
```
### （6） Nginx服务起停。
``` bash
$ cd /usr/local/nginx/sbin/
$ #Nginx 启动
$ ./nginx 
$ #Nginx 停止
$ ./nginx -s stop
$ #Nginx 退出
$ ./nginx -s quit
$ #Nginx 重启
$ ./nginx -s reload
```
### （7） Nginx配置文件nginx.conf。
```
server {
    listen       80;#Nginx监听端口
    server_name  localhost;

    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    location / {
        root   html;
        index  index.html index.htm;
        proxy_connect_timeout 15;
        proxy_send_timeout 15;
        proxy_read_timeout 15;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Connection "";
        proxy_pass http://localhost:3000; #（重定向接口）
        client_max_body_size 1024m;
    }
}
```
## **安装pm2**<br/>
### （1）  通过npm全局安装：
``` bash
$ npm install pm2 -g
```
### （2）  设置环境变量：
``` bash
$ vim /etc/profile

#在export添加如下一行
$ PATH=$PATH:/opt/node/lib/node_modules/pm2/bin

#保存后执行：
$ source /etc/profile
```
###  一些pm2命令：
``` bash
$ pm2 list # 查看当前正在运行的进程
$ pm2 start all  # 启动所有应用
$ pm2 restart all  # 重启所有应用
$ pm2 stop all # 停止所有的应用程序
$ pm2 delete all # 关闭并删除所有应用
$ pm2 logs # 控制台显示所有日志

$ pm2 start 0  # 启动 id为 0的指定应用程序
$ pm2 restart 0  # 重启 id为 0的指定应用程序
$ pm2 stop 0 # 停止 id为 0的指定应用程序
$ pm2 delete 0 # 删除 id为 0的指定应用程序

$ pm2 logs 0 # 控制台显示编号为0的日志
$ pm2 show 0  # 查看执行编号为0的进程
$ pm2 monit jsyfShopNuxt # 监控名称为jsyfShopNuxt的进程
```
## **工程部署**<br/>
### （1） Git下载代码。
``` bash
$ git clone https://github.com/Marine1125/mumutea.git
```
### （2） 安装依赖。
``` bash
$ npm install
```
### （3）pm2守护启动服务。
``` bash
# 进入到项目目录
$ pm2 start npm --name "mumutea" -- run start
```