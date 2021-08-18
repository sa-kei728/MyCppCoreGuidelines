# P.7: Catch run-time errors early
ランタイムエラーは早期にcatchしろ

## Reason
謎のクラッシュを回避する.  
間違った結果につながる(おそらく認識されない)エラーを避けることができます.

## Example
```
void increment1(int* p, int n)    // bad: error-prone
{
    for (int i = 0; i < n; ++i) ++p[i];
}

void use1(int m)
{
    const int n = 10;
    int a[n] = {};
    // ...
    increment1(a, m);   // maybe typo, maybe m <= n is supposed
                        // but assume that m == 20
    // ...
}
```
ここでは, データの破損やクラッシュにつながるuse1の小さなミスを犯してしまいました.  
(pointer, count)形式のインターフェースにより, increment1()には範囲外のエラーから自分を守る現実的な方法がありません.  
添え字の範囲外アクセスをチェックできれば, p[10]がアクセスされるまでエラーは発見されないでしょう.  
より早くチェックして, コードを改善することができます.  
```
void increment2(span<int> p)
{
    for (int& x : p) ++x;
}

void use2(int m)
{
    const int n = 10;
    int a[n] = {};
    // ...
    increment2({a, m});    // maybe typo, maybe m <= n is supposed
    // ...
}
```
これで, m <= nは後からではなく, 呼び出しの時点(初期)でチェックすることができます.  
もし, nを境界にするというタイプミスがあったとしたら, コードはさらに単純化できます(エラーの可能性をなくすことができます).
```
void use3(int m)
{
    const int n = 10;
    int a[n] = {};
    // ...
    increment2(a);   // the number of elements of a need not be repeated
    // ...
}
```

## Example, bad
同じ値を繰り返しチェックしない.構造化されたデータを文字列として渡さない.
```
Date read_date(istream& is);    // read date from istream

Date extract_date(const string& s);    // extract date from string

void user1(const string& date)    // manipulate date
{
    auto d = extract_date(date);
    // ...
}

void user2()
{
    Date d = read_date(cin);
    // ...
    user1(d.to_string());
    // ...
}
```
日付は(Dateコンストラクタによって)2回検証され, 文字列(非構造化データ)として渡されます.

## Example
過剰なチェックはコストがかかります.  
その値が必要になることはないかもしれませんし, 全体よりも簡単にチェックできる部分だけが必要になるかもしれないので,  
早めのチェックが非効率的な場合もあります.  
同様にインターフェイスの漸近的な動作を変えるような有効性チェックを追加してはいけません.  
(例. 計算量がO(1)のインターフェイスにO(n)のチェックを追加してはいけない)
```
class Jet {    // Physics says: e * e < x * x + y * y + z * z
    float x;
    float y;
    float z;
    float e;
public:
    Jet(float x, float y, float z, float e)
        :x(x), y(y), z(z), e(e)
    {
        // Should I check here that the values are physically meaningful?
    }

    float m() const
    {
        // Should I handle the degenerate case here?
        return sqrt(x * x + y * y + z * z - e * e);
    }
};
```
ジェット機の物理法則(e * e < x * x + y * y + z * z)は,  
測定誤差が生じる可能性があるため, 不変ではありません.

## Enforcement
- ポインタと配列を見る: 繰り返しではなく早期に範囲チェックを行う.  
- 変換を見る: 狭義の変換を排除するか, マークを付ける.
- 入力から来るチェックされていない値を探す.
- 構造化されたデータ(不変量を持つクラスのオブジェクト)が文字列に変換されていないか？

### 個人的な所感
自明な内容ではありますが, 下記を考慮しつつ実施するというのがなかなか難しいのかなという所感.  
>過剰なチェックはコストがかかります.  
その値が必要になることはないかもしれませんし, 全体よりも簡単にチェックできる部分だけが必要になるかもしれないので,  
早めのチェックが非効率的な場合もあります.  
同様にインターフェイスの漸近的な動作を変えるような有効性チェックを追加してはいけません. 

