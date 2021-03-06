
## Host1Plus 备忘录

可以访问:[博客2017](http://lishangxi.cn)

> IP ==185.141.165.28==

> 账号: 105****063@qq.com

## nginx 启动

> ngnix版本为 1.6.2

> nginx监听端口 ==80==

#### 启动命令

```
/usr/local/webserver/nginx/sbin/nginx 

```

#### Nginx 其他命令

```
/usr/local/webserver/nginx/sbin/nginx -s reload            # 重新载入配置文件
/usr/local/webserver/nginx/sbin/nginx -s reopen            # 重启 Nginx
/usr/local/webserver/nginx/sbin/nginx -s stop              # 停止 Nginx

```

> 配置文件

```
/usr/local/webserver/nginx/conf/nginx.conf
```
> nodejs配置---端口与域名配置

[多域名配置参考文章](http://www.cnblogs.com/freespider/p/4684586.html)

```
server {
listen 80;
server_name lishangxi.cn; #域名
    location / {
    proxy_pass http://127.0.0.1:8000;
    }
}
### 配置多域名
server {
listen 80;
server_name chat.lishangxi.cn; #域名
    location / {
    proxy_pass http://127.0.0.1:3000;
    }
}
```
==Nginx解析域名,转发给本地的nodejs的8000端口~==

> nginx安装及配置参考说明

[配置参考文章](http://www.runoob.com/linux/nginx-install-setup.html)

### Node.js项目监听端口

> blog2017 监听端口 ==8000==

> HiChat 监听端口 ==3000==

### 启动mongodb

#### 端口

`启动端口在 57*47`

> mongodb安装位置 /usr/local/mongodb

> ==手动启动 /usr/local/mongodb/bin/mongo== 

- 1.下载MongoDB（64位）

`http://fastdl.mongodb.org/linux/mongodb-linux-x86_64-2.4.9.tgz`
或
`http://pan.baidu.com/s/1mgyRB8c`

- 2.安装MongoDB（安装到/usr/local）

```
tar zxvf mongodb-linux-x86_64-2.4.9.tgz
mv mongodb-linux-x86_64-2.4.9 mongodb
cd mongodb
mkdir db
mkdir logs
cd bin
vi mongodb.conf

dbpath=/usr/local/mongodb/db
logpath=/usr/local/mongodb/logs/mongodb.log
port=27017
fork=true
nohttpinterface=true

```
- 3.重新绑定mongodb的配置文件地址和访问IP
```
/usr/local/mongodb/bin/mongod --bind_ip localhost -f /usr/local/mongodb/bin/mongodb.conf
```
- 4.开机自动启动mongodb
```
vi /etc/rc.d/rc.local
/usr/local/mongodb/bin/mongod --config /usr/local/mongodb/bin/mongodb.conf
```
- 5.重启一下系统测试下能不能自启
```
#进入mongodb的shell模式 
/usr/local/mongodb/bin/mongo
#查看数据库列表 
show dbs
#当前db版本 
```

> 

### pm2常用命令

- pm2 list ==启动项目的列表==

![](https://img3.doubanio.com/view/note/large/public/p10127862.jpg)
- pm2 stop [id] 停止项目
- pm2 restart [id] 重启项目
- pm2 restart all ==重启所有进程==
- pm2 monit ==监视所有进程==

![pic](https://img1.doubanio.com/view/note/large/public/p10140518.jpg)

- pm2 logs ==显示所有进程的日志==
- pm2 stop all ==停止所有进程==


```
$ npm install pm2 -g     # 命令行安装 pm2 
$ pm2 start app.js -i 4 #后台运行pm2，启动4个app.js 
                                # 也可以把'max' 参数传递给 start
                                # 正确的进程数目依赖于Cpu的核心数目
$ pm2 start app.js --name my-api # 命名进程
$ pm2 list               # 显示所有进程状态
$ pm2 monit              # 监视所有进程
$ pm2 logs               #  显示所有进程日志
$ pm2 stop all           # 停止所有进程
$ pm2 restart all        # 重启所有进程
$ pm2 reload all         # 0秒停机重载进程 (用于 NETWORKED 进程)
$ pm2 stop 0             # 停止指定的进程
$ pm2 restart 0          # 重启指定的进程
$ pm2 startup            # 产生 init 脚本 保持进程活着
$ pm2 web                # 运行健壮的 computer API endpoint (http://localhost:9615)
$ pm2 delete 0           # 杀死指定的进程
$ pm2 delete all         # 杀死全部进程

运行进程的不同方式：
$ pm2 start app.js -i max  # 根据有效CPU数目启动最大进程数目
$ pm2 start app.js -i 3      # 启动3个进程
$ pm2 start app.js -x        #用fork模式启动 app.js 而不是使用 cluster
$ pm2 start app.js -x -- -a 23   # 用fork模式启动 app.js 并且传递参数 (-a 23)
$ pm2 start app.js --name serverone  # 启动一个进程并把它命名为 serverone
$ pm2 stop serverone       # 停止 serverone 进程
$ pm2 start app.json        # 启动进程, 在 app.json里设置选项
$ pm2 start app.js -i max -- -a 23                   #在--之后给 app.js 传递参数
$ pm2 start app.js -i max -e err.log -o out.log  # 启动 并 生成一个配置文件
你也可以执行用其他语言编写的app  ( fork 模式):
$ pm2 start my-bash-script.sh    -x --interpreter bash
$ pm2 start my-python-script.py -x --interpreter python

0秒停机重载:
这项功能允许你重新载入代码而不用失去请求连接。
注意：
仅能用于web应用
运行于Node 0.11.x版本
运行于 cluster 模式（默认模式）
$ pm2 reload all

CoffeeScript:
$ pm2 start my_app.coffee  #这就是全部

PM2准备好为产品级服务了吗？
只需在你的服务器上测试
$ git clone https://github.com/Unitech/pm2.git
$ cd pm2
$ npm install  # 或者 npm install --dev ，如果devDependencies 没有安装
$ npm test

pm2 list
```

### 配置VPN服务器

> 一键部署VPS（复制下面的代码并按回车。 中途会提示设置端口和密码，如果直接回车使用的是默认配置）


```
wget –no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh  
chmod +x shadowsocks.sh  
./shadowsocks.sh 2>&1 | tee shadowsocks.log  
```

### 设置环境变量

> window 下 在cmd下  
` set NODE_ENV = development `

> linux 下
` export NODE_ENV = production `

### CentOS7安装firewalld

```
sudo yum install firewalld # installs firewalld
sudo systemctl enable firewalld # enables firewalld to run on boot
sudo systemctl start firewalld # starts firewalld
```

### CentOS7使用firewalld打开关闭防火墙与端口

```
1、firewalld的基本使用
启动： systemctl start firewalld
查看状态： systemctl status firewalld 
停止： systemctl disable firewalld
禁用： systemctl stop firewalld
 
2.systemctl是CentOS7的服务管理工具中主要的工具，它融合之前service和chkconfig的功能于一体。
启动一个服务：systemctl start firewalld.service
关闭一个服务：systemctl stop firewalld.service
重启一个服务：systemctl restart firewalld.service
显示一个服务的状态：systemctl status firewalld.service
在开机时启用一个服务：systemctl enable firewalld.service
在开机时禁用一个服务：systemctl disable firewalld.service
查看服务是否开机启动：systemctl is-enabled firewalld.service
查看已启动的服务列表：systemctl list-unit-files|grep enabled
查看启动失败的服务列表：systemctl --failed

3.配置firewalld-cmd

查看版本： firewall-cmd --version
查看帮助： firewall-cmd --help
显示状态： firewall-cmd --state
查看所有打开的端口： firewall-cmd --zone=public --list-ports
更新防火墙规则： firewall-cmd --reload
查看区域信息:  firewall-cmd --get-active-zones
查看指定接口所属区域： firewall-cmd --get-zone-of-interface=eth0
拒绝所有包：firewall-cmd --panic-on
取消拒绝状态： firewall-cmd --panic-off
查看是否拒绝： firewall-cmd --query-panic
 
那怎么开启一个端口呢
添加
firewall-cmd --zone=public --add-port=80/tcp --permanent    （--permanent永久生效，没有此参数重启后失效）
重新载入
firewall-cmd --reload
查看
firewall-cmd --zone= public --query-port=80/tcp
删除
firewall-cmd --zone= public --remove-port=80/tcp --permanent
 
```
