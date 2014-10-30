## 認証無し

### コマンド

```
su -
yum -y install samba
mkdir /home/samba
chown nobody:nobody /home/samba
chmod 777 /home/samba
vi /etc/samba/smb.conf
/etc/init.d/smb start
```


### smb.confの変更箇所

#### [global]セクション

- workgroupをWindowsのネットワーク名へ変更
- map to guest = Bad Userの追加

#### [public]セクション

- 全てのコメントアウトを外す

## 認証有り

###

1. map to guest = Bad Userのコメントアウト

2. ユーザーを追加

```
useradd -s /bin/false smbuser
smbpasswd -a smbuser
```
