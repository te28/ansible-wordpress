# Ansibleによるwordpressのローカル環境構築

## Ansible

python製のサーバーの構成管理ツール

<http://www.ansible.com/home>

日本語のチュートリアル

<http://yteraoka.github.io/ansible-tutorial/>

## wp-cli

wordpressのコマンドラインインターフェース

<http://wp-cli.org/>

## 構成ファイル

* Inventry:どのサーバーを管理するかを記述する(/.local)
* ansible.cfg:ansibleの設定を記述(./ansible.cfg)
* Playbook:どういった構成を作るかを記載(./xxxx.yaml)

```
.
├── README.md // 当ファイル
├── Vagrantfile // vagrantのプログラム
├── ansible // ansibleのタスク群
│   ├── ansible.cfg // ansible全般の設定を記述
│   ├── host_vars
│   ├── hosts // インベントリファイル どのサーバーを管理するのか記述
│   ├── roles
│   │   ├── common // 環境構築に必要なパッケージを設定
│   │   │   ├── defaults
│   │   │   ├── files
│   │   │   ├── handlers
│   │   │   │   └── main.yml
│   │   │   ├── meta
│   │   │   ├── tasks
│   │   │   │   ├── ansible.yml
│   │   │   │   ├── main.yml
│   │   │   │   ├── utility.yml
│   │   │   │   └── yum_repos.yml
│   │   │   ├── templates
│   │   │   └── vars
│   │   ├── httpd // apacheの設定
│   │   │   ├── defaults
│   │   │   ├── files
│   │   │   ├── handlers
│   │   │   │   └── main.yml
│   │   │   ├── meta
│   │   │   ├── tasks
│   │   │   │   ├── apache.yml
│   │   │   │   └── main.yml
│   │   │   ├── templates
│   │   │   │   └── vhost.conf.j2
│   │   │   └── vars
│   │   ├── mysql // mysqlの設定
│   │   │   ├── defaults
│   │   │   ├── files
│   │   │   ├── handlers
│   │   │   ├── meta
│   │   │   ├── tasks
│   │   │   │   ├── main.yml
│   │   │   │   └── mysql.yml
│   │   │   ├── templates
│   │   │   └── vars
│   │   ├── php // phpの設定
│   │   │   ├── defaults
│   │   │   ├── files
│   │   │   ├── handlers
│   │   │   │   └── main.yml
│   │   │   ├── meta
│   │   │   ├── tasks
│   │   │   │   ├── main.yml
│   │   │   │   └── php.yml
│   │   │   ├── templates
│   │   │   └── vars
│   │   ├── ruby // rubyの設定
│   │   │   ├── defaults
│   │   │   ├── files
│   │   │   │   └── rbenv.sh
│   │   │   ├── handlers
│   │   │   │   └── main.yml
│   │   │   ├── meta
│   │   │   ├── tasks
│   │   │   │   ├── main.yml
│   │   │   │   └── ruby.yml
│   │   │   ├── templates
│   │   │   └── vars
│   │   └── wordpress // wordpressの設定
│   │       ├── defaults
│   │       ├── files
│   │       ├── handlers
│   │       ├── tasks
│   │       │   ├── config.yml
│   │       │   ├── copy_and_edit_index_php.yml
│   │       │   ├── create_database.yml
│   │       │   ├── download.yml
│   │       │   ├── install.yml
│   │       │   ├── main.yml
│   │       │   ├── update.yml
│   │       │   ├── wordmove.yml
│   │       │   └── wpcli.yml
│   │       ├── templates
│   │       │   ├── movefile.j2
│   │       │   └── wp-config.php
│   │       └── vars
│   │           └── main.yml
│   └── site.yml
└── html // 仮想環境のドキュメントルート以下が同期されます。
```

## 環境

* python： 2.7.9
* ansible： 1.8.4

## 導入方法

`homebrew`や`pip`で`ansible`をインストールして下さい。

macであればpythonはデフォルトでインストールされているかと思います。
必要であればpyenv等を使用してインストールして下さい。

### homebrewからインストール

```
$ brew install ansible
```

### pipからインストール

```
$ pip install ansible
```

## 使用方法

### リポジトリからテンプレートを取得

任意のディレクトリに移動して、このテンプレートをリポジトリから取得してください。

```
$ cd ~/hoge
$ git clone https://github.com/te28/ansible-wordpress
```

### 設定ファイルの編集

`Vagrantfile`を開き、下記の`ip:`を任意の値に編集してください。

```
config.vm.network "private_network", ip: "192.168.33.51"
```

`hosts`を開き、上記で設定した`ipアドレス`を入力してください。

```
[local]
192.168.33.51 ansible_ssh_user=vagrant
```

### vagrantの起動

以下コマンドを実行してください。

```
$ vagrant up
```

### 設定を変更し、適用する場合

```
$ vagrant provision
```
