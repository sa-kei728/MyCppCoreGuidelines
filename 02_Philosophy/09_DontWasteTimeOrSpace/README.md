# P.9: Don’t waste time or space
時間と空間を無駄にしない

## Reason
これはC++です.

## Note
ある目標(開発スピード, リソースの安全性, テストの簡素化など)を達成するために上手に使った時間と空間は無駄にはなりません.  
"効率性を追求することのもう一つの利点は, その過程で問題をより深く理解せざるを得なくなることです." - Alex Stepanov

## Example, bad
```
struct X {
    char ch;
    int i;
    string s;
    char ch2;

    X& operator=(const X& a);
    X(const X&);
};

X waste(const char* p)
{
    if (!p) throw Nullptr_error{};
    int n = strlen(p);
    auto buf = new char[n];
    if (!buf) throw Allocation_error{};
    for (int i = 0; i < n; ++i) buf[i] = p[i];
    // ... manipulate buffer ...
    X x;
    x.ch = 'a';
    x.s = string(n);    // give x.s space for *p
    for (gsl::index i = 0; i < x.s.size(); ++i) x.s[i] = buf[i];  // copy buf into x.s
    delete[] buf;
    return x;
}

void driver()
{
    X x = waste("Typical argument");
    // ...
}
```
これは風刺画のようなものですが, 私たちはプロダクションコードで個々のミスを見てきましたし, さらに悪いこともしてきました.  
Xのレイアウトでは, 少なくとも6バイト(それ以上の可能性もある)が無駄になることが保証されていることに注意してください.  
コピー操作の偽りの定義により, ムーブのセマンティクスが無効になっているため,  
リターン操作が遅くなる(ここでは, RVO(Return Value Optimization)が保証されていないことに注意してください).  
bufに対するnewとdeleteの使用は冗長です. 本当にローカル文字列が必要ならローカル文字列を使うべきです.  
他にもいくつかパフォーマンス上のバグがあり, ありがたいことに複雑になっています.

## Example, bad
```
void lower(zstring s)
{
    for (int i = 0; i < strlen(s); ++i) s[i] = tolower(s[i]);
}
```
これは実際にプロダクションコードでの例です. 条件の中に i < strlen(s) があるのがわかります.  
この式はループの繰り返しごとに評価されるので, strlenはループごとに文字列を走査してその長さを調べる必要があります.  
文字列の内容が変化している間は, toLowerが文字列の長さに影響を与えないことが想定されるので, 
ループの外で長さをキャッシュして, 繰り返しのたびにそのコストを発生させないようにするのがよいでしょう.

## Note
個々のムダが重要な意味を持つことはほとんどなく, 重要な意味を持つ場合でも, 専門家が簡単に取り除くことができます.  
しかし, コードベース全体に散らばっている無駄は容易に重大なものとなり, また専門家は常に我々が望むほどには利用できません.  
このルール(およびそれをサポートする, より具体的なルール)の目的は, C++の使用に関連するほとんどの無駄を発生する前に排除することです.  
その後, アルゴリズムや要件に関連する無駄を検討することができますが, それはこのガイドラインの範囲外です.  

## Enforcement
さらに多くの具体的なルールが全体的な目標であるシンプルさと無償の無駄の排除を目指しています.  
- デフォルトではないユーザー定義のpostfix operator++ または operator-- 関数からの未使用の戻り値にフラグを立てます. 代わりにプレフィックス形式を使うことを推奨します. (注:"User-defined non-defaulted"はノイズを減らすことを目的としています. 実際に使ってみてまだノイズが多いようであれば, この施行を見直してください)

### 個人的な所感
これ, いい言葉ですね.
>"効率性を追求することのもう一つの利点は, その過程で問題をより深く理解せざるを得なくなることです."   
“Another benefit of striving for efficiency is that the process forces you to understand the problem in more depth.”