---
title: Ngrok实现内网穿透
date: 2019-05-20 17:03:43
categories: 开发环境
tags:
- Ngrok
description: 手动搭建便于开发的Ngrok服务
---
## 什么是ngrok
ngrok 是一个反向代理，通过在公共的端点和本地运行的 Web 服务器之间建立一个安全的通道。ngrok 可捕获和分析所有通道上的流量，便于后期分析和重放。

## 内网穿透
内网穿透，即NAT穿透，网络连接时术语，计算机是局域网内时，外网与内网的计算机节点需要连接通信，有时就会出现不支持内网穿透。穿透后，你个人电脑指定的端口就会暴露在外网，可以通过域名来访问到你提供的服务。

## 典型用例
微信开发。如果你没有拥有内网穿透的工具，你要在微信测试公众号上面调试功能，只能发布到测试环境，严重影响开发效率。

## ngrok服务搭建
### 准备资源
1. 服务器1台，如：阿里云CentOS 6.8 64位，假设IP是1.2.3.4。
2. 域名1个，已备案。如：ngrok.xxx.com，新增以下两个解析（本文在二级域名基础上做展示，即内网穿透要使用到三级域名，需要做泛解析）
3. ngrok.xxx.com 映射到 1.2.3.4
4. *.ngrok.xxx.com 映射到 1.2.3.4
5. 配置好后，可以ping一下随便一个三级域名，如：dev.ngrok.xxx.com，如果能ping通那就可以继续往下了。

## 环境准备
~~~
yum -y install zlib-devel openssl-devel perl hg cpio expat-devel gettext-devel curl curl-devel perl-ExtUtils-MakeMaker hg wget gcc gcc-c++ git
~~~

## GO语言环境
1. 首先拿梯子去go官网下载对应的安装包 ，64位 OR 32位 | 当然其他地方也有的 : )。
2. 将文件上传到服务器
3. 执行以下命令
4. 解压：tar -C /usr/local -xzf go1.12.linux-amd64.tar.gz
5. 设置环境：vim /etc/profile
    新增一行 export PATH=$PATH:/usr/local/go/bin
6. 使环境生效：source /etc/profile
7. 验证：go version
   输出[go version go1.12 linux/amd64]就代表万事大吉了。
## ngrok服务
### 下载源码
~~~
git clone https://github.com/inconshreveable/ngrok.git
cd ngrok
~~~

### 生成证书
~~~
NGROK_DOMAIN="ngrok.xxx.com"

openssl genrsa -out base.key 2048
openssl req -new -x509 -nodes -key base.key -days 10000 -subj "/CN=$NGROK_DOMAIN" -out base.pem
openssl genrsa -out server.key 2048
openssl req -new -key server.key -subj "/CN=$NGROK_DOMAIN" -out server.csr
openssl x509 -req -in server.csr -CA base.pem -CAkey base.key -CAcreateserial -days 10000 -out server.crt
~~~~

### 替换证书
~~~
cp base.pem assets/client/tls/ngrokroot.crt
cp server.crt assets/server/tls/snakeoil.crt
cp dserver.key assets/server/tls/snakeoil.key
~~~

###编译
~~~
make release-server
~~~~

### 运行
~~~
./bin/ngrokd -tlsKey="assets/server/tls/snakeoil.key" -tlsCrt="assets/server/tls/snakeoil.crt" -domain="wechat.userwyh.com"  -httpAddr=":80" -httpsAddr=":443" -tunnelAddr=":4443"
#参数说明：
#-domain 生成证书时配置的NGROK_DOMAIN
#-httpAddr http协议端口，默认为80
#-httpsAddr https协议端口，默认为443
#-tunnelAddr 通道端口，默认4443
~~~

### 后台运行
~~~
setsid ./bin/ngrokd -tlsKey="assets/server/tls/snakeoil.key" -tlsCrt="assets/server/tls/snakeoil.crt" -domain="ngrok.xxx.com"  -httpAddr=":80" -httpsAddr=":443" -tunnelAddr=":4443"
~~~

### 客户端
客户端生成根据自己电脑的实际环境执行相应的命令。执行成功后在bin目录下会有一个windows_amd64，把它下载到自己的电脑上。

~~~
GOOS=windows GOARCH=amd64 make release-client
----------------------------------------------------------------------------------------
#MAC 平台 32 位系统：GOOS=darwin GOARCH=386 make release-client
#MAC 平台 64 位系统：GOOS=darwin GOARCH=amd64 make release-client
#ARM 平台：GOOS=linux GOARCH=arm make release-client
#Linux 平台 32 位系统：GOOS=linux GOARCH=386 make release-client
#Linux 平台 64 位系统：GOOS=linux GOARCH=amd64 make release-client
#Windows 平台 32 位系统：GOOS=windows GOARCH=386 make release-client
#Windows 平台 64 位系统：GOOS=windows GOARCH=amd64 make release-client
~~~

下载完成后，在ngrok.exe同级目录下创建一个简单的配置文件ngrok.cfg

~~~
server_addr: "ngrok.xxx.com:4443"
trust_host_root_certs: false
~~~

注意：server_addr由在CentOS上面配置的NGROK_DOMAIN:tunnelAddr组成
然后可以使用命令启动

~~~
ngrok -config=ngrok.cfg -log=ngrok.log -subdomain=dev 8080
#参数说明：
-config：你的配置文件，就上面创建那个
-log：日志文件地址
-subdomain：三级域名 本地服务端口号
~~~

启动正常的话会控制台会输出如下内容，此时你就可以通过域名【http://dev.ngrok.xxx.com】来访问你的本地服务了。如果访问不了，可以看看自己域名映射配置正确了没有，看【准备资源】一节。

~~~
ngrok                                                           (Ctrl+C to quit)

Tunnel Status                 online
Version                       1.7/1.7
Forwarding                    http://dev.ngrok.xxx.com -> 127.0.0.1:8080
Forwarding                    https://dev.ngrok.xxx.com -> 127.0.0.1:8080
Web Interface                 127.0.0.1:4040
# Conn                        0
Avg Conn Time                 0.00ms
~~~

## nginx配置
上面的配置已经实现了内网穿透，但是你会发现访问的时候要带个端口号，但是微信开发的时候只能使用80端口，就是说到这里，我们还是白忙活，还是不能在本地快乐的编码。我们一般也不能直接把80端口配置给了ngrok，因为80端口一般是nginx这个大佬占据的，所以我们只能在nginx上面做一下转发，如下：

~~~
server {
       listen 80;
       server_name dev.ngrok.xxx.com;
       location / {
           proxy_pass  http://dev.wechat.userwyh.com:8888/;
           proxy_redirect off;
           client_max_body_size 10m; 
           client_body_buffer_size 128k;
           proxy_connect_timeout 90; 
           proxy_read_timeout 90; 
           proxy_buffer_size 4k;  
           proxy_buffers 6 128k;
           proxy_busy_buffers_size 256k;
           proxy_temp_file_write_size 256k;
       }
}
~~~

然后重启一下nginx，你就可以使用不带端口号的域名【http://dev.ngrok.xxx.com】进行访问了。
如果你觉得可以使用80端口给ngrok，那么也是可以的，忽略本节。