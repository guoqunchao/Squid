## 使用Squid搭建代理服务器  

Server端配置
Squid是Linux自带的代理软件，与其它代理软件如Apache、Socks等相比，下载安装简单，配置灵活，支持缓存和多种协议。

```shell
[root@iZt4nafbsyuy7qm2iwuyu6Z ~]# yum install squid -y
[root@iZt4nafbsyuy7qm2iwuyu6Z ~]# yum install httpd-tools -y
[root@iZt4nafbsyuy7qm2iwuyu6Z ~]# htpasswd -cd /etc/squid/passwords admin  #生成密码文件
[root@iZt4nafbsyuy7qm2iwuyu6Z ~]# /usr/lib64/squid/basic_ncsa_auth /etc/squid/passwords #测试密码文件
#输入用户名 密码
admin 123456
#提示ok说明成功
ok
#ctrl+c退出
[root@iZt4nafbsyuy7qm2iwuyu6Z ~]# vim /etc/squid/squid.conf

#在最后添加
auth_param basic program /usr/lib64/squid/basic_ncsa_auth /etc/squid/passwords
auth_param basic realm proxy
acl authenticated proxy_auth REQUIRED
http_access allow authenticated

#这里是端口号，可以按需修改
http_port 0.0.0.0:3128
[root@iZt4nafbsyuy7qm2iwuyu6Z ~]# systemctl start squid.service
[root@iZt4nafbsyuy7qm2iwuyu6Z ~]# systemctl enable squid.service
```

## Client端配置
```shell
#linux客户端
vi /etc/profile
#在最后加入
export http_proxy="http://admin:123456@proxy_ip:port"

#yum代理 编辑/etc/yum.conf，在最后加入
proxy=http://username:password@proxy_ip:port/
```

```shell
#Windows客户端
通过全局代理上网，http验证即可！
```



