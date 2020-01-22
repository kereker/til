## PHPファイルからWebAPIを呼ぶ方法
`curl`のライブラリを使う。

### そもそも、 `curl` ってなによ
~~それにつけてもおやつはカール~~  
```
cURL（カール）は、さまざまなプロトコルを用いてデータを転送するライブラリとコマンドラインツールを提供するプロジェクトである。
※Wikipediaから抜粋
```
Linuxなどのコマンドラインから使われたりする。  
「このAPIの返却値見てみたい」とかでお試しでよく使われたり、今回みたいにプログラム上からAPIを叩くときに使われたりする。

### サンプルコード
```php
<?php

// リクエストヘッダ
$header = array(
    'X-PrettyPrint: 1',
    'Content-Type: application/json',
);
// リクエストボディ
$body = array(
  'id' => '1',
  'flg' => '0'
);

$cl = curl_init();
curl_setopt_array($cl, [
        CURLOPT_URL => 'http://example.com/',           // 行き先URL
        CURLOPT_RETURNTRANSFER => true,                 // レスポンスボディを文字列でもらうかどうか
        CURLOPT_HTTPHEADER => $header,                  // リクエストヘッダー
        CURLOPT_POST => true,                           // POSTかどうか
        CURLOPT_POSTFIELDS => http_build_query($body),  // 
]);
$response = curl_exec($cl);
curl_close($cl);
```
