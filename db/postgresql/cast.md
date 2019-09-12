### 結論
- PostgreSQLは、比較対象の両辺の型が異なるとエラーが出る
- SQLを見直すか、castのINOUT属性を指定する

### 経緯
`select hoge from hogehoge where hoge = '1'`  
みたいなSQLを叩いたところ、  
`operator does not exist: character = integer`  
PostgreSQLからこんなエラーが出た。

### 原因
カラム `hoge` の型、実は `integer` だった。  
PostgreSQL 8.3で型変換チェックが厳密になり、両辺の型が揃っていないとエラーを吐く。  

### 対処法
SQLを見直す。  
もしくは、 `CREATE CAST` で、キャスト定義を行う(8.4以上)  
`CREATE CAST (int4 AS text) WITH INOUT AS IMPLICIT;`
