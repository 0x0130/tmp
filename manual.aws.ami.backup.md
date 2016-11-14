# EC2インスタンスバックアップ（AMI/スナップショット作成）手順

## Summary
- AWS/EC2インスタンスのバックアップ（AMI/スナップショット作成取得）手順である。

## Prerequisite
- 【[AWS Management Console](https://ap-northeast-1.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#Instances:sort=instanceId)】にログインできること。
- AMI作成対象のEC2インスタンスの電源が落ちている（状態が【stopped】である）こと。

## 以下、手順。

--------------------------------

#### 1. 【[AWS EC2 Manaagement Console - Instances](https://ap-northeast-1.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#Instances:sort=instanceId)】にアクセスする。

#### 2. AMIを作成するEC2インスタンスの状態が【stopped】であることを確認する。

#### 3. AMIを作成するEC2インスタンスを右クリック->【イメージ】->【イメージの作成】の順にクリックする。

#### 4. 【イメージ名】に適当な内容を`[a-zA-Z_\-\.]`で入力する。
例：`www.hogehoge.com_backup_20161110`

#### 5. 【イメージの作成】をクリックし、AMIを作成する。（また、時刻を控えておく。）

#### 6. 【[AWS EC2 Manaagement Console - AMI](https://ap-northeast-1.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#Images:visibility=owned-by-me;sort=creationDate)】にアクセスする。

#### 7. 数分待機し、作成したAMIの状態が【pending】から【available】へと変化することを確認する。

#### 8. 【[AWS EC2 Manaagement Console - SnapShots](https://ap-northeast-1.console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#Snapshots:sort=startTime)】にアクセスする。

#### 9. AMI作成時刻のスナップショットが作成されていることを確認する。
例：`Created by CreateImage(i-fffffffffffffffff) for ami-ffffffff from vol-fffffffffffffffff`

--------------------------------

## 以上。
