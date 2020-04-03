## Laravelを入れてみる
### 用意したもの
- 環境
  - CentOS(on Vagrant)
  - PHP 7.2
- 入れたもの
  - Composer(必須)
  - Laravel 7.4.0
  - unzip,zip

### Composer を入れる
下記コマンドでインストール
```bash
# Composerのセットアップモジュールを取得
$ php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
# セットアップモジュールを実行
$ php composer-setup.php
# OKならセットアップモジュールを削除
$ php -r "unlink('composer-setup.php');"
# セットアップ時に生成されたcomposer.pharを移動＆リネーム
$ mv composer.phar /usr/local/bin/composer
# 動作確認
$ composer
```

### unzipとzipモジュールをインストール
**※これがないとLaravelのインストールでエラーが出る**  
下記コマンドでインストール
```bash
$ yum install unzip
$ yum install zip
```

### Laravelのインストール
リファレンスにある通り、下記コマンドを実行
```bash
$ composer global require laravel/installer
```
※ここでしくった場合、何かが足りないのでエラーメッセージを確認  

次の手順で必要な `laravel` コマンドを使えるように、パスを通す。
```bash
$ vi ~/.bash_profile
```
↓を追記
```
export PATH=~/.composer/vendor/bin:$PATH
```
反映
```bash
$ source ~/.bash_profile
```

任意(※Httpサーバーのドキュメントルート配下)の場所で下記コマンドを実行で、Laravelのプロジェクトを生成
```bash
$ laravel new <project name>
```
いる場所をプロジェクトルートにしたい場合
```bash
$ laravel new .
```
