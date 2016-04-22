# iptables
### 設定をクリア
```sh
/sbin/iptables -F
/sbin/iptables -X
```
### ポリシーの設定
```sh
/sbin/iptables -P INPUT DROP
/sbin/iptables -P OUTPUT DROP
/sbin/iptables -P FORWARD DROP
```
### yum wget ftpを許可
```sh
/sbin/iptables -A INPUT -p tcp --sport 80 -j ACCEPT
/sbin/iptables -A INPUT -p tcp --sport 21 -j ACCEPT
/sbin/iptables -A INPUT -p tcp --sport 20 -j ACCEPT
/sbin/iptables -A OUTPUT -p tcp --dport 80 -j ACCEPT
/sbin/iptables -A OUTPUT -p tcp --dport 21 -j ACCEPT
/sbin/iptables -A OUTPUT -p tcp --dport 20 -j ACCEPT
```
### dnsを許可
```sh
/sbin/iptables -A INPUT -p udp --sport 53 -j ACCEPT
/sbin/iptables -A OUTPUT -p udp --dport 53 -j ACCEPT
```
### pingを許可
```sh 
/sbin/iptables -A INPUT -p icmp -j ACCEPT
```
### ループバックを許可
```sh
/sbin/iptables -A INPUT -i lo -j ACCEPT
```
### 特定のipアドレスからのみsshを受け付ける
```sh
/sbin/iptables -A INPUT -s XXX.XXX.XXX.XXX -p tcp --sport 22 -j ACCEPT
/sbin/iptables -A INPUT -d XXX.XXX.XXX.XXX -p tcp --dport 22 -j ACCEPT
```
