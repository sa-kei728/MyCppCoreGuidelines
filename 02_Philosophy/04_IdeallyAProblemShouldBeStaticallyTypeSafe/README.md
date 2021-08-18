# P.4: Ideally, a program should be statically type safe
理想的には, プログラムは静的に型安全であること

## Reason
理想的なのは, プログラムが完全に静的に(コンパイル時に)型安全になることです.  
残念ながら, それは不可能です.
問題のある分野:
- Unions
- キャスト
- 関数の引数に渡すことで配列からポインタに変換されることによる[配列減衰](https://www.geeksforgeeks.org/what-is-array-decay-in-c-how-can-it-be-prevented/)
- 範囲エラー
- [Narrowing conversion](https://www.ibm.com/docs/en/ztpf/2020?topic=warnings-narrowing-conversion)

## Note
これらの分野は深刻な問題(クラッシュやセキュリティ違反など)の原因となっています.  
私たちは, それに代わる技術を提供しようとしています.  

## Enforcement
私たちは, 個々のプログラムの必要性と実現可能性に応じて,  
個々の問題カテゴリーを別々に禁止, 抑制, または検出することができます.  
常に代替案を提案する.  
例:
- Unions: [variant](https://cpprefjp.github.io/reference/variant/variant.html)を使用.
- キャスト: 使用を最小限にする. [template](https://docs.oracle.com/cd/E19957-01/805-7887/6j7dsdheo/index.html)が便利.
- 配列減衰: [span](https://github.com/microsoft/GSL/wiki/gsl::span-and-std::span)(GSL)を使用.
- 範囲エラー: spanを使用.
- Narrowing conversion: 使用を最小限に抑え, 必要に応じて[narrow](https://github.com/microsoft/GSL/blob/main/include/gsl/narrow)または[narrow_cast](https://github.com/microsoft/GSL/blob/main/include/gsl/util#L95)(GSL)を使用.

### 個人的な所感
型安全性に伴うトラブルは結構起こりうるので対処するに越したことはない認識.  
とはいえ組込み周りでキャストはバンバン使うので結構悩みどころ.(C++よりかはC依りの話ですが.)  
STLで対処できる範囲のものはSTLで対応したいところ.  
