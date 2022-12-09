+++
date = "2022-12-07T20:00:00+09:00"
draft = false
title = "[C++] VC++でHackerRankの環境構築"
tags = ["cpp", "HackerRank"]
+++

> この記事は [C++ Advent Calendar 2022](https://qiita.com/advent-calendar/2022/cxx) 7日目の記事です。

（内容まとめ）  
競プロ勢にとったら当たり前のローカル環境設定についての記事です  
自分の言葉でまとめなおしたメモになります  
みんな！AtCoder もいいけど HackerRank も面白いよ！

---

今年の3月ごろ、同僚（海外の方）から  
「HackerRank 結構いいよ、わたしは週末によくやってるよ～」  
と教えてもらいまして  
たまにポチポチ解いています

- HackerRank  
[https://www.hackerrank.com/](https://www.hackerrank.com/)

ざっくり言うと、お題に対するコードを書いて、要求通りの出力が出来れば Pass! でポイントが溜まっていってバッチがもらえたりします  
わたしは主にC++の Problem Solving を解いています  
（数学系の問題も多いです）

> C++ language のジャンルで解くことも出来ますが、こっちは文法問題なので、あまり面白くない

HackerRankは他のオンラインコーディングプラットフォームとはちょっと異なっていて  
競技で問題を解いてランクを上げるというよりは、スキルの習得に焦点を当てているような気がします  
解いて欲しい問題用の関数がミニマムに提供されてて、最初に記載するであろう入力系の処理が予め記載されているので、解くのに集中出来ますね

で、それまで真面目に `Easy` 問題を解いていたのですが、簡単すぎるし量も多い…つまらん…  
となって、教えてくれた同僚に  
「HackerRank、何の問題を解いてる？」  
と聞いたところ、  
「えぇー `Easy` やってるの？時間の無駄だよ～わたしは `Hard` しかやらないよ」  
と突っ込まれました＞＜。

さてそこで、`Hard` 問題をやり始めましたが、あら。。。難しい。。。  
こんなけ難しかったらデバッグしたくなるんだけど、HackerRankのサイトに組み込まれたコードエディタ上から問題を解くので、プリントデバッグしか方法がない  
変数の中身や配列の中身をもっとアグレッシブに確認したい…

前置きが長くなりましたが、HackerRankをやるときに、Visual Studio上でコードを書いて確認する方法を記載しましたｍｍ

＜今回の環境＞

- Windows10
- Visual Sutdio 2019
- Hacker Rank の C++ 問題を対象

---

## VSにコードコピペしてHackerRankの問題を解きたい

コピペ出来ないとしんどいです！

しかし HackerRank も他の競プロと同じく、**#include ＜bits/stdc++.h＞** があります  
そのまんまじゃリンク通りません

なので、ライブラリを認知する箇所に stdc++.h を置いておけば良いのです

- <bits/stdc++.h> のコピペ元のgist  
**reza-ryte-club/stdc++.h**  
[https://gist.github.com/reza-ryte-club/97c39f35dab0c45a5d924dd9e50c445f](https://gist.github.com/reza-ryte-club/97c39f35dab0c45a5d924dd9e50c445f)

他にも色んな方が公開されてますけど、検索するとこの方の↑↑↑リンクがよく見つかりました

これを、VC++がデフォで認知する場所に置きます

一番簡単な場所に調べ方は、

```cpp
#include <iostream>
```

とコードに書いて、`F12` で `iostream` の場所までジャンプ  
→ソースのタブを右クリックすると、そこのディレクトリの場所までジャンプ出来る

![](/pic/How-to-develop-HackerRank-for-VSCode_00.png)

> ちなみにわたしの場合は、ここにありました  
C:\Program Files (x86)  
　\Microsoft Visual Studio  
　　\2019  
　　　\Enterprise  
　　　　\VC  
　　　　　\Tools  
　　　　　　\MSVC  
　　　　　　　\14.29.30133  
　　　　　　　　\include

## エラーやワーニングが出る場合

めんどいですが、毎回エラー除去のための `define` を書くのが無難な対応だと思います  
（業務のコードじゃないし）

例えば、HackerRank でよく出てくるこんなコード

```cpp
ofstream fout(getenv("OUTPUT_PATH"));
```

エラーになります

> error C4996: 'getenv': This function or variable may be unsafe. Consider using _dupenv_s instead. To disable deprecation, use _CRT_SECURE_NO_WARNINGS. See online help for details.

あんた `getenv` は非推奨だけど使いたいなら、`_CRT_SECURE_NO_WARNINGS` 定義しなさいよ！ってやつです  
ええ、使いたいんです、HackerRank にコードコピペしたいんで！  
なのでわたしはガッツリと先ほどの **stdc++.h** の上の方に定義を追記しています

自分はVSに慣れてるので、慣れた環境でコード書けるのが快適ですね


## (余談) VSCodeでやろうとしたが…

最初は、Windoows の VSCode で環境構築をしようとしてたのですが、なかなか手ごわいです…

mac はさくっとものの数分で C++デバッグ環境が構築できたのに  
Windows だと、半日かけてもうまく動きませんでした＞＜。  
（VC++ がうまく動かなくて、gcc はなんとか動作したって感じです）

色々調べていた時に、VSCode on Windows 情報でいちばん面白かったのが、この中国の投稿サイトでした

- Visual Studio Code はどのように C および C++ プログラムを作成して実行しますか?（翻訳） - zhihu.com  
[https://www.zhihu.com/question/30315894](https://www.zhihu.com/question/30315894)

わたしは中国語はよくわからないので、Googleのページ翻訳でよくここの Zhihu サイトを見るのですが  
この記事は見てて笑っちゃいました。。。  
情報量も多いし、皮肉アリ、ぶっちゃけトークありで、読み物として面白かったです

ここを見て、あ、VSCode on Windows でVC++使うのちょっと大変かも…と思いました  
中国は同じように感じがあるので、そこの悩みも詳細にかかれてて、親近感沸きました

あんまり日本でこういう記事見ないな～なんでだろ、ぶっちゃけすぎるからかな？

C++だけじゃなくてUnityの情報も多いです、知識投稿サイトってやつかなー？とにかく面白い

---

（まとめ）

コード書くなら開発環境は整えようね！

C++ で mac なら VSCode だけど、Win なら VS 使おうぜ！

HackerRank ほんとお勧め、地道に解くのが楽しい

---

（参考）

- Visual StudioでC++のbits/stdc++.hを使う  
[https://qiita.com/kokosan60/items/d24ab868f7090586b0bc](https://qiita.com/kokosan60/items/d24ab868f7090586b0bc)
