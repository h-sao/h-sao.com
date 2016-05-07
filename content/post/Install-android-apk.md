+++
date = "2015-03-13T22:00:51+09:00"
draft = false
title = "[Android] 開発者向けオプション表示とAPKインストールコマンド"
tags = ["Android"]
+++

ずいぶん昔から出ている情報ですが、メモっときます<br /><br />Android端末に自作アプリを入れるシンプルな方法です<br />

# Android端末での準備

Android4.2以降の端末だと「開発者向けオプション」が非表示になってるので<br /><br />USBデバッグを有効にしたい場合は<br /><blockquote>端末情報&nbsp;＞ ビルド番号</blockquote><br />このビルド番号を7回クリックしたら、「開発者向けオプション」が表示されます<br /><br />やっておくことは<br /><ul><li>設定 ＞ セキュリティ ＞ 提供元不明のアプリ　ON</li><li>設定 ＞ 開発者向けオプション ＞ USBデバッグ　ON</li><li>デバイスを提供している会社のサイトから、Android用SDKドライバをインストールする</li></ul>こんなところでOK<br /><br />（参考）<br /><div><a  href="http://www.yahoo-help.jp/app/answers/detail/p/601/a_id/54447/~/usb%E3%83%87%E3%83%90%E3%83%83%E3%82%B0%E3%82%92%E6%9C%89%E5%8A%B9%E3%81%AB%E3%81%99%E3%82%8B%E3%80%81%E6%8F%90%E4%BE%9B%E5%85%83%E4%B8%8D%E6%98%8E%E3%82%A2%E3%83%97%E3%83%AA%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%82%92%E8%A8%B1%E5%8F%AF%E3%81%99%E3%82%8B" target="_blank">USBデバッグを有効にする、提供元不明アプリのインストールを許可する - Yahoo!スマホマネージャーヘルプ&nbsp;</a></div><br />

# Android端末にAPKをインストール

AndroidSDKをインストールした場所に adb tool が入っています<br /><br />わたしの場合は&nbsp;C:\Library\android-sdk にインストールしたので、adb.exe は

> C:\Library\android-sdk\platform-tools\adb.exe

よく他のサイトで、PCの環境変数パスに、C:\Library\android-sdk\platform-tools を追加設定する、というのを見かけますが<br />単にこの abd.exe を使うだけであれば、環境変数への追加は必要ないです<br /><br />UBSでAndroid端末をPCに接続した状態で<br />手元の MyApp.apk をインストールするコマンドはこちら

```xml
 adb install -r MyApp.apk
```

さくっとインストールできました<br /><br />デバイスからAPKリンクをつつくなどでも良いのですが<br />開発者っぽい感じかつ、<br />ちょっと手元でAPKを試したい時の参考までに(^^)/

