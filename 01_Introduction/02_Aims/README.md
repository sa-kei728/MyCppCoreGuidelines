# Aims
この文書の目的は開発者が最新のC++を採用し,  
コードベース間でより統一されたスタイルを実現することです.  

私達はこれらのルール一つ一つが全てのコードベースに効果的に適用できるとは思っていません.  
古いシステムを更新するのは大変です.  
しかし, 私達はルールを使用したプログラムが使用していないものに比べエラーが少なく保守性が高いと信じています.  
多くの場合, ルールは初期の開発をより早く/簡単にします.  
これらのルールはゼロオーバーヘッドの原則に従っていると言えます.  
新しいコードではこれらのルールを理想とし, 古いコードではそれを利用する機会を考え,  
可能な限りこれらの理想に近づけるようにしてください.  

## In.0 Don't panic!
ガイドラインのルールが自分のプログラムに与える影響を理解するために時間をかけてください.  

このガイドラインは"subset of superset"の原則([Stroustrup05](https://www.stroustrup.com/SELLrationale.pdf))に基づいて作成されています.  
単に使用するC++のサブセットを定義するものではないです.  
その代わり, C++の最もエラーの起こしやすい機能の使用を冗長にするいくつかの単純な"拡張"の使用を強く推奨しており,  
それらを禁止することができます.  

このルールでは, 静的な型安全性とリソース安全性を重視しています.  
そのため, [範囲チェックの可能性](https://www.jpcert.or.jp/sc-rules/c-int08-c.html), [nullptrの参照を避ける](https://www.jpcert.or.jp/sc-rules/c-exp34-c.html), [ダングリングポインタを避ける](https://www.jpcert.or.jp/sc-rules/c-mem01-c.html), [(RAIIによる)例外の体系的な使用](https://programming-place.net/ppp/contents/cpp/language/032.html)を重視しています.  
エラーの原因となる不明瞭なコードを最小限に抑えるため, ルールはシンプルさと,  
必要な複雑さを十分に仕様化されたインターフェースの後ろに隠すことを重視しています.  

ルールの多くは規定的なものです. 私達は代替案を示さずに単に"してはいけない"というだけのルールに違和感を覚えます.  
その結果, 正確で機械的に検証可能なチェックだけでなく, ヒューリスティックな方法でしかサポートできないルールもあります.  
また, 一般的な原則を明示したルールもあります.  
このような一般的なルールに対しては, より詳細で具体的なルールで部分的なチェックを行います.  

このガイドラインでは, C++の中核部分とその使用方法を説明しています.  
大規模な組織や特定のアプリケーション分野, さらには大規模プロジェクトでは, さらなるルールやさらなる制限,  
さらなるライブラリのサポートが必要になることが予想されます.  
例えば, ハードリアルタイムプログラマーはダイナミックメモリを自由に使うことができず, ライブラリ選択にも制限があります.  
このようなより具体的なルールを, このコアガイドラインの補遺として作成することをおすすめします.  
自分の理想とする小さな基礎ライブラリを作り, それを使うことで, プログラミングのレベルを華やかなアセンブリコードにまで下げるのではなく...  

ルールは段階的に導入できるように設計されています.  

様々な安全性を高めることを目的としたルールもあれば, 事故の可能性を減らすことを目的としたルールもあり, 多くはその両方です.  
事故防止を目的としたガイドラインでは, 完全に合法的なC++を禁止することが多い.  
しかし, あるアイデアを表現する方法が2つあり, 一方が一般的なエラーの原因になり, もう一方がそうでない場合,  
私達はプログラマーを後者に導くようにしています.  
