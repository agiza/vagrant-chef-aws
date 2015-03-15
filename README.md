Vagarnt と Chef による仮想環境構築の自動化

# 環境

* ホストOS: Mac OS X 10.10.2
* Vagrant 1.6.5
* Chef 0.3.5
 
# 準備

AWSアカウントの作成（割愛）

## vagrantユーザの作成

ホストOS上のVagrant からゲストOS（仮想環境）を立ち上げる際のユーザを作成する。
ユーザのポリシーには PowerUserAccess とする。

1. Services -> Administration & Security -> IAM を選択
2. 左のメニューの Users -> Create New Users -> ユーザ名を入力（例. vagrant）-> Create （ユーザが作成される）
3. 左のメニューの Groups -> Create New Group -> グループ名を入力 -> Attach Policy で「PowerUserAcess」を選択 -> 次の画面で Create Group

## Security Group の作成

1. Services -> Computing -> EC2
2. 左側メニューの NETWORK SECURITY で Security Group を選択
3. Create Security Group -> Security Group Name と Description を入力
4. Inboud のルールに以下を追加

|Type|Protocl|Port|Source  |
|:---|:------|:---|:-------|
|SSH |TCP    |22  |Anywhere|


## Security Group の作成

 


