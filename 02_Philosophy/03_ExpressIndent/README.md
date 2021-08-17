# P.3: Express intent
意図を表現しろ

## Reason
あるコードの意図が(名前やコメントなどで)述べられていない限り,  
そのコードが意図した通りに動作しているかどうかを知ることはできません.  

## Example
```
gsl::index i = 0;
while (i < v.size()) {
    // ... do something with v[i] ...
}
```
vの要素を"ただ"ループするという意図は, ここでは表現されていません.  
インデックスという実装の詳細が(誤用される可能性があるように)公開されており,  
iはループの範囲を超えていますが, これは意図されたものかもしれませんし, 意図されていないものかもしれません.  
読者はコードのこの部分だけではわかりません.  

Better:
```
for (const auto& x : v) { /* do something with the value of x */ }
```
ここでは, 反復の仕組みについて明示されていないし, loopはconst要素への参照で動作するので, 誤って修正することはありません.  
修正が必要な場合は, こうしましょう.  
```
for (auto& x : v) { /* modify x */ }
```
for-statementの詳細については、[ES.71](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Res-for-range)を参照してください.  
さらに良い方法として, 名前付きのアルゴリズムを使用することがあります.  
この例では, Ranges TSのfor_eachを使用していますが, これは意図を直接表現しているからです.  
```
for_each(v, [](int x) { /* do something with the value of x */ });
for_each(par, v, [](int x) { /* do something with the value of x */ });
```
最後の変形は, vの要素が処理される順序には興味がないことを明確にしています.  
プログラマーは以下のことに精通している必要があります.
- [The guidelines support library](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#S-gsl)
- [The ISO C++ Standard Library](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#S-stdlib)
- 現在のプロジェクトで使用されている基礎ライブラリ

## Alternative formulation
どのようにすべきかではなく, 何をすべきかを言う.

## Note
言語構造の中には, 意図をよりよく表現するものがあります.

## Example
2つのintが2次元座標であることを意味しているなら, こうしましょう.
```
draw_line(int, int, int, int);  // 曖昧
draw_line(Point, Point);        // より明確
```

## Enforcement
より良い代替案がある共通のパターンを探す.
- 単純なforループ vs. range-forループ
- f(T*, int) インターフェース vs. f(span<T>) インターフェース
- ループ変数のスコープが大きすぎる
- 裸のnewとdelete
- 組み込み型のパラメータを多く持つ関数

### 個人的な所感
[P.1](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rp-direct)と言ってることは似たような内容かなと思います.  
