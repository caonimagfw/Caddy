# Caddy
Caddy setting

first run:
```bash
yum -y install wget
```


first, please don't reboot vps ********

01:install bbr plus 
```bash
wget --no-check-certificate -O onefast.sh https://raw.githubusercontent.com/caonimagfw/onefast/master/onefast.sh && bash onefast.sh
```
02:install ssr for caddy
```bash
wget --no-check-certificate -O shadowsocksRForCaddy.sh https://raw.githubusercontent.com/caonimagfw/ssr/master/shadowsocksRForCaddy.sh && bash shadowsocksRForCaddy.sh
```
  
03:install caddy 
```bash
wget --no-check-certificate -O caddy_install.sh https://raw.githubusercontent.com/caonimagfw/Caddy/master/caddy_install.sh && bash caddy_install.sh
```
reboot vps

run 01:

select bbr plus 
```bash
run bash onefast.sh: select bbr plus mode 
run bash onefast.sh: select 11 optimizing network setting 
```

modify ssr setting:
bash shadowsocksRForCaddy.sh modify
