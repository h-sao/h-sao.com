+++
date = "2015-08-25"
draft = false
title = "[Hugo/wercker]ブログを移行しました"
tags = ["Hugo", "wercker"]
+++

ライブドアで書いていたブログ [「遥佐保の技術メモ」 http://blog.livedoor.jp/haruka_sao/](http://blog.livedoor.jp/haruka_sao/) を移植することにしました  

思えばライブドアはソースコードを表示するのには適していないブログでした…  
2008年からなので、もう7年も!?  
かなり気づくのが遅かったですね…  

- Hugo - [https://gohugo.io/](https://gohugo.io/) 

もともと、**Sao`s Closed Circle** というGitHub Pages の練習用のブログを [JekyllBootstrap](http://jekyllbootstrap.com/) で作っていました  
今回からは、ツールも新たに [Hugo](https://gohugo.io/) を使うようにしました  

書くほどのお知らせじゃないですが、このブログはHugoという静的サイトジェネレーターで作っています、静的、つまりただのHTMLです  
自前運用です  
ここ5年くらい前？から流行っているらしいです  
最近は技術系以外でもよく使われているらしいです

どのジェネレーターが良いかと言うのは、よくわからないですけど  
[Jekyll](http://jekyllrb.com/)（Rubyベース）が有名なのかなーと思います

**Static Site Generators** まとめによるとJekyllはGithubスター数がトップですね  
（Static Site Generatorsは、ちまたの静的サイトジネェレータ一覧です）    
[https://staticsitegenerators.net/](https://staticsitegenerators.net/)  
こうしてみると、Hugoは後発ながら結構人気みたい

Jekyllはとても高機能で（イイコトなんですけど）  
でもわたし、Rubyがよくわからなくて（すまん…Web屋さんじゃないの…）  
そんなときにGoで作られた、ちょっぱや生成のHugoを知ってしまったら、もうJekyllには戻れなくなりました、haha...

Jekyllの方が事例も多いし、検索ですぐ解決できるけど、ちょっとジェネレートの時間がかかるのが弱点かも  
たいして記事数のないわたしでもそう思いましたから、記事数の多い人に取ったらちょっとイライラの原因だったかもしれませんね

Hugoは、機能あれもない、これもないの、(ヾﾉ･∀･`)ﾅｲﾅｲずくし  
けどそこが良い！シンプルで良いんです！(_≧Д≦)ﾉ彡☆

- wercker - [http://wercker.com/](http://wercker.com/) 

さらに、生成するだけでなく、CIサービスである [wercker](http://wercker.com/) を使って、GitHubにプッシュしたら自動でデプロイするという仕組みにしました  
これを構築したメモもおいおい記載していくつもりです  
（そうしないと自分が忘れてしまう…）

このブログ自体もまだまだ作り中で、色々と足りてない機能が多いのですが、勉強しながら追加していこうと思います

過去ブログの情報移植もこれからです  
（RSSが変に更新されるかもしれません）

気持ちも新たに、役立つ情報を発信していければと思っています！

またよろしくお願いしまーす！

