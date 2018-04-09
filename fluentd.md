# fluentd

## インストール (td-agent)

### 前準備

1. ntpの設定

2. ファイルディスクリプタを最大にする
```
% sudo vim /etc/security/limits.conf
:
root soft nofile 65536
root hard nofile 65536
* soft nofile 65536
* hard nofile 65536
% sudo shutdown -r now
% ulimit -n
65536
```

3. ネットワークのカーネルパラメータのチューニング
```
% sudo vim /etc/sysctl.conf
:
net.core.somaxconn = 1024
net.core.netdev_max_backlog = 5000
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.ipv4.tcp_wmem = 4096 12582912 16777216
net.ipv4.tcp_rmem = 4096 12582912 16777216
net.ipv4.tcp_max_syn_backlog = 8096
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_tw_reuse = 1
net.ipv4.ip_local_port_range = 10240 65535
% sudo sysctl -p
```

### インストール

```
curl -L https://toolbelt.treasuredata.com/sh/install-ubuntu-xenial-td-agent3.sh | sh
```
