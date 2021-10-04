+++
date = "2021-10-05T00:30:00+09:00"
draft = false
title = "[C++]Compiler Explorerの出力結果の表示方法"
tags = ["C++"]
+++

**Compiler Explorer** というオンラインコンパイルサービスがあります

- **Compiler Explorer**  
[https://godbolt.org/](https://godbolt.org/)

書いたC++コードのアセンブリ出力結果をリナルタイムに表示してくれるサービスです

![](/pic/How-to-show-stdout-on-Compiler-Explorer_00.png)

すごーーー👀  
初めてこのサービスを見た時には感激しました〜

さておき、  
わたし、これで実行結果を見る方法を知らなかったんですね…ハズカシー(*ﾉдﾉ)

「Output」を押すと出力結果ウィンドウは出てきますが  
このリターン値だけじゃなくて、 `std::cout` の結果も欲しいんですけど…👀 

![](/pic/How-to-show-stdout-on-Compiler-Explorer_01.png)

ちょっと調べてみたらすぐに判明しました＞＜。

Outputの設定項目に **Execute the code** のチェックボックスがあるので、それを選択すれば表示されます

![](/pic/How-to-show-stdout-on-Compiler-Explorer_02.png)

出力はこんな感じですね〜

![](/pic/How-to-show-stdout-on-Compiler-Explorer_03.png)

便利です〜