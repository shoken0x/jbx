### コマンド
su -
yum -y install
mkdir /home/samba
chown nobody:nobody /home/samba
chmod 777 /home/samba
vi /etc/samba/smb.conf
/etc/init.d/smb start

### smb.confの変更箇所

#### [global]セクション

- workgroupをWindowsのネットワーク名へ変更
- map to guest = Bad Userの追加

#### [public]セクション

- 全てコメントアウト

