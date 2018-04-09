# soracom

## Raspberry PiでSORACOMを使う

```
% sudo apt install -y usb-modeswitch wvdial
% curl -O http://soracom-files.s3.amazonaws.com/connect_air.sh
% chmod 755 connect_air.sh
% sudo mv connect_air.sh /usr/local/sbin/
```

```
% sudo /usr/local/sbin/connect_air.sh
```

### 踏み台経由で外からRasberry PiへSSH接続できるようにする

1. 踏み台サーバ(shojikai.com)の8022番ポートをラズベリーパイ(localhost)の22番ポートへポートフォワードする

```
// rasberry piから実行
% ssh shoji@shojikai.com -R 8022:localhost:22 -N -f
```

2. sshするローカルの.ssh/configには以下のように書いておくと便利

```
% vim ~/.ssh/config
Host shojikai
  Hostname shojikai.com
  Port 22
  User shoji
  ForwardAgent yes

Host pi
  Hostname localhost
  Port 8022
  User pi
  ForwardAgent yes
  ProxyCommand ssh -W %h:%p shojikai
```
