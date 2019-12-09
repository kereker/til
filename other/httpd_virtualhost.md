## そもそもVirtualHostって何者なのよ
- 1台のマシン上で、2つ以上のwebシステムを立ち上げたい
- ドメインを分けて運用できるもの
- Aというシステムを運用しているが、別のBというシステムを運用したいときとか  
はたまたローカル環境で、同じサーバー設定だが、システム自体が違うものの実装をしたいときとかに使える。
  - 自分は後者。あとお試しコードとか置きたい用に置いてる。

## 作り方
### 確認環境
- ホストOS：Windows10
- ゲストOS：CentOS 7.2(Vagrant)
  - Apache 2.4.6
### ゲスト側
1. `/etc/httpd/conf.d` に新しいconfファイルを作成する  
名前はなんでもよい。
2. ファイルに設定ごりごり。  
`ServerName` はなんでもよい。自分はローカルであれば末尾に`.local`をつけて締める。
```bash
# ひとつめのドメイン
<VirtualHost *:80>
    DocumentRoot /var/www/html/mainDomain
    ServerName mainDomain.local
</VirtualHost>
# ふたつめのドメイン
<VirtualHost *:80>
    DocumentRoot /var/www/html/sample
    ServerName sample.local
</VirtualHost>
```
3. httpd再起動
### ホスト側
これをやらないとDNS定義がないので迷子になる。
1. `(OSがインストールされてるドライブ)\Windows\System32\drivers\etc\hosts` を修正。  
管理者権限があればそのまま、なければデスクトップにコピーしてくる。
2. `<ゲストOSのIP> <設定したドメイン名>`と言った感じでhostsを修正。  
```
(例)
192.168.1.1 mainDomain.local
192.168.1.1 sample.local
```
3. (必要に応じて)OS再起動  
再起動しなくてもいけることはいける。反映されなかったらの場合。
