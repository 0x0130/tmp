# AWS/EC2/CentOS6 EBSが8GBしか認識しない（[参考](http://ysh.hateblo.jp/entry/2016/04/08/011349)）

## さまりー
- 本問題の原因は、cloud-init が正常に動作していないこと。
- cloud-utils-growpartをインストール、手動でgrowpartを実行し、問題を解決する。

#### 『ファイルシステムが認識しているボリュームサイズ(8GB)』を確認する。

    df -hP

#### 『parted』をインストールする。

    yum -y install parted

#### 『実際のボリュームサイズ(100GB)』と『パーティションサイズ(8GB)』を確認する。

    parted -l

#### 『cloud-init』要件として、『epel』リポジトリをインストール（デフォルトで無効）する。

    yum -y install https://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
    yum -y install yum-utils
    yum-config-manager --disable epel*

#### 『cloud-init』および『growpart』コマンドをインストールする。

    yum --enablerepo=epel -y install cloud-init
    yum --enablerepo=epel -y install cloud-utils-growpart

#### 『growpart』コマンド要件として、ロケールが『en_US.UTF-8』でない場合は変更する。

    locale

    # 既にロケールが en_US.UTF-8 の場合、本手順はスキップしてOK。
    export LANG="en_US.UTF-8"
    locale

#### 『growpart』コマンドを実行し、ボリュームを正常に認識させる。

    growpart /dev/xvda 1

    #| 『growpart ディスク 番号』
    #| # parted -l
    #| Model: Xen Virtual Block Device (xvd)
    #| Disk /dev/xvda: 107GB
    #|     ~~~~~~~~~~~ <= ディスク
    #| Sector size (logical/physical): 512B/512B
    #| Partition Table: msdos
    #| 
    #| Number  Start   End    Size   Type     File system  Flags
    #|  1      1049kB  107GB  107GB  primary  ext4         boot
    #| ~~~ <= 番号

#### 『パーティションサイズ(100GB)』と『実際のボリュームサイズ(100GB)』が等しいことを確認する。

    parted -l

#### 『ファイルシステムが認識しているボリュームサイズ(8GB)』を確認する。

    df -hP

#### 再起動を実施。

    reboot
