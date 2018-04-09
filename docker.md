# Docker

## dockerコマンド

### バージョンの確認 (version)

```
% docker version
Client:
 Version:      17.09.1-ce
 API version:  1.32
 Go version:   go1.8.3
 Git commit:   19e2cf6
 Built:        Thu Dec  7 22:22:25 2017
 OS/Arch:      darwin/amd64

Server:
 Version:      17.12.0-ce
 API version:  1.35 (minimum version 1.12)
 Go version:   go1.9.2
 Git commit:   c97c6d6
 Built:        Wed Dec 27 20:12:29 2017
 OS/Arch:      linux/amd64
 Experimental: false
```

### 現在の情報 (info)

```
% docker info
Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 0
Server Version: 17.09.1-ce
Storage Driver: overlay2
 Backing Filesystem: extfs
 Supports d_type: true
 Native Overlay Diff: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins:
 Volume: local
 Network: bridge host ipvlan macvlan null overlay
 Log: awslogs fluentd gcplogs gelf journald json-file logentries splunk syslog
Swarm: inactive
Runtimes: runc
Default Runtime: runc
Init Binary: docker-init
containerd version: 06b9cb35161009dcb7123345749fef02f7cea8e0
runc version: 3f2f8b84a77f73d38244dd690525642a72156c64
init version: 949e6fa
Security Options:
 seccomp
  Profile: default
Kernel Version: 4.9.49-moby
Operating System: Alpine Linux v3.5
OSType: linux
Architecture: x86_64
CPUs: 2
Total Memory: 1.952GiB
Name: moby
ID: CJVX:HO5I:ZCVL:227L:ACI2:QWPV:K7SZ:P4XQ:GLIR:7I5X:64MT:WEYM
Docker Root Dir: /var/lib/docker
Debug Mode (client): false
Debug Mode (server): true
 File Descriptors: 18
 Goroutines: 29
 System Time: 2018-01-03T05:30:04.3728938Z
 EventsListeners: 1
No Proxy: \*.local, 169.254/16
Registry: https://index.docker.io/v1/
Experimental: true
Insecure Registries:
 127.0.0.0/8
Live Restore Enabled: false
```

### イメージの取得 (pull)

```
% docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
Digest: sha256:ec0e4e8bf2c1178e025099eed57c566959bb408c6b478c284c1683bc4298b683
Status: Image is up to date for ubuntu:latest
```

### インタラクティブ実行

```
% docker run -i -t -h ubuntu ubuntu /bin/bash
root@ubuntu:/#
```

### 長時間実行するアプリケーションの実行

1. 10秒間隔で"Hello world"を表示する処理をバックグラウンドで実行

```
% docker run -d ubuntu /bin/sh -c "while true; do echo Hello world; sleep 10; done"
f47ff9bcba08b9db01f172d91059a5cf5a72a730e7e77b2b585aaada0e6d9d79
```

2. docker psで動作しているコンテナを確認

```
% docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
f47ff9bcba08        ubuntu              "/bin/sh -c 'while..."   13 seconds ago      Up 18 seconds                           ecstatic_dijkstra
```

3. logsコマンドで標準出力を確認

```
% docker logs -f f47ff9bcba08
Hello world
Hello world
Hello world
```

### コンテナの停止 (stop)

```
% docker stop f47ff9bcba08
f47ff9bcba08
```

### コンテナの開始/再開 (start/restart)

```
% docker restart f47ff9bcba08
f47ff9bcba08
```

### コンテナへのアタッチ (attach)

```
% docker attach $(docker ps -q)
Hello world
Hello world
Hello world
^C
```

### コンテナの強制終了 (kill)

```
% docker kill $(docker ps -q)
f47ff9bcba08
```

### コンテナ内プロセスの確認 (top)

```
% docker top $(docker ps -q)
PID                 USER                TIME                COMMAND
7746                root                0:00                /bin/sh -c while true; do echo Hello world; sleep 10; done
7769                root                0:00                sleep 10
```

### コンテナの詳細情報の確認 (inspect)

```
% docker inspect $(docker ps -q)
[
    {
        "Id": "f47ff9bcba08b9db01f172d91059a5cf5a72a730e7e77b2b585aaada0e6d9d79",
        "Created": "2018-01-03T05:47:23.4697447Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while true; do echo Hello world; sleep 10; done"
        ],
        :
```

### コンテナのコミット (commit)

```
% docker commit $(docker ps -q)
sha256:929ba250f43c261da40e9e96b4a0c1daba48506d1262a5d5dad073c8f8bb5fde
```

### コンテナの削除、イメージの削除 (rm, rmi)

コンテナの削除

```
% docker rm $(docker ps -aq)
f47ff9bcba08
c2fdc475c346
```

一括削除

```
// -v: 他から参照されていないボリュームも削除
% docker rm -v $(docker ps -aq -f status=enabled)
```

イメージの削除

```
% docker rmi -f $(docker images -q)
Untagged: ubuntu:16.04
Untagged: ubuntu:latest
Untagged: ubuntu@sha256:ec0e4e8bf2c1178e025099eed57c566959bb408c6b478c284c1683bc4298b683
Deleted: sha256:00fd29ccc6f167fa991580690a00e844664cb2381c74cd14d539e36ca014f043
Deleted: sha256:009b74a968aa80a4a557e710284529fb026bc0060db89914a29af323448ab897
Deleted: sha256:5c65a70eb44dc2088918c880a263580464724253d3b04327eb2b750368d43ad4
Deleted: sha256:f147f1ff4f36c57105ce957f04d7628005aa739474377e730e91a324cefb21a1
Deleted: sha256:5cdc1917cf08505e804bd1ad2c8f809f69fc8fad4edc33b2df27a0d70cb732e5
Deleted: sha256:48e0baf45d4dfd028eab0a7d444309b436cac01ae879805ac0cfdf5aaf1ff93a
```

### 実行中のファイルシステムに対して行われた変更を表示 (diff)

e.g. mv /bin /basketを実行した場合

```
% docker diff 25ea099b5604
A /basket
A /basket/bash
A /basket/cat
:
D /bin
```

### docker serverにログインする (Docker for Mac)

```
% screen ~/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/tty    # C-a + kで抜ける
```
