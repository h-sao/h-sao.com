+++
date = "2015-12-13T23:30:00+09:00"
draft = false
title = "[Hugo] ブログ構築メモ(2)-テンプレートの更新方法（CSSはメインではない）"
tags = ["Hugo"]
+++

前回、スタティックジェネレーターのHugoを使って、[GitHub Pages](https://github.com/h-sao/Blog/tree/gh-pages) にブログを展開する方法を記載しました

- [Hugo]ブログ構築メモ(1)-Hugoとテンプレート  
[http://h-sao.com/blog/2015/09/09/HugoAndTheme/](http://h-sao.com/blog/2015/09/09/hugoandtheme/)

このとき、「デザインと機能を諦めた」と書きましたが  
その続きをひそかにポチポチとやっておりました…  
しかし、ポチポチやる日々が辛くなり、ついにはブログ更新そのものが怠ることに…


# 一向に進まない理由

一人ではWebのフロントエンドのノウハウもなく、もう無理かもーと思っていたことを  
[@mayuki](http://www.misuzilla.org/) さんに相談したところ、こんなアドバイスを頂きました


>> CSSってイマドキ、モバイル対応などで複雑なので、デザインを一から自分で書き起こすのは辞めた方がいい  
いつまでたっても、モノが出来上がらない

>> "CSSデザイン" を触るのは最後  
先に "テンプレート" を駆使して、自分なりの配置にするのがおススメ

つまり  
**機能のテンプレートは他から持ってこれるがデザインはあらかじめ好みのものを選ぶ方が良い**  
というアドバイスでした

あれ？前回わたしが書いた時には、

>> 結局はデザインよりも機能が一番多いのがいいんでない？という発想で選びました

おお、まったく逆ですね…  
デザインさておき、機能重視してたので、うまくいかなかったのか…！！！

# 好みっぽく、かつシンプルなテンプレート

とまぁ、素人くさい話で本当に恐縮ですが、気持ち改め、シンプルで改変しやすそうなデザインのテンプレートを引っ張ってきました

- **minimalist**  
[https://github.com/digitalcraftsman/hugo-minimalist-theme/](https://github.com/digitalcraftsman/hugo-minimalist-theme/)

![](/pic/Change-Theme-To-Minimalist0.png)


これは相談したその場で[@mayuki](http://www.misuzilla.org/) さんが、ちゃちゃっとテストするために触ってくれたテンプレートです

彼はものの10分程度で、わたしのやりたいことを目の前でやってくれたので、とても参考になりました  
ありがとうございます～（涙目）

## 最初に見る場所はHTMLテンプレート

やったことはまず、**themes > minimalist > layouts** の下を見るようにしました  
わたしの環境だと C:\Users\haruk\Dropbox\sites\Blog\themes\minimalist\layouts になります

ここから必要そうな機能の、HTMLテンプレートを作っていきます  
わたしの場合、以下の機能（テンプレートとなるブロックと名付けます）が不足していました

- ヘッダーに "About me" などのリンクをつけたい
- 最新ポストの表示
- タグ表示

こんな感じです

## ヘッダーに直接何か書いてみる

**themes > minimalist > layouts > partials > header.html** がヘッダーに相当します

素人のわたしが混乱する原因として

- Hugoの機能がわからない
- テンプレートの更新方法がわからない
- CSSの更新方法がわからない

こんな感じでしょうか(+_+)

まぁHTMLタグの直書きであれば、なんとかなるだろうの勢いで、まずは、提供されている minimalist テンプレート（HTML）の中身を更新しちゃうことにします  

 > Hugo的作法としては、管理ディレクトリ直下の **layouts** を更新すべきらしいのですが、色々やっているうちに訳が分からなくなるので、やめました…  
 > もしかしたらHugoはファイル管理とか命名規則が、えいやっっっなのかもしれない？  
 > とにかく（わたしの能力不足なんでしょうけど）思うように管理できなかった


オレオレ topbar-layer classの追加  

```html
<header class="site-header">
  <div class="transparent-layer">
    <h2>{{ .Site.Params.header_title }}</h2>
  </div>
</header>
  <div class="topbar-layer">
    <a href="{{ .Site.BaseURL }}about">About me</a>&nbsp;&nbsp;&nbsp;
    <a href="{{ .Site.BaseURL }}tags">Tags</a>
 </div>
```

出力結果
![](/pic/Change-Theme-To-Minimalist1.png)

ヘッダーと本文の間に、バーを入れることが出来ました

ちなみに Hugo は、デフォルトで **[サイトURL]/tags/** というタグ一覧のHTMLを生成してくれます


## ブロックとして設置出来た後に、CSSを触る

勘所のある人には、こいつはなんというツマラナイことを書いてるんだ！？と思われるかもしれませんが、これも経験なんで許してください…書いててつらい…

気を取り直して、いよいよCSSを触ります

自分の追加した "topbar-layer" を追加します  
といっても、同じヘッダの "transparent-layer" をコピペする気まんまんです  

幸いなことに、この minimalist のテンプレートはとてもシンプルで、触るファイルは１つのみ！
**themes > minimalist > static > css > styles.css** に、"transparent-layer" のパクリ設定を追加します

```css
.topbar-layer {
  width: 100%;
  height: 100%;
  background-color: #252a2c;

  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  color: #e0e0e0;
  text-align: center;
  position: relative;
  line-height: 50px;
  height: 50px;
  transition: all 0.3s ease;
}

.topbar-layer a{
    color: #a2a2a2;
}
.topbar-layer a:hover {
    color: #fff;
}
```

出力結果
![](/pic/Change-Theme-To-Minimalist2.png)

高さなど一部、自分で調整  
これくらいならできた…

# (私的作業コスト比)デザイン調整＞テンプレート調整

わたしがプログラム目線だからかもしれませんが、実際このブログでは、出来てなかった機能（タグと最新ポスト表示）を難なく実装することは出来ました

Hugoが高機能ということ、公式ドキュメントがそこそこ充実してることなど、調べればなんとかなるもんです  

- タグなどのソートの方法  
[https://gohugo.io/templates/list/](https://gohugo.io/templates/list/)

まぁ実は、最新投稿表示の際には、表示させたくない静的なサイト（About me ページなど) をはじいたりするために  
ちょっと工夫が必要だったのですが、その話はまた今度…

とにかく、デザインをいじるのは難しい…！  
テンプレートを触ることに特化し、デザインは最後の最後にちょっとだけ手を入れる  
という方針が吉でした  

[@mayuki](http://www.misuzilla.org/) さん、アドバイスほんとにありがとーヽ(｀▽´)/

引き続きがんばります…！

また、RSSフィードが乱れるかもしれませんが、ご容赦くださいませ…

（次回へ続く）

