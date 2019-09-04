## 20190903

slackの個人メモにまとめておいたやつ。  
順不同にごりごりしていく(後に整理する忘れない)

### リモートの最新情報を取得する(ファイルはマージされない)
`git fetch`
### リモートの最新情報を取得する(ファイルはマージされる)
`git pull`
### リモートブランチからローカルブランチを切る
`git checkout -b <branch name> origin/<branch name>`
### ブランチ一覧確認
`git branch` : ローカルのみ  
`git branch -r` : リモートのみ  
`git branch -a` : ローカル＆リモート  
### マージ
`git merge <branch name>`  
※注意：実行するのはマージ対象のブランチで行う  
<details>
<summary>例：feature/hoge ブランチに develop ブランチの情報をマージする場合</summary>

```
$ git checkout feature/hoge # 切替
$ git merge develop # マージ
```

</details>

### ローカルでの変更情報の確認
`git status`
### ローカルの変更をコミット対象に載せる
`git add <file name>`
<details>
<summary>※以下のような記載でも可(展開)</summary>

```
$ git add <directory name>  # 指定ディレクトリが対象
$ git add .  # すべてのディレクトリ配下のファイルが対象
$ git add *.php  # 拡張子「PHP」と名の付くファイルが対象

```
</details>

### コミット
`git commit -m '<comment>'`  
※ `git commit` のみでも可。gitにおいてコミットコメントは必須なので、vimが開く。  
コメント記入後、 `:wq` でコミット完了。  
中断する場合は `:q!`
### ローカルの変更状態を退避する
`git stash`
### 退避した状態を戻す(退避データは消える)
`git stash pop`
### 退避した状態を戻す(退避データは消えない)
`git stash apply`
### ローカルでのコミットをリモートに反映する
`git push origin <branch name>`
### ブランチ切替
`git checkout <branch name>`
