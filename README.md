# Caddy
Caddy setting

first run:
```bash
yum -y install wget
```
yum install gcc
yum install epel-release
yum install libsodium

first, please don't reboot vps ********

01:install bbr plus , select 2- BBRplus
```bash
wget --no-check-certificate -O onefast.sh https://raw.githubusercontent.com/caonimagfw/onefast/master/onefast.sh && bash onefast.sh
```
02:install ssr for caddy
```bash
wget --no-check-certificate -O shadowsocksRForCaddy.sh https://raw.githubusercontent.com/caonimagfw/ssr/master/shadowsocksRForCaddy.sh && bash shadowsocksRForCaddy.sh
```
  
02:install ssr IPV4 for caddy
```bash
wget --no-check-certificate -O shadowForCaddyIpv4.sh https://raw.githubusercontent.com/caonimagfw/ssr/master/shadowForCaddyIpv4.sh && bash shadowForCaddyIpv4.sh
```

03:install caddy 
```bash
wget --no-check-certificate -O caddy_install.sh https://raw.githubusercontent.com/caonimagfw/Caddy/master/caddy_install.sh && bash caddy_install.sh
```
reboot vps

run 01:

select bbr plus 
```bash
run bash onefast.sh: select 5:open bbr plus mode 
run bash onefast.sh: select 8: optimizing network setting 
```

modify ssr setting:
```bash
bash shadowsocksRForCaddy.sh modify
```

vim /etc/selinux/config

禁止ping 
```bash
# echo “net.ipv4.icmp_echo_ignore_all = 1” >> /etc/sysctl.conf 
# sysctl -p


firewall-cmd --zone=public --add-port=22/tcp --permanent
firewall-cmd --zone=public --add-port=80/tcp --permanent

firewall-cmd --zone=public --add-port=9999/tcp --permanent

firewall-cmd --zone=public --remove-port=22/tcp --permanent
```

#修改ssh 端口
```bash
vi /etc/ssh/sshd_config
systemctl restart sshd
``` 

安装Firewall命令
```bash
yum install firewalld firewalld-config
```
开启关闭防火墙示例（示例中的端口改成你的就行）
#添加防火墙8080端口
```bash
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --zone=public --add-port=8080/udp --permanent
```
#关闭防火墙8080端口
```bash
firewall-cmd --zone=public --remove-port=8080/tcp --permanent
firewall-cmd --zone=public --remove-port=8080/udp --permanent
```
#批量添加5000-5500区间端口
```bash
firewall-cmd --zone=public --add-port=5000-5500/udp --permanent
firewall-cmd --zone=public --add-port=5000-5500/tcp --permanent
```
#批量关闭5000-5500区间端口
```bash
firewall-cmd --zone=public --remove-port=5000-5500/udp --permanent
firewall-cmd --zone=public --remove-port=5000-5500/tcp --permanent
```
开启/禁用/重启防火墙命令
#开启防火墙
```bash
systemctl start firewalld.service
```
#重启防火墙（添加或关闭端口操作后一定要重启防火墙使其生效，下面两条都是重启命令，随意哪一种）
```bash
firewall-cmd --reload
service firewalld restart
```
#关闭禁用防火墙（嫌一个个开端口麻烦直接关闭防火墙也可以）
```bash
systemctl stop firewalld
```
设置开机（禁用开机）防火墙
#开机启动
```bash
systemctl enable firewalld
```
#停止并禁用开机启动
```bash
systemctl disable firewalld
```
查看所有已开放端口
```bash
firewall-cmd --permanent --list-port
```
查看防火墙状态
#下面两条命令都是查看状态，第一种可查看详细状态
```bash
systemctl status firewalld
```
#这个只能看到状态是否运行
```bash
firewall-cmd --state
```

systemctl mask --now firewalld
#ipv6添加, 设置IPV6_AUTOCONF = "yes"
```bash
vi /etc/sysconfig/network
```

#修改ip地址
```bash
vi /etc/sysconfig/network-scripts/ifcfg-eth0

IPV6INIT=yes
IPV6ADDR=第一个ipv6/64
IPV6ADDR_SECONDARIES="第二个ipv6/64 第三个ipv6/64 第N个IPV6/64"  ###注意中间有空格
IPV6_DEFAULTGW="fe80::1"
DNS3=2001:4860:4860::8888
DNS4=2001:4860:4860::8844

```

```
systemctl restart network
/etc/ssh/sshd_config

ClientAliveInterval 120
ClientAliveCountMax 720

dpkg --add-architecture i386

apt-get update
make V=99 -i

```

#申请 证书
```
yum -y install git
python -v #确保Python版本大于2.7
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel
git clone https://github.com/letsencrypt/letsencrypt
cd letsencrypt/
./certbot-auto --server https://acme-v02.api.letsencrypt.org/directory -d "*.7even.online" --manual --preferred-challenges dns-01 certonly
--manual交互式获取，--preferred-challenges dns使用DNS验证的方式（泛域名只能使用DNS验证），--server指明支持acme-v02的Server地址，默认是acme-v01的地址。

```

#文件介绍
```
生成证书中会创建 /etc/letsencrypt 文件夹，证书文件默认存放在 /etc/letsencrypt/live/example.com 文件夹中，在 example.com 文件夹中包含 4 个文件
• cert.pem 域名证书
• chain.pem 根证书及中间证书
• fullchain.pem 由 cert.pem 和 chain.pem 合并而成
• privkey.pem 证书私钥

1
$ certbot-auto renew

toyoo.pw {
 gzip
 tls /root/xxx.crt /root/xxx.key
 proxy / https://www.google.com.hk
}
```



#443 use ####################
```
#caddy --------------
:8100 {
	root /usr/local/caddy/www
	timeouts none
	tls /root/ssl/bmwpay.net/cert.pem /root/ssl/bmwpay.net/privkey.pem
	gzip
}

:80 {
	redir https://xxx.net{uri}
}

#config--------------
"server_port":443
 "redirect":["*:443#127.0.0.1:8100"],
```

#80 use ###############
```
#caddy --------------
:443 {
	root /usr/local/caddy/www
	timeouts none
	tls /root/ssl/bmwpay.net/cert.pem /root/ssl/bmwpay.net/privkey.pem
	gzip
}

:8099 {
	redir https://xxx.net{uri}
}

#config--------------
"server_port":80
 "redirect":["*:80#127.0.0.1:8099"],
```
