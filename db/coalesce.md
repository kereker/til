## こんなデータがあった
テーブル名：test

|id|year|month|day|
|:--:|--:|--:|--:|
|1|2019|01|01|
|2|2019|02|01|
|3| | | |

### こういうことをしたい
`2019/01/02`以降のレコードが取りたい。↓  
```sql
SELECT
    *
FROM
    test
WHERE
    TO_DATE(year || '/' || month || '/' || day, 'YYYY/MM/DD') >= '2019/01/02'
```
…ってやったらこんなエラーが出た。
```
Query failed: ERROR: invalid value &quot;//&quot; for &quot;YYYY&quot; DETAIL: Value must be an integer. 
```

### どういうこと？
`TO_DATE(year || '/' || month || '/' || day, 'YYYY/MM/DD')` は、カラム同士を結合させて、日付を作っている。  
ただし、値が**空文字**だったらエラーが出る。空文字のときの`TO_DATE()`の引数は↓  
`TO_DATE('//', 'YYYY/MM/DD')`  
というふうになってしまうため。

### 回避策
職場の方から教えて頂いた `COALESCE()` 関数を使う。  
この関数は、指定した引数の中から、**NULLではない最初の引数を返してくれる**もの。  
つまりこうする↓
```sql
SELECT
    *
FROM
    test
WHERE
    TO_DATE(COALESCE(year, '0') || '/' || COALESCE(month, '0') || '/' || COALESCE(day, '0'), 'YYYY/MM/DD') >= '2019/01/02'
```
が、変わらず。エラーが発生した行のカラムには`NULL`ではなく、**空文字**が入ってたオチ。  
なので`NULLIF()`も使って試すことに。  
この関数は、指定した引数の値が**同じであればNULLを返す**もの。なので、
```sql
SELECT
    *
FROM
    test
WHERE
    TO_DATE(COALESCE(NULLIF(year, ''), '0') || '/' || COALESCE(NULLIF(month, ''), '0') || '/' || COALESCE(NULLIF(day, ''), '0'), 'YYYY/MM/DD') >= '2019/01/02'
```
とすると、カラムが空文字のとき、`TO_DATE()`の引数はこう↓なって、  
`TO_DATE('0/0/0', 'YYYY/MM/DD')`  
取ってきてくれるデータは、

|id|year|month|day|
|:--:|--:|--:|--:|
|2|2019|02|01|

↑な感じで、`id:2`のレコードだけが取れて幸せハッピー。

## まとめ
- nullableなカラムに対して、文字列結合して関数で処理させたい場合は`COALESCE()`をかませると吉
- nullableだが、値が`null`ではなく`空文字`の場合は`NULLIF()`もかませる
- そもそもDB設計ちゃんと考えてやろうね
