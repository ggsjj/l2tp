# 中文

Debian 8 改内核

echo >/etc/resolv.conf && echo "nameserver 8.8.8.8" >> /etc/resolv.conf && echo "nameserver 8.8.4.4" >> /etc/resolv.conf && apt-get install vim -y && wget http://security-cdn.debian.org/pool/updates/main/l/linux/linux-image-3.16.0-4-amd64_3.16.43-2+deb8u5_amd64.deb && dpkg -i linux-image-3.16.0-4*.deb && apt-get -y remove linux-image-3.16.0-6-amd64 && reboot

wget --no-check-certificate https://raw.githubusercontent.com/ggsjj/l2tp/master/l2tp.sh && chmod +x l2tp.sh && ./l2tp.sh


锐速

wget --no-check-certificate -qO /tmp/appex.sh "https://raw.githubusercontent.com/0oVicero0/serverSpeeder_Install/master/appex.sh" && bash /tmp/appex.sh 'install'

# ubuntu16.04修改网卡名称enp2s0为eth0<当安装不了查看网卡名称>

ifconfig

1、vim /etc/default/grub 


```bash
找到GRUB_CMDLINE_LINUX=""
改为GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"
然后sudo grub-mkconfig -o /boot/grub/grub.cfg
重启后，网卡名称果然变成了eth0和wlan01234
```

2、打开ubuntu的/etc/network/interfaces文件默认的内容如下：


```bash
auto lo
iface lo inet loopback12
```

在后面添加内容 
1、获取动态配置：


```bash
auto eth0
iface eth0 inet dhcp
```


基于 OpenVZ 虚拟化技术的 VPS 需要开启TUN/TAP才能正常使用，购买 VPS 时请先咨询服务商是否支持开启 TUN/TAP。

OpenVZ 虚拟的 VPS 需要系统内核支持 IPSec 才行。也就是说，母服务器的内核如果不支持的话那就没办法，只能换 VPS。
因此，一般不建议在 OpenVZ 的 VPS 上安装本脚本。脚本如果检测到该 VPS 为 OpenVZ 架构，会出现警告提醒。

如何检测是否支持TUN模块？ 

执行命令：

cat /dev/net/tun

如果返回信息为：cat: /dev/net/tun: File descriptor in bad state 说明正常

#如何检测是否支持ppp模块？

执行命令：

cat /dev/ppp

如果返回信息为：cat: /dev/ppp: No such device or address 说明正常

当然，脚本在安装时也会执行检查，如果不适用于安装，脚本会予以提示。

# 使用方法：

root 用户登录后，运行以下命令：

wget --no-check-certificate https://raw.githubusercontent.com/teddysun/across/master/l2tp.sh && chmod +x l2tp.sh && ./l2tp.sh

# 执行后，会有如下交互界面

 L2TP

Please input IP-Range:

(Default Range: 192.168.18):

输入本地IP段范围（本地电脑连接到VPS后给分配的一个本地IP地址），直接回车意味着输入默认值192.168.18

Please input PSK:

(Default PSK: teddysun.com):

PSK意为预共享密钥，即指定一个密钥将来在连接时需要用到，直接回车意味着输入默认值teddysun.com

Please input Username:

(Default Username: teddysun):

Username意为用户名，即第一个默认用户。直接回车意味着输入默认值teddysun

Please input teddysun’s password:

(Default Password: Q4SKhu2EXQ):

输入用户的密码，默认会随机生成一个10位包含大小写字母和数字的密码，当然你也可以指定密码。

ServerIP:your_server_main_IP

显示你的 VPS 的主 IP（如果是多 IP 的 VPS 也只显示一个）

Server Local IP:192.168.18.1

显示你的 VPS 的本地 IP（默认即可）

Client Remote IP Range:192.168.18.2-192.168.18.254

显示 IP 段范围

PSK:teddysun.com

显示 PSK

Press any key to start…or Press Ctrl+c to cancel

按下任意按键继续，如果想取消安装，请按Ctrl+c键

安装完成后，脚本会执行 ipsec verify 命令并提示如下：

If there are no [FAILED] above, then you can connect to your

L2TP VPN Server with the default Username/Password is below:

