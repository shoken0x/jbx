### SSL証明書

#### mod_sslのインストール

```
yum install mod_ssl
```

#### 証明書（key, crt）の作成

```
openssl genrsa 2048 > server.key
openssl req -new -key server.key > server.csr
openssl x509 -days 3650 -req -signkey server.key < server.csr > server.crt
```

#### /etc/httpd/conf.d/ssl.confの編集

```
SSLCertificateFile /etc/httpd/conf/ssl/server.crt
SSLCertificateKeyFile /etc/httpd/conf/ssl/server.key 
```

DocumentRootの設定とindex.htmlの作成

### BASIC認証

#### パスワードファイルの作成

```
mkdir /etc/httpd/basic/
htpasswd -c "/etc/httpd/basic/passwd" fujisaki
```

#### basic認証の設定

/etc/httpd/conf/httpd.conf

```
<Directory "/usr/share/httpd/fujisaki/basic">
    AuthType Basic
    AuthName "ユーザー名とパスワードを入力して下さい"
    AuthUserFile "/etc/httpd/basic/passwd"
    Require valid-user
</Directory>
```
