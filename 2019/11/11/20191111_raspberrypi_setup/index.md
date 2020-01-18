# Raspberry Pi用にRaspbianのインストール(Mac)


Raspberry Pi 用の Micro SD カードに Raspbian をインストール手順を時間があくととすぐ忘れてしまうので手順をメモしておく。

### 1. SDカードの選別
基本的に SD カードのサイズは、マイクロSDカードが必要（ただし Raspberry Pi Model A およびRaspberry Pi Model B は、フル SD カード）
NOOBS、Raspbianのイメージインストールは最小推奨で 8GB 以上なので、それ以上のサイズが必要。
カードのスループットに推奨値はないみたいだが、自分は大体 Class 10 以上を使っている。

SD Card  
https://www.raspberrypi.org/documentation/installation/sd-cards.md

### 2. インストール方法の選択 
NOOBS を使う方法と、Raspbian を直接イメージとして焼く方法の２種類がある。
NOOBS でも Raspbian はオフラインインストールできるみたいだが、若干 SD カードの容量を無題にするかなと思われる。
NOOBS で Ubuntuなどの Raspbian 以外の OS をインストールすることもできるがネットワーク経由になる。

Raspberry Pi - Downloads  
https://www.raspberrypi.org/downloads/

### 3. Micro SDカードのフォーマット
PC に SD カードを接続して、ディスクユーティリティを起動する。
Raspberry Pi は、FAT ファイルシステムのみサポートしているので、exFAT などは利用できません。

https://www.raspberrypi.org/documentation/installation/sdxc_formatting.md

![20191111_raspbian](/img/20191111_raspbian/1_diskutility.png)

### 4. Raspbian のダウンロードと書き込み
Raspbian のイメージを次からダウンロードします。デスクトップ版や推奨ソフトウェア込み版などがあるので好みに応じて選びます。

https://www.raspberrypi.org/downloads/raspbian/

ターミナルを起動し、まずはzipを解凍する。

```sh
$ unzip 2019-09-26-raspbian-buster.zip 
Archive:  2019-09-26-raspbian-buster.zip
  inflating: 2019-09-26-raspbian-buster.img 
```

SDカードのデバイス名を確認する。

```sh
$ df -h
/dev/disk2s1    29Gi  1.5Mi   29Gi     1%       0                   0  100%   /Volumes/RASPBIAN

$ diskutil list
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *121.3 GB   disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:                 Apple_APFS Container disk1         121.1 GB   disk0s2

/dev/disk1 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +121.1 GB   disk1
                                 Physical Store disk0s2
   1:                APFS Volume Macintosh HD            92.2 GB    disk1s1
   2:                APFS Volume Preboot                 23.6 MB    disk1s2
   3:                APFS Volume Recovery                519.3 MB   disk1s3
   4:                APFS Volume VM                      3.2 GB     disk1s4

/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *31.0 GB    disk2
   1:                 DOS_FAT_32 RASPBIAN                31.0 GB    disk2s1
```

MACでSDカードを接続すると、自動マウントされてイメージを書き込むタイミングでResource busyで怒られることになるので先にアンマウントしておく。

```sh
sudo diskutil umount /Volumes/RASPBIAN
```

dd コマンドでimgをdisk2のデバイスに書き込む。ここで disk でなく rdisk で指定した方が書き込みが早くなる点に注目。
なにやら 20 倍ほど早くなるとのこと。[Why is “/dev/rdisk” about 20 times faster than “/dev/disk” in Mac OS X](https://superuser.com/questions/631592/why-is-dev-rdisk-about-20-times-faster-than-dev-disk-in-mac-os-x)
イメージ書き込み待ち中暇なので調べたところ、disk と rdisk もデバイスとしては同じものを指定しているが、disk はランダムアクセス可能なデバイス、rdisk はシーケンシャルアクセスされるデバイスを意味しているらしい。
disk 指定の場合、 IO は 4KB に分割され、カーネル空間の Buffer Cache を経由してデバイスへ Read/Write されることになる。一方、rdiskはk基本的に Buffer Cache を経由せずに直接デバイスへIOとなる。
なので、dd コマンドでの書き込みのようなシーケンシャルアクセスでは、余計なオーバヘッドが発生しない rdisk 指定のアクセスが圧倒的に早いという話。

```sh
$ sudo dd bs=1m if=2019-09-26-raspbian-buster.img of=/dev/rdisk2
3652+0 records in
3652+0 records out
3829399552 bytes transferred in 274.894541 secs (13930431 bytes/sec)
```