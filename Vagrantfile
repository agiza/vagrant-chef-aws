# -*- mode: ruby -*-
# vi: set ft=ruby :

# dotenvプラグインで設定を環境変数に読み込み
Dotenv.load

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "dummy"
  config.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
  # Chef Zero を起動する
  config.chef_zero.chef_repo_path = "."
  # ゲストにChef Clientをインストールする
  config.omnibus.chef_version = :latest

  # AWSプロバイダ設定
  config.vm.provider :aws do |provider, override|
    # AWS認証情報
    provider.access_key_id          = ENV['AWS_ACCESS_KEY_ID']
    provider.secret_access_key      = ENV['AWS_SECRET_ACCESS_KEY']
    provider.keypair_name           = ENV['AWS_KEYPAIR_NAME']

    # EC2インスタンスの設定
    provider.region                 = "ap-northeast-1"
    provider.availability_zone      = "ap-northeast-1a"

    # Amazon LinuxのAMIを指定
    provider.ami                    = "ami-18869819"

    provider.instance_type          = "t2.micro"
    provider.instance_ready_timeout = 120
    provider.terminate_on_shutdown  = false

    # セキュリティグループ
    provider.security_groups        = [ ENV['AWS_SECURITY_GROUP'] ]
    # タグ（任意）。インスタンスに分かりやすい目印を付けたい場合。
    provider.tags                   = {
      "Name"        => "vagrant-aws",
      "Description" => "Boot from vagrant-aws",
    }

    # ログイン後、sudo できるようにする
    provider.user_data = <<-USER_DATA
    #!/bin/sh
    sed -i -e 's/^\\(Defaults.*requiretty\\)/#\\1/' /etc/sudoers
    USER_DATA

    # AWSへの接続ユーザおよび秘密鍵の指定
    override.ssh.username = ENV['AWS_SSH_USERNAME']
    override.ssh.private_key_path   = ENV['AWS_SSH_KEY']
    override.ssh.pty = true

    override.vm.synced_folder ".", "/home/ec2-user/vagrant", disabled: true
  end

    # Chef Client によるプロビジョンに設定
  config.vm.provision :chef_client do |chef|
    chef.custom_config_path = "chef_custom_config"
    chef.run_list = [
        "postgresql::server",
        "postgresql::client",
        "postgresql::contrib",
        "database::postgresql",
        "postgresql_config",
        "java",
        "play-sample"
    ]
    chef.json = {
      :postgresql => {
        :password => 'postgres'
      },
      :java => {
        :jdk_version => '7'
      }
    }
  end
end
