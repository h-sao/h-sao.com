+++
date = "2015-10-06T04:00:00+09:00"
draft = false
title = "[Android]Xiaomi Redmi Note2にGooglePlayをインストールする"
categories = ["Xiaomi", "Android"]
+++

クロスプラットフォームで開発していると、やはり最新の端末が欲しくなりますよね～

１年以内発売の iPhone と Windows Phone は持っているのですが  
そろそろ新しい Android が欲しい～と思っていたので、買ってしまいました

シンガポール旅行に行った際に、ブギスの電気屋街、[Sim Lim Square（シムリム スクウェア）](http://www.simlimsquare.com.sg/)で、今をトキメク Xiaomi（しゃおみ）デバイスを購入しました  

- **Xiaomi**  
[http://www.mi.com/](http://www.mi.com/)

![](/pic/InstallGooglePlayOnXiaomiRedmiNote2_00.JPG)


全部で6Fまでありましたが、結局1Fのお店の品ぞろえが一番まとまってる印象でした  
Xiaomi正規店も1Fに入っていました


----------
## Xiaomi Redmi Note 2 ##

購入したのは、**Xiaomi Redmi Note 2**  

<table style="border-style: none;">
 <tr>
   <td style="border-style: none;"><img src="/pic/InstallGooglePlayOnXiaomiRedmiNote2_01.JPG"></td>
   <td style="border-style: none;"><img src="/pic/InstallGooglePlayOnXiaomiRedmiNote2_02.JPG"></td>
 </tr>
</table>

画像のように、デフォルトでは中国語のアプリがたくさん入っています  
そして、Google Play Store は入っていません！

デフォルトのIMEが Buidu なので、まぁこのままでは、日本語が打てません(><;)

なので Google Play Store をインストールする方法をメモしておきます

----------
## GooglePlay installer APKが最適解 ##


検索すると色々な方法が出てくるのですが、おそらく、**Google installer のAPKを直接インストールする**のが、手っ取り早いです  
（チャイナストアやMIストアからGooglePlayらしきものをインストールしても、わたしの場合は、動作しませんでした）

情報元はこちらの Xiaomi公式コミュニティ掲示板です

- **[System Tools] Find all the Google Apps in Google installer!**　　
[http://en.miui.com/thread-3998-1-1.html](http://en.miui.com/thread-3998-1-1.html)

Xiaomiはすごくコミュニティも発達してるんですよね～ちゃんと英語の掲示板なので、読めました(^_^)

上記サイトにある、GooglePlayを入れるための Google installer をAPKからインストールします  

> Google installer谷歌应用下载器.apk  
> (1009.36 KB, Downloads: 237400) 
> ![](/pic/InstallGooglePlayOnXiaomiRedmiNote2_03.png)


PCから落とすなどするよりも、直接 Xiaomi デバイスからダウンロードした方が、早いと思います  
もしかしたら、MIアカウントを作らないとダウンロード出来ないかもしれません（わたしは作りました）

----------
## Androidがセキュア過ぎて、思った順番にインストール出来ない ##

installer を入れると、以下の４つがインストール可能になります

 1. Google Services Framework 
 1. Google Account Manager
 1. Google Play services
 1. Google Play Store

そのままインストールしたら良いので、おそらく、この順番でインストールが走ると思います  
（ここ、画面キャプチャを取り忘れたので申し訳ない）

ところが、1番目をインストールしようとすると、本当にインストールしていいかどうかの許可画面が出ますよね、こんな感じに…  
（イメージ画像です↓↓↓）

![](/pic/InstallGooglePlayOnXiaomiRedmiNote2_04.png)

この画面で 「1番のアプリに対して」ユーザが許可を押そうとするときに、すでに「2番目のインストール」が動いてしまうのです  

結果的に、 「1番のアプリの許可画面」は裏にまわってしまい  
「2番目のアプリの許可画面」が一番手前に来ることになります  
つまり最終的には、「4番目のアプリの許可画面」が最前面になります  

入れたいアプリは Google Play Store なので、それでも良いかなと思って  
４→３→２→１の順にインストールするんですけど、  
お察しの通り、Google Play Store は起動しません  
おそらく、Google Services Framework とかが、Google Play Store のインストールに必要なんじゃないかなーと思います

なので、いったん、Google Play services、Google Play Store をアンインストールし、再度、Google Play services、Google Play Store をインストールします

いやー(~0~)　これで、わたしの端末で、Google Play が動くようになりました！

![](/pic/InstallGooglePlayOnXiaomiRedmiNote2_05.JPG)

----------
## その他、Xiaomi デバイスで気づいたところ ##

#### 日本語モードはない

日本語モードは入っていません  
ROMとか焼き直すと良いらしいですけど、まだやってないです

#### Permissions アプリがうれしい

デフォルトで入っている Permittions アプリが最高に便利

<table style="border-style: none;">
 <tr>
   <td style="border-style: none;">各アプリ毎の許可</td>
   <td style="border-style: none;">どのアプリが使ってるか</td>
 </tr>
 <tr>
   <td style="border-style: none;"><img src="/pic/InstallGooglePlayOnXiaomiRedmiNote2_07.png"></td>
   <td style="border-style: none;"><img src="/pic/InstallGooglePlayOnXiaomiRedmiNote2_06.png"></td>
 </tr>
</table>

#### SIMは２枚ささる

両方LTEをつかめます  
ただし、片方がLTEをつかんでいるときは、もう一方はつかめない仕様です

なので、日本で、１番目にDocomoを刺して、２番目にauを刺す、とかして  
１番目はLTEでデータ通信、２番目はauで電話とメールのみ  
とかにしたくても、ちょっと難しいかも  
日本は今現在、ほぼLTEになってしまってるので、そういう活用は出来なさそうかも…？

どちらにしても、技適通ってない端末なので、日本では使っちゃダメ

#### まとめ

もろもろ満足しています  
同じようにシンガポールや中国で購入した方の参考になれば～

