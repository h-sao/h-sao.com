+++
date = "2016-12-21T18:00:00+09:00"
draft = false
title = "[Outlook] 使わなくなったOutlook on the webの予定表（カレンダー）を削除する方法"
tags = ["Outlook"]
+++

Office365とかの Outlook カレンダーではなく  
Webベースでアクセスできる **Outlook on the web** について  
データの削除がとてもややこしかったので、ちょっとメモしておきたいと思います

## かつての Windows Liveカレンダー

昔は、Windows Liveカレンダーという名称で、カレンダーサービスがあったのですが  
最近になり、Outlook on the web というサービス名に変わった様です

[https://calendar.live.com/calendar/calendar.aspx](https://calendar.live.com/calendar/calendar.aspx)

それに伴い、ちょっとずつ使い勝手も変わってきたりして…

わたし自身が  
Googleにもカレンダーがあり、サイボウズにもあり…カレンダーだらけだったので  
ちょっと過去データを整理しようと思ったのが、このブログのきっかけです

以降、「予定表」とあるのは、カレンダーの事を指しています  
マイクロソフトさんの用語で統一して書きたいと思います

## 問題点が２つ起きた

1. Outlook on the web のメインの予定表が消せない
1. Windows10 カレンダーアプリの情報が同期されない（消せない）

今日はこれを解決したいと思います…

-----
-----

## ＜問題1＞不要な予定表を削除したいが、メインは消せない

実際に、オフィシャルサイトには、予定表の削除の方法は書いていないのです…

- Outlook on the web で予定表を使用する  
 [https://support.office.com/ja-jp/article/Outlook-on-the-web-で予定表を使用する-8cfd8a5e-de9a-4468-9225-8611c8c8c7b0](https://support.office.com/ja-jp/article/Outlook-on-the-web-%E3%81%A7%E4%BA%88%E5%AE%9A%E8%A1%A8%E3%82%92%E4%BD%BF%E7%94%A8%E3%81%99%E3%82%8B-8cfd8a5e-de9a-4468-9225-8611c8c8c7b0)

しかし、Outlook on the web の方で、予定表を削除することは出来るようです  
該当する予定表の上で、右クリックしたら、"削除" が出てきます  
↓↓↓

<img src="/pic/Delete-Outlook-on-the-web-data_00.png" style="border:solid 5px #e6e6e6"/>  

しかし、メインの予定表は消せないみたいです  
（削除が出てこない）  
↓↓↓

<img src="/pic/Delete-Outlook-on-the-web-data_01.png" style="border:solid 5px #e6e6e6"/>  

とにかく情報を整理したいだけなのに…(+o+)

## ＜問題2＞Windows10 カレンダーアプリの情報は削除されない

メイン以外の予定表は消せるので、Outlook on the web上で操作するんですけど  
クライアントアプリである、Windows10 付随のカレンダーには、一向に反映されません

あれ、消したはずの予定表がいつまで経っても消えない…  
↓↓↓

<img src="/pic/Delete-Outlook-on-the-web-data_02.png" style="border:solid 5px #e6e6e6"/>  

-----
-----

## ＜解決方法＞Web上では無理、Outlookクライアントを使う


Outlook on the web だけでは無理なのが良く判ったので  
**Outlookのクライアントメールソフト** を使ってみました

Officeで使えるPC上にインストールするあれです

1. まず、メニュー > ファイル > 情報　に行きます  
<img src="/pic/Delete-Outlook-on-the-web-data_03.png" style="border:solid 5px #e6e6e6"/> 

1. 「アカウント情報」の下の「アカウントの追加」で、問題となっているマイクロソフトアカウント
（旧Windows Live カレンダーアカウント）を追加します  
<img src="/pic/Delete-Outlook-on-the-web-data_04.png" style="border:solid 5px #e6e6e6"/> 


1. 問題となっている登録したアカウントを選んだ状態で、
Outlookアプリの左下に表示されている（と思う）「…」を選ぶと、「フォルダー」という項目が選択できるので、
それを選びます  
<img src="/pic/Delete-Outlook-on-the-web-data_05.png" style="border:solid 5px #e6e6e6"/> 

1. なんと！？削除済みアイテムの中に、さっき消した予定表が入っていました！  
さくっと「フォルダーを空にする」を選びましょう  
（その画像は取り忘れちゃいましたが…操作する箇所はこれです）  
<img src="/pic/Delete-Outlook-on-the-web-data_06.png" style="border:solid 5px #e6e6e6"/> 

1. これで、Windows10のカレンダーアプリの情報も更新され、削除した予定表は消えました…！やったー！  
※ただし、即時反映ってん訳じゃなさそうでした、しばらく時間がかかるっぽいです
![](/pic/Delete-Outlook-on-the-web-data_07.png)

これで ＜問題2＞のほうは解決しました  
あとは、＜問題1＞のメイン予定表が消せない件ですね…

-----

メイン予定表は削除できない、なら、せめて予定表に入っている項目を全削除してやる！というのが、以下の試みです

1. 引き続き、Outlookクライアントを使います  
アカウントの「予定表」を選択した状態で  
ビューの変更 > 一覧  を選択します  
<img src="/pic/Delete-Outlook-on-the-web-data_08.png" style="border:solid 5px #e6e6e6"/> 

1. これまで登録している予定の一覧が表示されます  
どれか選択した状態で、「Ctrl + A」で予定を全選択します  
<img src="/pic/Delete-Outlook-on-the-web-data_10.png" style="border:solid 5px #e6e6e6"/> 

1. あとは「Delete」を押して、全選択すれば、  
内容をとにかく消すことには成功します  
（予定表そのものは消せない様ですが）  
消す前に、一応以下の様な注意が出るので、思い切って「はい」をぽちっとな！  
<img src="/pic/Delete-Outlook-on-the-web-data_11.png" style="border:solid 5px #e6e6e6"/> 

…過去に登録した項目は、メインの予定表からすべて消えました(●´ω｀●)  
いったんは、これで良しとしますかね…（´-`）.｡oO

-----

## ＜後片付け＞Outlookクライアントからアカウントを削除

Outlook on the webの予定表を消すためだけに  
Outlookクライアントにアカウントを登録したので、これを削除する画面キャプチャもついでに載せておきます

1. 再び、メニュー > ファイル > 情報　に行き、「アカウントの設定」を選びます  
<img src="/pic/Delete-Outlook-on-the-web-data_12.png" style="border:solid 5px #e6e6e6"/> 

1. 先ほど登録した Outlook on the web 用のアカウントを選択し、「削除」を押します  
<img src="/pic/Delete-Outlook-on-the-web-data_13.png" style="border:solid 5px #e6e6e6"/> 

これで後片付けも完了です

今回紹介した方法が、良いやり方かどうかは判りませんが  
似たような事で困っている人の助けになればと思います～(^^♪


（参考サイト）

- 【暫定解決策】Outlook.comのWebアプリで削除した予定表がアプリに残った時はPCのOutlookで「削除済みアイテム」を確認  
[http://blog.bari-ikutsu.com/entry/20151213_9138.html](http://blog.bari-ikutsu.com/entry/20151213_9138.html)


