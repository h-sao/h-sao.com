+++
date = "2015-09-09T01:11:00+09:00"
draft = false
title = "[Hugo]ブログ構築メモ(1)-Hugoとテンプレート"
categories = ["Hugo"]

+++

Windows10のノートPCに [Hugo](http://gohugo.io/) というスタティックジェネレーターをインストールして、このブログを構築しています  
作ったブログは、[GitHub Pages](https://github.com/h-sao/Blog) にアップして運用しています

今のところ気に入っているのは

> 1. ブログの管理が GitHubで行えること
> 1. ブログ用の運用サーバーが不要なこと (ありがとう！GitHub)
> 1. GitHubにPushしたら、werckerの自動ビルドでGitHubPagesにアップすること

この3点かなー  
今日は上から2番目までのブログ構築部分（つまりHugoの部分）をメモしておきます


----------
##  インストール (というか単なるexeファイル設置) ##

- 最新バージョン取得  
[https://github.com/spf13/hugo/releases](https://github.com/spf13/hugo/releases)

わたしが 2015/9/9 現在ダウンロードしたバージョンは **v0.14** です  
Hugoはexeだけで動くので、適当なディレクトリに置きます  
わたしは
  
    C:\Library\Hugo\bin\hugo.exe

に設置することにしました


----------

## ブログ新規作成 ##

Windows標準のコマンドプロンプトで作業します  
このとき既に、ブログを作ろうとするディレクトリが存在していたら、ブログ作成は失敗するので、気を付けてください  

> FATAL: 2015/09/09 C:\Users\haruk\Dropbox\sites\Blog already exists and is not empty

リネームなどして、再チャレンジ  
うまくいくと、何の反応もなく終わります

    C:\Users\haruk\Dropbox\sites>c:\Library\Hugo\bin\hugo.exe new site Blog
    
    C:\Users\haruk\Dropbox\sites>

ディレクトリ構造の確認

    C:\Users\haruk\Dropbox\sites>cd Blog
    C:\Users\haruk\Dropbox\sites\Blog>dir
     ドライブ C のボリューム ラベルは MyWindows です
     ボリューム シリアル番号は xxxx-xxxx です
    
     C:\Users\haruk\Dropbox\sites\Blog のディレクトリ
    
     2015/09/09  20:29<DIR>  .
     2015/09/09  20:29<DIR>  ..
     2015/09/09  20:28   270 .gitignore
     2015/09/09  20:28<DIR>  archetypes
     2015/09/09  20:28   107 config.toml
     2015/09/09  20:28<DIR>  content
     2015/09/09  20:28<DIR>  data
     2015/09/09  20:28<DIR>  layouts
     2015/09/09  20:2836 README.md
     2015/09/09  20:28<DIR>  static
       3 個のファイル 413 バイト
       7 個のディレクトリ  80,351,985,664 バイトの空き領域
    
    C:\Users\haruk\Dropbox\sites\Blog>
        
あっという間に自作ブログのベースは完了です  
次はテンプレートを落としてきます


----------
## テンプレート適用 ##

GitHubから有志の人たちのテンプレートを拝借してきます  

- Hugo Themes Repo  
[https://github.com/spf13/hugoThemes](https://github.com/spf13/hugoThemes)

たくさんテーマがあるけど、~~まじでうざいテーマしかない~~  
おっと失礼しました  
自己主張が激しいテーマが多いです  

とりあえず、GitHubのスター数が多いテンプレートをチェックしてみました  
中でも一番自分が気に入ったものが、**hyde-x**

- zyro/hyde-x  
[https://github.com/zyro/hyde-x](https://github.com/zyro/hyde-x)

テンプレートは自分のサイトの顔になるので、色々悩みましたが  
結局はデザインよりも機能が一番多いのがいいんでない？という発想で選びました

コンフィグファイルに
 
- GoogleAnalytics
- GitHub
- Bitbucket
- LinkedIn
- Facebook
- Twitter
- GooglePlus
- Gravatar

が揃っていたので、自分であれこれする手間も省けるかなーという印象  

GooglePlusはもう今ではダサいんじゃね？  
というアドバイスを受けたので有効にしていません＼(^o^)／  
同様、Gravatarもいらんやろ…と思いましたが、アイコン代わりに仕方なく使いました  

> Gravaterは自分でハッシュ作成をしないといけなかった  
> とゆーか、こんな変換くらいサイトで提供してよ…
> イマドキプログラマに変なところで手間かけさすなよなーいやまじで('ε')  
> [https://ja.gravatar.com/site/implement/hash/](https://ja.gravatar.com/site/implement/hash/)

話が多少逸れましたが、この hyde-x をテーマとして、Gitコマンドを使って適用します

> わたしは、SourceTree内蔵のGitを使ってみました  
> %USERPROFILE%\AppData\Local\Atlassian\SourceTree\git_local\bin  
> (参考)[https://answers.atlassian.com/questions/245850/how-to-run-a-git-command-as-a-custom-action](https://answers.atlassian.com/questions/245850/how-to-run-a-git-command-as-a-custom-action)
>
> "c:\Program Files (x86)\Git\bin\git.exe　とか持っているなら、それでも可

themes ディレクトリの下で作業します

    C:\Users\haruk\Dropbox\sites\Blog\themes>C:\Users\haruk\AppData\Local\Atlassian\SourceTree\git_local\bin\git.exe clone https://github.com/zyro/hyde-x
    Cloning into 'hyde-x'...
    remote: Counting objects: 358, done.
    remote: Total 358 (delta 0), reused 0 (delta 0), pack-reused 358
    Receiving objects: 100% (358/358), 253.37 KiB | 175.00 KiB/s, done.
    Resolving deltas: 100% (153/153), done.
    Checking connectivity... done.
    
    C:\Users\haruk\Dropbox\sites\Blog\themes>
    
**C:\Users\haruk\Dropbox\sites\Blog\config.toml** をhyde-x用に修正しましょう  
[hyde-xテーマのサイト](https://github.com/zyro/hyde-x)に解説が書いてあるので、それをそのままコピペします  
タイトルやTwitterアカウントなど、自分に必要な情報を更新します

他のジェネレータも同様ですが、  
コンフィグは結構、個別テーマに依存した項目になっているので（元のHugoの機能が少ない）別テーマでは使えないかもしれない点は注意です


----------

## ブログ投稿 ##

contentの 下に postディレクトリを作り、ブログ更新するMDファイルを置きます

    C:\Users\haruk\Dropbox\sites\Blog\content\post

わたしの場合、hyde-x が post の下、という仕様だったからであって  
他のテンプレでは違うかもしれません  
とにかく、ここの post の下に、以下のマークダウンファイルを用意します

**<20150909-HugoInstallAndTheme.md>**

```
+++
date = "2015-09-09T01:11:00+09:00"
draft = false
title = "[Hugo]Hugoブログ構築メモ-WindowsインストールとHugoテンプレート"
categories = ["Hugo"]

+++

Windows10のノートPCに [Hugo](http://gohugo.io/) というスタティックジェネレーターをインストールして、このブログを構築しています  
...（以下略）

```


ルート C:\Users\haruk\Dropbox\sites\Blog\　に移動し、Hugoコマンドを実行します

    C:\Users\haruk\Dropbox\sites\Blog\themes>cd ..
    
    C:\Users\haruk\Dropbox\sites\Blog>c:\Library\Hugo\bin\hugo.exe server -w
    0 draft content
    0 future content
    5 pages created
    1 paginator pages created
    5 categories created
    in 201 ms
    Watching for changes in C:\Users\haruk\Dropbox\sites\Blog/{data,content,layouts,static,themes\hyde-x}
    Serving pages from C:\Users\haruk\Dropbox\sites\Blog\public
    Web Server is available at http://127.0.0.1:1313/
    Press Ctrl+C to stop
    
ブラウザで http://127.0.0.1:1313/ にアクセスすると、サイトが出来ています  
実際には、public 以下にHTMLがジェネレートされているので、public以下を自分の GitHub の gh-pages ブランチにアップすると、自前ブログの完成(^^)

これも余談ですが、CNAMEファイルを置くと、独自ドメインが使えますね　　
（これ、ファイル名が大文字じゃないとダメでした）

- わたしのgh-pages の参考です  
[https://github.com/h-sao/Blog/tree/gh-pages](https://github.com/h-sao/Blog/tree/gh-pages)


----------

## 簡単に構築は出来たが… ##

ここまでだと結構簡単にできますが、ここから先は、自分で頑張るしかない部分です…

何がって？  
結局最初に諦めた、デザインのことと  
CMSに当たり前にあって、Hugoには無い機能を、実装していく必要があります

今のこのブログは、先ほどの hyde-x テンプレートを改変しまくっています  
とはいえ、トライ＆エラーの繰り返しだったので、解説まではできないかも…  
テンプレートやHugo本体の解説は、また気が向いたときにするということで許してください
  
それよりは次回は、CIツールである wercker の設定をメモします  
（次へ続く）