2014/10/30 午後 これまでのサービスサーバの復習
===

## 0. OSのインストール

- CentOS 6.5 をminimalでインストールする
- ネットワークの設定
 - /etc/sysconfig/network-scripts/ifcfg-eth0の設定
 - /etc/sysconfig/network でDefault GATEWAYの設定
 - /etc/resolv.confの設定
 - /etc/hostsの設定（必要ならば）
- SELinuxをオフ
- iptablesの編集（もしくはストップ）

手順はこちら  
https://github.com/syokenz/jbx/blob/master/initial.md

## 1. httpd

#### ベーシック

- ブラウザから80ポートにアクセスして、Apache httpdサーバーの画面が確認できること

```
yum -y install httpd
service httpd start
```

- Nameベースのバーチャルホスト設定ができること。設定したFQDNで、違う画面が出ることの確認。

#### アドバンス

- SSL証明書（オレオレ証明書）の作成し、httpsで通信できること。
- Basic認証の設定。
- アクセスログのカスタマイズ
- 起動ポートを8081へ変更

## 2. mail

#### ベーシック

- telnetを使ったメール送信。/var/log/maillogでログが出力されることの確認。

```
yum -y install postfix
yum -y install mailx
```

#### アドバンス

- Linuxのrootユーザーにメールを送信する。
- mailコマンドを使って、受信していることの確認。

/etc/hosts

```
127.0.0.1 jbcc.jp localhost...
192.168.xxx.xxx jbcc.jp
```


## 3. LDAP

#### ベーシック

- ldifファイルのインポート
- ldapsearchを使った検索

#### アドバンス

- 下記URLのデータを作成してください。ただし、suffixは"dc=jbcc,dc=jp"とすること。
- http://www.unix-power.net/linux/img/openldap1.jpg 

## 4. samba

#### ベーシック

- Windowsから、認証無しで共有ファイルが確認できること。

#### アドバンス

- Linuxユーザーの認証ありのファイル共有
- SWATのインストールと確認

## 5. 終わった人用

- Apache httpdをソースからコンパイルしてビルド。81ポートで起動させる。
