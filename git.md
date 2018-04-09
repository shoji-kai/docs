# Git

## git config

```
% git config --global user.name "<username>"
% git config --global user.email "<email>"
```

## サブモジュール

```
% git clone https://github.com/hyakusho/packer.git
% git submodule add https://github.com/boxcutter/ubuntu ubuntu
```

## clone先のコミットに追従する

```
% git clone https://github.com/hyakusho/packer-centos.git
% git remote add upstream https://github.com/boxcutter/centos 
% git fetch upstream
% git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/parallels12_bash
  remotes/upstream/master
  remotes/upstream/parallels12_bash
% git fetch upstream
% git merge upstream/master
```
