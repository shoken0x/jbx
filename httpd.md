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