ServerIP:your_server_IP

PSK:your PSK

Username:your usename

Password:your password

 If you want to modify user settings, please use command(s):
 
 l2tp -a (Add a user)
 
 l2tp -d (Delete a user)
 
 l2tp -l (List all users)
 
 l2tp -m (Modify a user password)
 
 Welcome to visit https://teddysun.com/448.html
 
 Enjoy it!
 
如果你要想对用户进行操作，可以使用如下命令：

 l2tp -a 新增用户
 
 l2tp -d 删除用户
 
 l2tp -m 修改现有的用户的密码
 
 l2tp -l 列出所有用户名和密码
 
 l2tp -h 列出帮助信息

#其他事项：

1、脚本在安装完成后，已自动启动进程，并加入了开机自启动。

2、脚本会改写 iptables 或 firewalld 的规则。

3、脚本安装时，会即时将安装日志写到 /root/l2tp.log 文件里，如果你安装失败，可以通过此文件来寻找错误信息。


#使用命令：
 - ipsec status （查看 IPSec 运行状态）
 - ipsec verify （查看 IPSec 检查结果）
 - /etc/init.d/ipsec start|stop|restart|status （CentOS6 下使用）
 - /etc/init.d/xl2tpd start|stop|restart （CentOS6 下使用）
 - systemctl start|stop|restart|status ipsec （CentOS7 下使用）
 - systemctl start|stop|restart xl2tpd （CentOS7 下使用）
 - service ipsec start|stop|restart|status （Debian/Ubuntu 下使用）
 - service xl2tpd start|stop|restart （Debian/Ubuntu 下使用）











# Some useful scripts
l2tp.sh
=======

- Description: Auto install L2TP VPN for CentOS6+/Debian7+/Ubuntu12+
- Intro: https://teddysun.com/448.html
```bash
Usage: l2tp [-l,--list|-a,--add|-d,--del|-m,--mod|-h,--help]

| Bash Command     | Description                  |
|------------------|------------------------------|
| l2tp -l,--list   | List all users               |
| l2tp -a,--add    | Add a user                   |
| l2tp -d,--del    | Delete a user                |
| l2tp -m,--mod    | Modify a user password       |
| l2tp -h,--help   | Print this help information  |
```

bbr.sh
======

- Description: Auto install latest kernel for TCP BBR
- Intro: https://teddysun.com/489.html

kms.sh
======

- Description: Auto install KMS Server
- Intro: https://teddysun.com/530.html

bench.sh
========

- Description: Auto test download & I/O speed script
- Intro: https://teddysun.com/444.html
```bash
Usage:

| No.      | Bash Command                    |
|----------|---------------------------------|
| 1        | wget -qO- bench.sh | bash       |
| 2        | curl -Lso- bench.sh | bash      |
| 3        | wget -qO- 86.re/bench.sh | bash |
| 4        | curl -so- 86.re/bench.sh | bash |
```

backup.sh
=========

- You must modify the config before run it
- Backup MySQL/MariaDB/Percona datebases, files and directories
- Backup file is encrypted with AES256-cbc with SHA1 message-digest (option)
- Auto transfer backup file to Google Drive (need install `gdrive` command) (option)
- Auto transfer backup file to FTP server (option)
- Auto delete Google Drive's or FTP server's remote file (option)
- Intro: https://teddysun.com/469.html

```bash
Install gdrive command step:

For x86_64: 
wget -O /usr/bin/gdrive http://dl.lamp.sh/files/gdrive-linux-x64
chmod +x /usr/bin/gdrive

For i386: 
wget -O /usr/bin/gdrive http://dl.lamp.sh/files/gdrive-linux-386
chmod +x /usr/bin/gdrive
```

ftp_upload.sh
=============

- You must modify the config before run it
- Upload file(s) to FTP server
- Intro: https://teddysun.com/484.html

unixbench.sh
============

- Description: Auto install unixbench and test script
- Intro: https://teddysun.com/245.html

pptp.sh(Deprecated, DO NOT USE)
===================

- Description: Auto Install PPTP for CentOS 6
- Intro: https://teddysun.com/134.html

Copyright (C) 2013-2018 Teddysun <i@teddysun.com>
