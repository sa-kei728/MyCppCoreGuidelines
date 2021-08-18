# P.6: What cannot be checked at compile time should be checkable at run time
コンパイル時にチェックできないものは, ランタイムにチェックできるようにすべき

## Reason
検出しにくいエラーをプログラムに残すことは, クラッシュや悪い結果を招くことになります.

## Note
理想的には, すべてのエラー(プログラマのロジックのエラーではない)をコンパイル時またはランタイムのいずれかでキャッチすることです.  
しかし, コンパイル時にすべてのエラーを検出することは不可能ですし, 実行時に残りのエラーをすべて検出することも不可能な場合が多いでしょう.  
しかし, 十分な資源(解析プログラム, 実行時チェック, マシンリソース, 時間)があれば, 原理的にチェック可能なプログラムを書くように努力すべきである.

## Example, bad
```
// separately compiled, possibly dynamically loaded
extern void f(int* p);

void g(int n)
{
    // bad: the number of elements is not passed to f()
    f(new int[n]);
}
```
ここでは, 重要な情報(要素数)が徹底的に"隠蔽"されているため, 静的解析はおそらく不可能であり,  
f()がABIの一部であり, そのポインタを"計測"することができない場合には, 動的チェックは非常に困難になります.  
役に立つ情報をフリーストアに埋め込むこともできますが, それにはシステムやコンパイラの大規模な変更が必要になります.  
このように, エラー検出が非常に困難な設計になっています.

## Example, bad
もちろん, ポインタと一緒に要素数を渡すこともできます.
```
// separately compiled, possibly dynamically loaded
extern void f2(int* p, int n);

void g2(int n)
{
    f2(new int[n], m);  // bad: a wrong number of elements can be passed to f()
}
```
要素数を引数として渡すことは,  
単にポインタを渡して要素数を知るための(明示されていない)慣習に頼るよりも優れています(そして, はるかに一般的です).  
しかし, 単純なタイプミスが重大なエラーを引き起こす可能性があります.  
f2()の2つの引数の間の接続は明示的ではなく慣習的です.  

また, f2()がその引数を削除することになっているのは暗黙の了解です(あるいは呼び出し側が2回目のミスをしたのでしょうか).

## Example, bad
標準ライブラリのスマートポインターは, オブジェクトを指すときにサイズを渡すことができません.  
```
// separately compiled, possibly dynamically loaded
// NB: this assumes the calling code is ABI-compatible, using a
// compatible C++ compiler and the same stdlib implementation
extern void f3(unique_ptr<int[]>, int n);

void g3(int n)
{
    f3(make_unique<int[]>(n), m);    // bad: pass ownership and size separately
}
```

## Example
ポインタと要素数を一体のオブジェクトとして渡す必要があります.  
```
extern void f4(vector<int>&);   // separately compiled, possibly dynamically loaded
extern void f4(span<int>);      // separately compiled, possibly dynamically loaded
                                // NB: this assumes the calling code is ABI-compatible, using a
                                // compatible C++ compiler and the same stdlib implementation

void g3(int n)
{
    vector<int> v(n);
    f4(v);                     // pass a reference, retain ownership
    f4(span<int>{v});          // pass a view, retain ownership
}
```
この設計では, 要素数をオブジェクトの不可欠な部分として一緒に運ぶので,  
エラーが発生する可能性は低く, ランタイムチェックは常に実現可能である.

## Example
所有権と, 使用の検証に必要なすべての情報の両方をどのようにmoveするか？
```
vector<int> f5(int n)    // OK: move
{
    vector<int> v(n);
    // ... initialize v ...
    return v;
}

unique_ptr<int[]> f6(int n)    // bad: loses n
{
    auto p = make_unique<int[]>(n);
    // ... initialize *p ...
    return p;
}

owner<int*> f7(int n)    // bad: loses n and we might forget to delete
{
    owner<int*> p = new int[n];
    // ... initialize *p ...
    return p;
}
```

## Example
- これは, 実際には必要なものを知っているのに, 多義的な基底クラスを渡すインターフェースによって可能なチェックがどのように回避されるかを示しています.あるいは"フリースタイル"のオプションとしての文字列.

## Enforcement
- (pointer, count)スタイルのインターフェースにフラグを立てる(これは, 互換性の理由で修正できない多くの例にフラグを立てることになる)

### 個人的な所感
extern実装に対して渡す場合には特に注意すべき問題.  
配列の類はvector参照かspanで渡そう!
