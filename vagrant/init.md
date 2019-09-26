## Vagrant で仮想環境を作る

### Vagrantってな～に？
- 仮想マシンを簡単に構築、同一環境をどこでも再現できるように管理できるコマンドラインツール
- 複数人での開発案件で、開発環境構築にかかる工数をある程度短縮できる

### 必要なもの
- Vagrant本体
  - https://www.vagrantup.com/downloads.html
- VirtualBox
  - https://www.virtualbox.org/wiki/Downloads
- 仮想環境のOSイメージ(box)
- 折れない心

### 手順
1. VagrantとVirtualBoxをインストール
    - さくさく進めるだけでOK
    - インストール後、再起動を求められるので逆らわずに再起動
    - その後、cmdでもって、下記コマンド叩いてバージョンが返ってくればOK
        ```
        $ vagrant -v
        Vagrant 2.0.0
        ```
1. Boxを見つける
    - https://app.vagrantup.com/boxes/search にて。
    - 「bento」と名のつくboxは信頼してよいらしい
1. Vagrantfileを作成＆修正
    - vagrant用、仮想マシン用のディレクトリを作ると増えたときに管理しやすくなる
    - 仮想マシン用のディレクトリで下記コマンドを実行
        ```
        $ vagrant init hoge/hoge    # box提供ユーザー名/box名
        ```
    - しばらく待つとコマンド実行したディレクトリに「Vagrantfile」が生成されるので、  
    下記のコメントアウトを外して保存
        ```
        config.vm.network "private_network", ip: "192.168.33.10"
        
        ```
1. 起動
    - 下記コマンドを実行して仮想マシンを起動
        ```
        $ vagrant up
        ```
    - 起動できたら下記コマンドを実行して仮想マシンにログインして完了！
        ```
        $ vagrant ssh
        ```
