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

### ソースからビルド

```
## 
## 依存パッケージをインストール
## 
yum -y install openssl-devel gcc make pcre pcre-devel wget

## 
## Apache本体のソースをdownload
## 
mkdir -p /usr/local/src/
cd /usr/local/src/
wget "http://mirrors.ibiblio.org/apache/httpd/httpd-2.4.10.tar.gz"
tar zxvf httpd-2.4.10.tar.gz
cd httpd-2.4.10/srclib/

## 
## Apache2.4のSource Installでは、apr, apr-utilをそれぞれ別途ソースをダウンロード・インストールする必要があるが、
## それぞれダウンロードしてapr, apr-utilという名前で展開したものをsrclib下に移動して、
## --with-included-aprオプションをつけてビルドすればよい
## 

wget "http://mirror.metrocast.net/apache/apr/apr-1.5.1.tar.gz"
tar zxvf apr-1.5.1.tar.gz
mv -i apr-1.5.1 apr

wget "http://mirror.metrocast.net/apache/apr/apr-util-1.5.4.tar.gz"
tar zxvf apr-util-1.5.4.tar.gz
mv -i apr-util-1.5.4 apr-util
```

```
cd /usr/local/src/httpd-2.4.10
./configure --prefix=/usr/local/apache-2.4.10 \
            --enable-mpms-shared=all \
            --enable-mods-shared=all \
            --enable-ssl \
            --enable-proxy \
            --enable-cache \
            --enable-mem-cache \
            --enable-rewrite \
            --with-included-apr

make
make install
```

バージョン確認

```
/usr/local/apache-2.4.10/bin/apachectl -v
```

#### 後の設定

起動スクリプトの設定、chkconfigを使ってserviceへの登録など
