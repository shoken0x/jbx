2014/10/30 午後 これまでのサービスサーバの復習
===

## 0. OSのインストール

- CentOS 6.5 をminimalでインストールする
- ネットワークの設定
 - /etc/resolv.confの設定
 - /etc/sysconfig/network-scripts/ifcfg-eth0の設定
 - /etc/hostsの設定
- SELinuxをオフ


## 1. httpd

#### ベーシック

- ブラウザから80ポートにアクセスして、Apache httpdサーバーの画面が確認できること
- Nameベースのバーチャルホスト設定ができること。設定したFQDNで、違う画面が出ることの確認。

#### アドバンス

- SSL証明書（オレオレ証明書）の作成し、httpsで通信できること。
- Basic認証の設定。
- アクセスログのカスタマイズ

## 2. mail

#### ベーシック

- telnetを使ったメール送信。/var/log/maillogでログが出力されることの確認。

#### アドバンス

- Linuxのユーザーにメールを送信する。
- mailコマンドを使って、受信していることの確認。

## 3. LDAP

#### ベーシック

- ldifファイルのインポート
- ldapsearchを使った検索

#### アドバンス


## 4. samba

#### ベーシック

- Windowsから共有ファイルが確認できること。

#### アドバンス
