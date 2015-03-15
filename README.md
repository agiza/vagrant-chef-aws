Vagarnt と Chef による仮想環境構築の自動化

# 環境

* ホストOS: Mac OS X 10.10.2
* Vagrant 1.6.5
* Chef 0.3.5
 
# 準備

le.chefにSSLの設定を記載AWSアカウントの作成（割愛）

## vagrantユーザの作成

ホストOS上のVagrant からゲストOS（仮想環境）を立ち上げる際のユーザを作成する。
ユーザのポリシーには PowerUserAccess とする。

1. Services > Administration & Security > IAM を選択
2. 左のメニューの Users 
3. Create New Users 
4. ユーザ名を入力（例. vagrant）
5. Create をクリック（ユーザが作成される）
6. 左のメニューの Groups
7. Create New Group
8. グループ名を入力
9. Attach Policy で「PowerUserAcess」を選択
10. Create Group

## Security Group の作成

以下の手順で Security Group を作成する。

1. Services > Computing > EC2
2. 左側メニューの NETWORK SECURITY で Security Group を選択
3. Create Security Group
4. Security Group Name と Description を入力
5. Inboud のルールに以下を追加してCreateをクリック

|Type |Protocl|Port|Source  |
|:----|:------|:---|:-------|
|SSH  |TCP    |22  |Anywhere|
|HTTP |RCP    |80  |Anywhere|
|HTTPS|TCP    |443 |Anywhere|

## キーペアを作成

1. Service > Compute & Networking > EC2に移動
2. NETWORK & SECURITY > Key Pairsに移動
3. Create Key Pair
4. キーペア名を入力してCreateをクリック
5. 秘密鍵をダウンロード
6. ~/.ssh に秘密鍵を格納

## Vagrant と Chef のインストール

前回の記事を参照。

## Vagrant プラグインのインストール

* vagrant-aws は AWS プロバイダを利用するためのプラグイン。
* vagrant-omnibusはゲストOSにChefをインストールするためのプラグイン。
* dotenv はカレントディレクトリの.envファイルに値の宣言をすることで、環境変数として読み込むことができるGem。アクセスキーなどをVagrantfileに直接書かずに外部ファイル化できる。

```
$ vagrant plugin install vagrant-aws
$ vagrant plugin install vagrant-omnibus
$ vagrant plugin install dotenv
```

## ~/vagrant-aws/.envにAWSの認証情報を記載

~/vagrant-aws/.env

```
# providerのデフォルトをAWSに変更
VAGRANT_DEFAULT_PROVIDER="aws"

# AWSの認証情報
AWS_SSH_USERNAME="ec2-user"
AWS_SSH_KEY="~/.ssh/vagrant.pem"
AWS_ACCESS_KEY_ID="＜ユーザ作成時のAccess Key Id＞"
AWS_SECRET_ACCESS_KEY="＜ユーザ作成時のSecret Access Key＞"
AWS_KEYPAIR_NAME="vagrant"
AWS_SECURITY_GROUP="vagrant"
```

## Vagrantfile.chefにSSLの設定を記載i



