+++
date = "2017-01-04T23:00:00+09:00"
draft = false
title = "[Chrome] Google Chromeのマルチプロファイル設定"
tags = ["Chrome"]
+++


**Google Chrome** を使って、Webサイトを開発をしている人向けの情報です  

自分のChromeのユーザプロファイラを使わずに、開発は別のプロファイラを使いたいってこと、ありますよね…？

## [プランA] ユーザプロファイラを切り替える

Google Chrome のユーザデータを、開発用と自分個人用とに分けたい  
開発用のモバイルモードで見てるときに、普通にPCブラウジングがしたい

とかいうときのために（か、どうか分かりませんが…）

Google chrome は複数ユーザに対応しています


1. chrome://settings/ にアクセス  
もしくは、メニューの「設定」を押す

1. 「ユーザ」の設定項目があるので、追加します  
<img src="/pic/How-to-use-multiple-browser-profiles-in-chrome_02.png" style="border:solid 5px #e6e6e6"/> 

1. アイコンと名前を選択して「追加」します  
（わたしの場合は適当に Development と付けました）  
<img src="/pic/How-to-use-multiple-browser-profiles-in-chrome_03.png" style="border:solid 5px #e6e6e6"/> 

1. ユーザ名が右上に表示されるようになり、新しい「Development」ユーザでブラウザが立ち上がります  
<img src="/pic/How-to-use-multiple-browser-profiles-in-chrome_04.png" style="border:solid 5px #e6e6e6"/> 

Extension や閲覧履歴は、完全に今まで使っていたものとは別になっているので  
新規プロファイルが使い放題です(*ﾉωﾉ)

これまで使っていたユーザは「ユーザ－１」  
開発用のユーザは「Development」  
で、使い分けることが可能になりました

## [おまけ] 起動時に Development ユーザを指定して起動する方法

ちなみに、デフォルト起動はもちろん、これまで使っていた「ユーザー１」で立ち上がります  

ショートカットなどで、さっき作ったDevelopmentユーザがいきなり立ち上がって欲しいときは  
以下のように「**profile-directory**」起動オプションを設定してあげると  
任意のユーザプロファイルで立ち上がります

```
"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --profile-directory="Profile 2"
```

> ※Chromeのショートカット設定にしておくと便利です  
<img src="/pic/How-to-use-multiple-browser-profiles-in-chrome_05.png" style="border:solid 5px #e6e6e6"/> 


わたしの環境では、"Peofile 2"という名前でしたが、これは作った環境によって異なっているはずです

Chromeユーザプロファイルのデフォルトの位置はこちら↓↓↓

> **"C:\Users\[Winユーザ名]\AppData\Local\Google\Chrome\User Data\Profile x"**

ちなみに、これまで使っていた自分の環境である「ユーザー１」は「**Default**」というディレクトリ名です

> "C:\Users\[Winユーザ名]\AppData\Local\Google\Chrome\User Data\Default"

---

さて、プランAのプロファイル切り替えでも、まぁ用途は足りるんですけど  
1点だけ、気に入らないところがありました…

確かに、完全にプロファイルは別になるのですが  
**起動時の画面サイズは、プロファイル間で共有されてしまう**のです

これを解消したい…！  
それが次に示すプランBのパターンです

## [プランB] 稼働環境を別物にする

つまりプランAのままだと、

> 自分でブラウジングしてる「ユーザー１」の画面サイズと  
「Development」ユーザの画面サイズを、  
個別に保存することは出来ない

のです…  
あぁ残念 xxx

でも大丈夫(*ﾉωﾉ)

方法がありました  

プロファイルの保存場所そのものを、別のディレクトリにすればOKでした

指定するオプションは「**user-data-dir**」です

```
"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --user-data-dir="C:\utils\chrome_dev_profile"
```

> ※同じく、Chromeのショートカット設定の図、先ほどのプランAの profile-directory オプションは不要  
<img src="/pic/How-to-use-multiple-browser-profiles-in-chrome_06.png" style="border:solid 5px #e6e6e6"/> 


このオプションで指定するディレクトリは、どこでもいいみたいですが  
出来ればデフォルトで利用している

> C:\Users\[Winユーザ名]\AppData\Local\Google\Chrome\User Data

の配下は、避けた方がいいかもしれません  
（わたしの環境では、スタートページがちょっと変になりました。深追いして調べてないです…）

---

プランA/Bのどちらでも、マルチプロファイラでブラウジングできるのですが  
より独立した設定をしたい！という人には、プランBが私的にはお勧めです

Chrome は起動オプションがいっぱいあって、他にも  
Userエージェントを指定出来たり（**user-agent**）、  
ブラウザの言語を指定したり（**lang=en-US**で英語になる）と  
いろんな機能が用意されてるので  
開発用途としてはなかなかいい感じだなと、今更ながら思いました(*´∇`)ﾉ

---

＜参考＞

ちなみに、Google Chrome の起動オプションは  
公式ではちょっと探しにくかったんですけど（わたしの探し方が悪いかも…）  
こちらの人のサイトが、簡潔で一覧性が高く、参考になりました(^▽^)/

- List of Chromium Command Line Switches - Peter Beverloo（英語）  
[http://peter.sh/experiments/chromium-command-line-switches/](http://peter.sh/experiments/chromium-command-line-switches/)

以下、日本語の参考サイト

- 完全Google Chrome マルチユーザマニュアル　永久保存版 - This is Shinobu's Memorandum  
[http://saske18jp.blogspot.jp/2012/05/google-chrome.html](http://saske18jp.blogspot.jp/2012/05/google-chrome.html)

- Google Chrome を別設定で複数起動させる方法 - digitalbox  
[http://digitalbox.jp/google-chrome-multiple-invoking/](http://digitalbox.jp/google-chrome-multiple-invoking/)
