#### nicの設定

/etc/sysconfig/network-scripts/ifcfg-eth0

```
DEVICE=eth0    #デバイス名
HWADDR=00:0C:29:09:AE:8D    #NICのMACアドレス
TYPE=Ethernet
ONBOOT=yes  
BOOTPROTO=static    #固定IPの場合:(none|static)、DHCPの場合:dhcp
IPADDR=192.168.xxx.xxx #IPアドレスの指定
```

#### ネットワークの設定

/etc/sysconfig/network

```
NETWORKING=Yes
HOSTNAME=jbcc.jp
GATEWAY=192.168.xxx.2
```

#### リゾルバの設定

/etc/resolv.conf

```
nameserver 192.168.xxx.2
```


#### SELinuxのオフ

一時的

```
setenforce 0
```

再起動時にもオフにするには、/etc/sysconfig/selinux を編集する

#### iptablesのstop

一時的

```
service iptables stop
```

再起動時にもstopするには、chkconfigコマンドで設定する

```
chkconfig --level 345 iptables off
```
