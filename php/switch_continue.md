### `switch`文中の`continue`の仕様がにくたらしい
```PHP
for ($i = 0; $i < 5; $i++) {
  switch ($i) {
    case 2:
    case 4:
      continue;   // ループスキップして頭から処理をし直したい
  }
  print "$i\n";
}
```
↑を実行すると↓のようになる
```
0
1
2
3
4
```
本当は`2`と`4`はスキップして出力しないでほしいのに、なぜか出力されちゃう。

#### なんでこうなるのさ
```
注意: PHP では、continue の動作に関しては switch 文がループ構造とみなされます。
continue に引数を渡さなかった場合の動きは break と同じです。
switch がループ内にある場合、continue 2 とすると、外側のループの次回の処理から続行します。
```
([※PHPリファレンスより抜粋](https://www.php.net/manual/ja/control-structures.continue.php))  

書いてあるとおり、PHPの`switch`文中の`continue`は、`switch`を**ループ**として扱う。  
なので、上記のコードの`continue`は単に、**`switch`を抜けただけ。**  
後続の処理はそのまま行い、`for`ループ処理を行った、というわけ。  

言ってしまえば`break`と同じ仕様をたどる。  
どうしてこういう仕様にしたのか……。ややこしすぎる。

#### 対応策
公式から言われている通り、`continue`に引数を渡してあげればよいだけ。
```PHP
for ($i = 0; $i < 5; $i++) {
  switch ($i) {
    case 2:
    case 4:
      continue 2;   // continueに引数を渡す
  }
  print "$i\n";
}
```
