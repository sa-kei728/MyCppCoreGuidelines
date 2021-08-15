# MyCppCoreGuidelines
http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines  
の内容を日本語化して自分なりのサンプルコードを作成したもの.  
基本的には[DeepL](https://www.deepl.com/translator)で直訳したものを載せてますが,  
訳が明らかに間違ってそうな箇所や独特な言い回しは自分なりの文書に直している箇所もあります.  
Supporting sectionsは対応していません.  

## Update Date
2021/08/15

## Abstract
この文書はC++を上手に使うためのガイドラインです. この文書の目的はC++を効果的に使用できるようにすることです.  
"modern C++"とは, ISO C++標準(最新はC++20だけど, 推奨事項はC++11,14,17にも適用される)の効果的な使用を意味する.  

このガイドラインはインターフェース, リソース管理, メモリ管理, 並行性など比較的ハイレベルな問題に焦点を当てている.  
このようなルールはアプリケーションアーキテクチャやライブラリ設計に影響を与えます.  
このルールに従うことで静的に型安全な, リソースリークのない, 今日のコードでは一般的ではない多くのロジックエラーを検出するコードが生まれます. また, 高速に動作するので正しいことをする余裕が生まれます.  

私達は命名規則やインデントのスタイルなどの低レベルの問題には関心がないが,  
プログラマーの助けになるような話題は禁止していることはありません.  

私達が最初に決めたルールは様々な形の安全性とシンプルさを重視している. しかしこれは厳しすぎるかもしれない.  
現実のニーズに対応するにはより多くの例外を導入すべきだと予想され, またより多くのルールが必要です.  

あなたの期待若しくは経験に反したルールも見つかるでしょう.  
もし私達があなたのコーディングスタイルを変える提案をしなかったら, 失敗したことになる.  
ぜひルールの検証や反証をしてみてください.  
特にいくつかのルールについては測定値やより良い例で裏付けてほしいと思っている.  

ルールの中には自明なものや些細なものもあるだろう.  
ガイドラインの目的の一つは経験の浅い人や異なる背景, 言語をもつ人がスピードアップするのを助けることだということを覚えておいて欲しい.  

ルールの多くは分析ツールによってサポートされるように設計されています.  
ルールに違反した場合は, 関連するルールへの参照(もしくはリンク)とともにフラグが立てられます.  
コードを書く前に全てのルールを暗記することは期待していません.  
このガイドラインは, 人間が読むことの出来るツールの仕様書と考えることもできますが, それだけではありません.   

## Agenda
[In:Introduction](./01_Introduction/README.md)  
[P:Philosophy](./02_Philosophy/README.md)  
[I:Interfaces](./03_Interfaces/README.md)  
[F:Functions](./04_Functions/README.md)  
[C:Classes and class hierarchies](./05_ClassesAndClassHierarchies/README.md)  
[Enum:Enumerations](./06_Enumerations/README.md)  
[R:Resource management](./07_ResourceManagement/README.md)  
[ES:Expressions and statements](./08_ExpressionsAndStatements/README.md)  
[Per:Performance](./09_Performance/README.md)  
[CP:Concurrency and parallelism](./10_ConcurrencyAndParallelism/README.md)  
[E:Error handling](./11_ErrorHandling/README.md)  
[Con:Constants and immutability](./12_ConstantsAndImmutability/README.md)  
[T:Templates and generic programming](./13_TemplatesAndGenericProgramming/README.md)  
[CPL:C-style programming](./14_CstyleProgramming/README.md)  
[SF:Source files](./15_SourceFiles/README.md)  
[SL:The Standard Library](./16_TheStandardLibrary/README.md)  

# Environment
- OS(dist): Linux(Ubuntu 18.04 LTS)
- CPU: x86_64
- compiler: gcc 7.5.0
