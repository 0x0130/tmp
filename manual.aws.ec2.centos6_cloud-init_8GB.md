# AWS/EC2/CentOS6 EBSが8GBしか認識しない（[参考](http://ysh.hateblo.jp/entry/2016/04/08/011349)）

## Summary
- 本事象は、AWSにおいて「AMI【CentOS Linux 6 x86_64 HVM EBS 1602】からEC2インスタンスを作成した場合、EBSを8GBまでしか認識しない」というものである。
- 原因は、【cloud-init】パッケージのボリューム管理の機能が正常に動作していないことにある。
- 解決策として、【cloud-utils-growpart】をインストールし、ルートボリュームに対し手動で【growpart】コマンドを実行する。

## Prerequisite
- 【root】ユーザであること。
- 再起動が必要になるため、作業実施の周知が事前に行われていること。
- 事前にバックアップを取得していること。

## 以下、手順。

--------------------------------

####  0. 事前準備（時刻・ホスト名・ユーザ確認）

    date;uname -n;whoami

####  1. 『ファイルシステムが認識しているボリュームサイズ(8GB)』を確認する。

    df -hP

####  2. 【parted】をインストールする。

    yum -y install parted

####  3. 『実際のボリュームサイズ(100GB)』と『パーティションサイズ(8GB)』が異なることを確認する。

    parted -l

####  4. 【cloud-init】要件として、【epel】リポジトリをインストール（デフォルトで無効）する。

    yum -y install yum-utils
    yum -y install https://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
    yum-config-manager --disable epel*

####  5. 【cloud-init】および【growpart】コマンドをインストールする。

    yum --enablerepo=epel -y install cloud-init
    yum --enablerepo=epel -y install cloud-utils-growpart

####  6. 【growpart】コマンド要件として、ロケールを【en_US.UTF-8】に変更する。

    locale

    # 既にロケールが en_US.UTF-8 の場合、以下2行はスキップしてOK。
    export LANG="en_US.UTF-8"
    locale

####  7. 【growpart】コマンドを実行し、ボリュームを正常に認識させる。

    growpart /dev/xvda 1

    #| **** 『growpart ディスク 番号』について ****
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

####  8. 『実際のボリュームサイズ(100GB)』と『パーティションサイズ(100GB)』が等しいことを確認する。

    parted -l

####  9. 『ファイルシステムが認識しているボリュームサイズ(8GB)』を確認する。

    df -hP

#### 10. 再起動を実施。

    reboot

#### 11. 『ファイルシステムが認識しているボリュームサイズ(100GB)』を確認する。

    df -hP

--------------------------------

## 以上。
