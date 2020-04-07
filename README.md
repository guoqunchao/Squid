# Squid
内网的很多机器不能连接外网，只能开放几个特定的IP访问。我们可以在能上外网的机器上面搭建代理服务器，其它机器配置好代理就能上网了。

Server端配置
Squid介绍
Squid是Linux自带的代理软件，与其它代理软件如Apache、Socks等相比，下载安装简单，配置灵活，支持缓存和多种协议。


安装
yum install squid -y
yum install httpd-tools -y
1
2
生成密码文件
mkdir /etc/squid3/
#xiaodong 是用户名
htpasswd -cd /etc/squid3/passwords xiaodong
#提示输入密码，比如输入123456
1
2
3
4
测试密码文件
/usr/lib64/squid/basic_ncsa_auth /etc/squid3/passwords
#输入用户名 密码
xiaodong  123456
#提示ok说明成功
ok
#ctrl+c退出
1
2
3
4
5
6
配置squid.conf文件
vi /etc/squid/squid.conf
#在最后添加
auth_param basic program /usr/lib64/squid/basic_ncsa_auth /etc/squid3/passwords
auth_param basic realm proxy
acl authenticated proxy_auth REQUIRED
http_access allow authenticated

#这里是端口号，可以按需修改
#http_port 3128 这样写会同时监听ipv6和ipv4的端口，推荐适应下面的配置方法。
http_port 0.0.0.0:3128
1
2
3
4
5
6
7
8
9
10
日志
squid的日志位于/var/log/squid/目录下。

启动
#启动start（停止stop) 
systemctl start squid.service
#配置开机自启动
systemctl enable squid.service
1
2
3
4
说明
squid官方文档

Client端配置
Linux客户端
全局代理
vi /etc/profile
#在最后加入
export http_proxy="http://xiaodong:123456@proxy_ip:port"
export http_proxy="http://xiaodong:123456@proxy_ip:port"
1
2
3
4
yum代理
编辑/etc/yum.conf，在最后加入：

# Proxy
proxy=http://username:password@proxy_ip:port/
1
2
Windows客户端
windows客户端通过全局代理上网，建议采用Proxifier软件。Proxifier是一款功能非常强大的socks5客户端。配置方法如下：

配置代理服务器
打开代理工具，选择菜单栏的配置文件，选择代理服务器，在弹出的代理服务器对话框中选择添加按钮。


配置代理规则
用户可以自由选择访问哪些ip需要代理，哪些不需要。
