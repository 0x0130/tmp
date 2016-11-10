# EC2インスタンスのバックアップ（スナップショット）取得手順

## 以下、手順。

--------------------------------

#### 1. [AWS EC2 Manaagement Console - Instances](https://ap-northeast-1.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#Instances:sort=instanceId)にアクセスする。

#### 2. バックアップを取得するEC2インスタンスの状態が【stopped】であることを確認する。

#### 3. バックアップを取得するEC2インスタンスを右クリック->【イメージ】->【イメージの作成】の順にクリックする。

#### 4. 【イメージ名】に適当な内容を`[a-zA-Z_\-\.]`で入力する。
例：`www.hogehoge.co.jp_backup_20161110`

#### 5. 【イメージの作成】をクリックする。

#### 6. [AWS EC2 Manaagement Console - AMI](https://ap-northeast-1.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#Images:visibility=owned-by-me;sort=creationDate)にアクセスする。

#### 7. 作成したAMIの状態が【pending】から【available】へと変化したことを確認する。

#### 8. [AWS EC2 Manaagement Console - SnapShots](https://ap-northeast-1.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#Snapshots:sort=startTime)にアクセスする。

#### 9. AMI作成時刻のスナップショットが作成されていることを確認する。
例：`Created by CreateImage(i-fffffffffffffffff) for ami-ffffffff from vol-fffffffffffffffff`

--------------------------------

## 以上。
