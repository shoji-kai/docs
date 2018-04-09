Raspberry Pi
============

## インストール

1. SDカードのdisk#を調べる
```
% diskutil list
```

2. FAT32でフォーマットする (diskutil eraseDisk <format> <name> [APM[Format]|MBR[Format]|BPG[Format]] <device>)
```
% diskutil eraseDisk FAT32 RASPBIAN MBRFormat /dev/disk2
  ```

3. ミラーサイトより最新のOSイメージをダウンロード
```
% curl -LO 'http://ftp.jaist.ac.jp/pub/raspberrypi/raspbian/images/raspbian-2017-09-08/2017-09-07-raspbian-stretch.zip'
% unzip raspbian-2017-09-08/2017-09-07-raspbian-stretch.zip
```

4. ddコマンドでSDカードにOSイメージを書き込み
```
% diskutil unmountDisk /dev/disk2
% sudo dd if=2017-09-07-raspbian-stretch.img of=/dev/rdisk2 bs=1m
```

5. Raspberry PiにSSH接続できるように/boot直下にsshという名前の空ファイルをtouchする
```
% touch /Volumes/boot/ssh
```

6. SDカードをアンマウントする
```
% diskutil unmountDisk /dev/disk2
```

## セットアップ

1. Raspberry PiにSDカードを差し込み、LANケーブルをつなぐ

2. Raspberry PiのIPアドレスを調べる
```
% for i in {1..254}; do echo "172.254.99.$i"; done | parallel -j254 ping -c1 -t1 {}
% arp -an | grep b8:27:eb
? (172.254.99.43) at b8:27:eb:**:**:** on en0 ifscope [ethernet]
```

3. Raspberry PiにSSHする
```
% ssh pi@172.254.99.43  # 初期パスワードはraspberry
```

4. apt-get update && upgradeする
```
pi@raspberrypi:~ $ sudo apt-get update
pi@raspberrypi:~ $ sudo apt-get -y upgrade
```

apt-utilsのインストールでエラーになっていたので、apt --fix-broken installした
```
pi@raspberrypi:~ $ sudo apt-get -y upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
You might want to run 'apt --fix-broken install' to correct these.
The following packages have unmet dependencies:
 apt-utils : Depends: apt (= 1.4.7) but 1.4.8 is installed
E: Unmet dependencies. Try 'apt --fix-broken install' with no packages (or specify a solution).
```

日本語に必要なパッケージ
```
sudo apt install fonts-vlgothic
```

各種設定 (Wi-Fi, Locale, Timezone, VNC, SPI)
```
sudo raspi-config
```

