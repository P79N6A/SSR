#搭建方案
//安装纯净系统
apt-get install -y xz-utils openssl gawk file
wget --no-check-certificate -qO InstallNET.sh 'https://moeclub.org/attachment/LinuxShell/InstallNET.sh' && chmod a+x InstallNET.sh
bash InstallNET.sh -u 16.04 -v 64 -a
#root密码:Vicer
//vi无法正常编辑
apt-get remove vim-common
apt-get install vim
//修改网卡名称
vi /etc/default/grub
GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"
grub-mkconfig -o /boot/grub/grub.cfg
vi /etc/network/interfaces
//安装iptables
apt-get install iptables
//修改hostname
vi /etc/hostname
//修改时间
date -R
tzselect
cp /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime
ntpdate time.windows.com
//socks5
wget --no-check-certificate https://raw.github.com/Lozy/danted/master/install.sh -O socks5.sh
bash socks5.sh --port=999 --user=admin --passwd=admin
/etc/init.d/sockd status
//魔改BBR
wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
//anycoonect
wget -N --no-check-certificate https://softs.loan/Bash/ocserv.sh && chmod +x ocserv.sh && bash ocserv.sh
//ssr
wget -q -N --no-check-certificate https://git.fdos.me/stack/AR-B-P-B/raw/master/install.sh && bash install.sh develop
vi /usr/local/shadowsocksr/mudb.json
//ssrr
wget --no-check-certificate -O shadowsocksRR.sh https://git.io/vdMUr && chmod +x shadowsocksRR.sh && ./shadowsocksRR.sh 2>&1 | tee shadowsocksR.log
//SSR.GO
apt-get install curl
bash -c "$(curl -fsSL https://git.io/fNpuL)"
vi /etc/shadowsocks.json
/etc/init.d/shadowsocks restart
//配置文件
{
    "server":"0.0.0.0",
    "server_ipv6":"[::]",
    "server_port":443,
    "local_address":"127.0.0.1",
    "local_port":1080,
	
    "password":"admin443",
    "timeout":120,
    "udp_timeout":60,
    "method":"chacha20",
    "protocol":"auth_aes128_md5",
    "protocol_param":"443",
    "obfs":"tls1.2_ticket_auth",
    "obfs_param":"itunes.apple.com/cn/app/349",
	
	"dns_ipv6":false,
	"connect_verbose_info":1,
    "redirect":["*:443#itunes.apple.com/cn/app/349:443"],
    "dns_ipv6":false,
    "fast_open":false,
    "workers":1
}
//中转配置
iptables -t nat -A PREROUTING -p tcp -m tcp --dport 443 -j DNAT --to-destination 52.175.54.186:443
iptables -t nat -A PREROUTING -p udp -m udp --dport 443 -j DNAT --to-destination 52.175.54.186:443
iptables -t nat -A POSTROUTING -d 52.175.54.186 -p tcp -m tcp --dport 443 -j SNAT --to-source 10.0.0.4
iptables -t nat -A POSTROUTING -d 52.175.54.186 -p udp -m udp --dport 443 -j SNAT --to-source 10.0.0.4
