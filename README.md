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
├── README.md
├── Vagrantfile
└── ansible
    ├── ansible.cfg // ansible全般の設定を記述
    ├── common.yml // roles/commonを呼び出すplaybook
    ├── site.yml // common.yml,wordpress.ymlを呼び出すplaybook
    ├── wordpress.yml // roles/wordpressを呼び出すplaybook
    ├── group_vars
    │   └── all.yml // playbook内で使用する変数を記載
    ├── host_vars
    ├── local // インベントリファイル どのサーバーを管理するのか記述
    └── roles
    │   ├── common // LAMP環境の構築
    │   │   ├── defaults
    │   │   ├── files
    │   │   │   └── rbenv.sh
    │   │   ├── handlers
    │   │   │   └── main.yml
    │   │   ├── meta
    │   │   ├── tasks
    │   │   │   ├── ansible.yml
    │   │   │   ├── apache.yml
    │   │   │   ├── main.yml
    │   │   │   ├── mysql.yml
    │   │   │   ├── php.yml
    │   │   │   ├── ruby.yml
    │   │   │   └── utility.yml
    │   │   ├── templates
    │   │   └── vars
    │   └── wordpress // wordpress環境の構築
    │       ├── defaults
    │       ├── files
    │       ├── handlers
    │       ├── meta
    │       ├── tasks
    │       │   ├── main.yml
    │       │   ├── wordpress.yml
    │       │   └── wpcli.yml
    │       ├── templates
    │       └── vars
    │           └── main.yml
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
$ git clone https://192.168.1.201/git/house-projects.wordpress-ansible
```

### 設定ファイルの編集

`Vagrantfile`を開き、下記の`ip:`を任意の値に編集してください。

```
config.vm.network "private_network", ip: "192.168.33.51"
```

`local`を開き、上記で設定した`ipアドレス`を入力してください。

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
