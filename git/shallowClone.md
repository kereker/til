### Shallow Cloneって何さ
- 最新のリポジトリ情報 **だけ** を取る方法。
- 最新のコミット履歴しか取りに行かないので、clone速度がとても速い。
  - プラグインやらなんやらのcloneに最適。
- 以前通常cloneしたリポジトリもshallow cloneにでき、逆もできる。
  - unshallowする際は当然履歴を取得するのでcloneに時間がかかる

### コマンド
shallow clone でリポジトリを取得
```
$ git clone --depth 1 https://github.com/git/git
```
shallow clone をやめる(Unshallow)
```
$ git fetch --unshallow
```
通常cloneしたリポジトリをshallow clone にする
```
$ git clone --depth 1
```

### 余談
- shallow = 浅い
- shadow clone ではない。
