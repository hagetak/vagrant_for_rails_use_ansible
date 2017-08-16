## 概要

プロビジョンツール ansible を用いて、 `vagrant` で Rails 環境を整える.

ミドルウェア等のバージョンは下記のとおりである。

yum のインストールを`latest`に指定しているため、バージョンアップした場合はこの情報と異なる可能性がある。

| midleware | version          | 
|-----------|------------------| 
| mysql     | 5.6.37           | 
| ruby      | 2.4.1p111        | 
| rbenv     | 1.1.1-2-g615f844 | 
| Rails     | 5.1.3            | 
| node      | v6.11.2          | 
| npm       | 3.10.10          | 


## 使い方

```
$ cd ~/src/ 
$ git clone vagrant_use_ansible
$ cd vagrant_use_ansible

$ vagrant plugin install vagrant-vbguest
$ vagrant ssh-config --host=127.0.0.1 >> ~/.ssh/config # host を ansible に利用するため指定 ※ 注1
$ vagrant up
```

`--host={hostname}` は、 `provision/hosts` と同様であればOK。

#### ※1回目は正常終了しないため、re-provision する

上記の `vagrant up` 時、下記のエラーが発生すると思われる。

```
The guest machine entered an invalid state while waiting for it
to boot. Valid states are 'starting, running'. The machine is in the
'paused' state. Please verify everything is configured
properly and try again.

If the provider you're using has a GUI that comes with it,
it is often helpful to open that and watch the machine, since the
GUI often has more helpful error messages than Vagrant can retrieve.
For example, if you're using VirtualBox, run `vagrant up` while the
VirtualBox GUI is open.

The primary issue for this error is that the provider you're using
is not properly configured. This is very rarely a Vagrant issue.
```

上記のエラーが出た場合は、`vagrant ssh`で仮想環境に入り、以下のコマンドを実行する

```
ref: http://qiita.com/wakaba260/items/b5c87b7815b710f303a0
$ vagrant ssh
$ sudo yum -y update kernel
$ sudo yum -y install kernel-devel kernel-headers dkms gcc gcc-c++

$ vagrant reload --provision
...(省略)
PLAY RECAP *********************************************************************
127.0.0.1                  : ok=26   changed=21   unreachable=0    failed=0
```



## 参考サイト

* http://qiita.com/katsuhiko/items/56935225754a90d58314

* http://qiita.com/KisaragiZin/items/e241f189c83580f92e9c
