# packer

## インストール

```
% brew install packer
```

## Vagrant Boxの作成

### CentOS

```
% git clone https://hyakusho@github.com/hyakusho/packer-centos
% cd packer-centos
% mkdir iso
% cd iso
% curl -LO 'http://ftp.jaist.ac.jp/pub/Linux/CentOS/7/isos/x86_64/CentOS-7-x86_64-DVD-1708.iso'
% cd ..
% packer build -only=virtualbox-iso -var-file=centos7.json -var "iso_path=$(pwd)/iso" -var "update=true" centos.json
% vagrant box add centos7-metadata.json
```
### Ubuntu

```
% git clone https://hyakusho@github.com/hyakusho/packer-ubuntu
% cd packer-ubuntu
% mkdir iso
% cd iso
% curl -LO 'http://ftp.jaist.ac.jp/pub/Linux/ubuntu-releases/16.04/ubuntu-16.04.3-server-amd64.iso'
% cd ..
% packer build -only=virtualbox-iso -var-file=ubuntu1604.json -var "iso_path=$(pwd)/iso" ubuntu.json
% vagrant box add ubuntu1604-metadata.json
```

