# P.5: Prefer compile-time checking to run-time checking
ランタイムチェックよりもコンパイル時チェックを優先せよ

## Reason
コードの明快さとパフォーマンス.  
コンパイル時にキャッチしたエラーに対して, エラーハンドラを書く必要がありません.  

## Example
```
// Int is an alias used for integers
int bits = 0;         // don't: avoidable code
for (Int i = 1; i; i <<= 1)
    ++bits;
if (bits < 32)
    cerr << "Int too small\n";
```
この例は実現しようとしていることを達成できていない(オーバーフローが未定義)なので, 単純な static_assert に置き換えるべきです.  
```
// Int is an alias used for integers
static_assert(sizeof(Int) >= 4);    // do: compile-time check
```
あるいは, 型システムを利用してintをint32_tに置き換えるのが良いでしょう.

## Example
```
void read(int* p, int n);   // read max n integers into *p

int a[100];
read(a, 1000);    // bad, off the end
```
↓ better
```
void read(span<int> r); // read into the range of integers r

int a[100];
read(a);        // better: let the compiler figure out the number of elements
```

## Alternative formulation
コンパイル時にうまくできることをランタイムに先送りしてはいけません.

## Enforcement
- ポインタ引数を探す.
- 範囲違反のランタイムチェックを探す.

### 個人的な所感
コンパイルで弾けるようにコーディングしておくのは大事.  
自明といえば自明な内容ですが, 実践時にあまり意識できてないかも...
