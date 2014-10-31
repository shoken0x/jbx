### 1. 仮想マシンの設定でハードディスク領域を追加(sdb, sdc)

仮想マシンの設定画面などからハードディスクを追加。  
rebootすると新しいデバイスが認識されている。  

```
# fdisk -l

Disk /dev/sdb: xxxx.0 GB, xxxxxxxxxxxxx bytes
xxx heads, xx sectors/track, xxxxxx cylinders
Units = シリンダ数 of xxxxx * xxx = xxxxxxx bytes

ディスク /dev/sdb は正常な領域テーブルを含んでいません

～略～

ディスク /dev/sdc は正常な領域テーブルを含んでいません
```

追加したデバイスをパーティションに登録 (/dev/sdb, /dev/sdc それぞれ)

- p 区画テーブルの表示(何も追加されていないことの確認）
- n 新しい区画を作成
- t 区画のシステムIDを変更(LVMとして使用するので 8e にする)
- w テーブルを書き込んで終了
- p 区画テーブルの表示(パーティションが追加されたことの確認）

```
# fdisk /dev/sdb
コマンド (m でヘルプ): p
Disk /dev/sdb: xxxx.0 GB, xxxxxxxxxxxxx bytes
xxx heads, xx sectors/track, xxxxxx cylinders
Units = シリンダ数 of xxxxx * xxx = xxxxxxx bytes

デバイス Boot      Start         End      Blocks   Id  System

コマンド (m でヘルプ): n
コマンドアクション
   e   拡張
   p   基本領域 (1-4)
p
領域番号 (1-4): 1
最初 シリンダ (1-xxxxxx, default 1): (Enter)
終点 シリンダ または +サイズ または +サイズM または +サイズK (1-xxxxxx, default xxxxxx): (Enter)

コマンド (m でヘルプ): t
Selected partition 1
16進数コード (L コマンドでコードリスト表示): 8e
領域のシステムタイプを 1 から 8e (Linux LVM) に変更しました

コマンド (m でヘルプ): w
領域テーブルは交換されました！

ioctl() を呼び出して領域テーブルを再読込みします。
ディスクを同期させます。
# fdisk /dev/sdb
コマンド (m でヘルプ): p

Disk /dev/sdb: xxxx.0 GB, xxxxxxxxxxxxx bytes
xxx heads, xx sectors/track, xxxxxx cylinders
Units = シリンダ数 of xxxxx * xxx = xxxxxxx bytes

デバイス Boot      Start         End      Blocks   Id  System
/dev/sdb1               1      xxxxxx  xxxxxxxxxx   8e  Linux LVM
```

#### PV(Physical Volume、物理ボリューム)を作成

```
# pvcreate /dev/sdb1
  Physical volume "/dev/sdb1" successfully created
# pvcreate /dev/sdc1
  Physical volume "/dev/sdc1" successfully created
# pvdisplay
  --- Physical volume ---
  PV Name               /dev/sdb1
  VG Name               VgData01
  PV Size               x.xx GB / not usable x.xx MB
  Allocatable           yes (but full)
  PE Size (KByte)       xxxx
  Total PE              xxxxxx
  Free PE               0
  Allocated PE          xxxxxx
  PV UUID               xxxxxx-xxxx-xxxx-xxxx-xxxx-xxxx-xxxxxx

  --- Physical volume ---
  PV Name               /dev/sdc1
  VG Name               VgData01
  PV Size               x.xx GB / not usable x.xx MB
  Allocatable           yes (but full)
  PE Size (KByte)       xxxx
  Total PE              xxxxxx
  Free PE               0
  Allocated PE          xxxxxx
  PV UUID               xxxxxx-xxxx-xxxx-xxxx-xxxx-xxxx-xxxxxx
```

### VG(Volume Group、ボリュームグループ)を作成


```
# vgcreate VgData01 /dev/sdb1 /dev/sdc1
  Volume group "VgData01" successfully created
# vgdisplay
  --- Volume group ---
  VG Name               VgData01
  System ID
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               1
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               x.xx TB
  PE Size               x.00 MB
  Total PE              xxxxxx
  Alloc PE / Size       xxxxxx / x.xx TB
  Free  PE / Size       0 / 0
  VG UUID               xxxxxx-xxxx-xxxx-xxxx-xxxx-xxxx-xxxxxx
```

### LV(Logical Volume、論理ボリューム)作成

100%FREEを指定することで、全VGをLVにあてることができる。

```
# lvcreate -L 100M -n Lg01 VgData01
  Logical volume "LvData01" created
# lvdisplay
  --- Logical volume ---
...
```

### ファイルシステム構築

OSが古かったので今回はext3フォーマットだった

```
# mkfs.ext3 /dev/VgData01/LvData01
mke2fs 1.39 (29-May-2006)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
xxxxxxxx inodes, xxxxxxxx blocks
xxxxxxxx blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=xxxxxxxx
xxxxx block groups
xxxxx blocks per group, xxxxx fragments per group
xxxxx inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968,
        102400000, 214990848, 512000000, 550731776, 644972544

Writing inode tables: done
Creating journal (xxxxx blocks): done
Writing superblocks and filesystem accounting information: done
```

### マウント

```
# mount -t ext3 /dev/VgData01/LvData01 /lvmdata
```

reboot時にマウントされるよう設定(オプション)

```
# e2label /dev/VgData01/LvData01 /lvmdata
# vi /etc/fstab
---
LABEL=/lvmdata            /lvmdata ext3    defaults 1 2
---
```
