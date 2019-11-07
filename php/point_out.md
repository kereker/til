## 業務中、ソースコードを見てて「あ～あ」って思うこと
をまとめた。

### そもそも危険系
#### SQL問い合わせ
```php
$sql = 'select hoge, tes from t_test where id="'. pg_escape_string($id) .'"';
$res = pg_query($con, $sql);
```
`pg_escape_string()`を通してやってるのは100歩譲るとしても、この書き方を許してはならない。  
関数を通すのを忘れてしまうと、**SQLインジェクション**という攻撃を食らうことになる。  

`pg_prepare()` などを使って、プリペアドステートメントで問い合わせを行うようにする。
```php
$prepare = pg_prepare($con, 'test', 'select hoge, tes from t_test where id = $1');
$result = pg_execute($con, 'test', array($id));
```

### 関数使えばいいのに系
#### `implode()` で楽になる
```php
$csv = $a . ','.
       $b . ','.
       $c . ','.
       $d;
```
つなぎ目が`,`って予めわかっているなら、こう
```php
$csv_contents = array($a, $b, $c, $d);
$csv = implode(',', $csv_contents);
```
`implode()`は、配列を文字列に変換する関数で、第一引数に指定した値を結合子として配列の要素を結合、文字列として変換する。  
逆のことをしたい場合は`explode()`という関数を使うと、文字列から配列に変換できる(要結合子)。

### 他
#### if の無駄遣い その１
昔やった覚えがあるけど…。
```php
$result = $_POST['result'];

if ($result) {
    // NOP
} else {
    $msg = 'failed';
    throw new Exeption($msg);
}
```
もはやなにもいうまい。
```php
$result = $_POST['result'];

if (!$result) {
    $msg = 'failed';
    throw new Exeption($msg);
}
```
#### if の無駄遣い その２
```php
$flg = true;
if ($a == 1) {
    $flg = true;
} else {
    $flg = false;
}
```
こうすればスマートになる
```php
$flg = ($a == 1);
```
#### throwしたあとの処理
```php
if (!$result) {
    $msg = 'faild';
    throw new Exception($msg);
    exit();
}
```
`exit()`いらなーい。
