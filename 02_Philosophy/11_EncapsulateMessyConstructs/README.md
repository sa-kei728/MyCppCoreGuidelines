# P.11: Encapsulate messy constructs, rather than spreading through the code
厄介な構造物をコード中に分散させず、カプセル化せよ

## Reason
雑然としたコードはバグを隠しやすく, 書くのが大変です.  
優れたインターフェイスは, より簡単で安全に使用できます. 雑然とした低レベルのコードは, そのようなコードをより多く生み出す.  

## Example
```
int sz = 100;
int* p = (int*) malloc(sizeof(int) * sz);
int count = 0;
// ...
for (;;) {
    // ... read an int into x, exit loop if end of file is reached ...
    // ... check that x is valid ...
    if (count == sz)
        p = (int*) realloc(p, sizeof(int) * sz * 2);
    p[count++] = x;
    // ...
}
```
これは低レベルで, 冗長で, エラーが発生しやすいものです.  
例えば, メモリの枯渇をテストすることを"忘れて"いました. 代わりにvectorを使うことができます.  
```
vector<int> v;
v.reserve(100);
// ...
for (int x; cin >> x; ) {
    // ... check that x is valid ...
    v.push_back(x);
}
```

## Note
標準ライブラリとGSLはこのPhilosophyの例です.  
例えば, vector, span, lock_guard, futureなどの主要な抽象化を実装するために必要な  
配列, Union, キャスト, トリッキーな寿命の問題, gsl::ownerなどに手を出す代わりに,  
通常の私たちよりも多くの時間と専門知識を持つ人々によって設計・実装されたライブラリを使用しています.  
同様に, ユーザー(多くの場合, 私たち自身)に低レベルのコードを繰り返しうまく処理するという課題を負わせるのではなく,  
より専門的なライブラリを設計・実装することもできますし, そうすべきです.  
これは, このガイドラインの根底にある"スーパーセットのサブセットの原則"の変形です.

## Enforcement
- 複雑なポインタ操作やキャストなど, 抽象化された実装の外側にある"面倒なコード"を探してください. 

### 個人的な所感
自明ではありますが, レガシーなコードだと避けられないことは多々あります.  
辛いですがやっていきましょう. 
