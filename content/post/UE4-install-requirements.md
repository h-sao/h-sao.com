+++
date = "2016-11-16T20:45:00+09:00"
draft = false
title = "[UE4] Unreal Engine4のインストール要件"
tags = ["UE4"]
+++

仕事上、色んな Game Engine を試しておりまして  
先日、クリーンで、比較的低スペックな Windows10 PC に  
Epic さんの Unreal Engine 4 をインストールしたときに、はふん？と思った件のメモです

---

**Enreal Engine 4 のダウンロード** はダッシュボードから出来ます (ログインが必要)

- **Enreal Engine 4 ダウンロード**  
[https://www.unrealengine.com/dashboard](https://www.unrealengine.com/dashboard)

<img src="/pic/UE4-install-requirements1.png" style="border:solid 5px #e6e6e6"/>  

迷わず、Windows のプラットフォームを選択！

落としてきた EpicGamesLauncherInstaller-2.12.14-3176191.msi (2016/11/16現在) を実行します

あれ？


<img src="/pic/UE4-install-requirements0.png" style="border:solid 5px #e6e6e6"/>  

> コンピューターにXINPUT1_3.dll がないため、プログラムを開始できません。この問題を解決するには、プログラムを再インストールしてみてください。

はて。。。何やら懐かしい香りのDLL名がエラーに上がっています(￣▽￣)

UE4 のインストール要件って、なんだったっけ？と思ったのですが、ダウンロードの画面には、何も書かれてないんですよね～

よく判らないから、ドキュメントを見てみよう！ということで  
「ハードウェア＆ソフトウェア仕様」のドキュメントサイトを見ていくと  
ソフトウェアのインストール要件が書かれていました

- ハードウェア＆ソフトウェア仕様  
[https://docs.unrealengine.com/latest/JPN/GettingStarted/RecommendedSpecifications/index.html](https://docs.unrealengine.com/latest/JPN/GettingStarted/RecommendedSpecifications/index.html)

 - エンジンの実行環境   
   OS : Windows 7 / 8 64-bit  
   **DirectX Runtime : [DirectX End-User Runtimes (June 2010)](https://www.microsoft.com/en-us/download/details.aspx?id=8109)**

おぉぉ、**DXのランタイム**！お前さんだったのかぁ～

というわけで、**DirectX End-User Runtimes (June 2010)** を入れたら、すんなり UE4 のインストールは出来ました(^o^)

ランチャーのランタイムエラーが判りにくいよ…と思ったけど  
これくらいのことはゲーム開発する人にとったら、許容範囲ですかね(^^;)

あと実は、UE4 は Win10 を  
今日現在で、オフィシャルにはサポートしてないのかぁと思いました

単にドキュメントの更新の問題かしら？？？

まぁ今のところ、わたしの Windows10 の環境では  
問題なく UE4 の開発環境は動いてるので、特に問題ないです(^^)