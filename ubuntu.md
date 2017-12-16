# ubuntu

systemctl status console-setup.service'

## インストール直後にやること

sudo update-alternatives --config editor
sudo visudo                               // 一番最後の業に書かないと有効にならない
sudo apt update
sudo shutdown -r now

## 最初にインストールするパッケージ

vim
build-essentials
software-properties-common

## PPA(Personal Package Archives)とは

Ubuntuの公式レポジトリからインストールできないソフトウェアや最新のバージョンを試したいときに使う

### 最新のgitを手に入れる

sudo add-apt-repository ppa:git-core/ppa
sudo apt update
sudo apt install git

### 最新のtmuxを手に入れる

sudo apt update
sudo apt install -y git
sudo apt install -y automake
sudo apt install -y build-essential
sudo apt install -y pkg-config
sudo apt install -y libevent-dev
sudo apt install -y libncurses5-dev
sudo apt install -y xsel
rm -fr /tmp/tmux
git clone https://github.com/tmux/tmux.git /tmp/tmux
cd /tmp/tmux
git checkout $(git tag | sort -V | tail -n 1)
sh autogen.sh
./configure && make -j4
sudo make install
cd -
rm -fr /tmp/tmux

### tmuxのcompletionの設定

git clone https://github.com/imomaliev/tmux-bash-completion /tmp/tmux-bash-completion
sudo cp /tmp/tmux-bash-completion/completions/tmux /etc/bash_completion.d/
rm -fr /tmp/tmux-bash-completion
