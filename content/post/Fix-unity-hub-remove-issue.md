+++
date = "2025-11-25T17:21:00+09:00"
draft = false
title = "[Unity] Unity Hubでremoveできないバージョンの削除方法"
tags = ["Unity"]
+++

Unity Hub からUnityの色んなバージョンをインストールしていると思うのですが  
たまに「Unityを手動インストールしてから、UnityHubに認識させる」  
というのをやるときがあると思います

ないよーと思うかもしれませんが、この記事はそれがある人のための手順です


## エンジンバージョンの歯車メニューの項目

UnityHubのInstallsの中には、複数のUnityバージョンが存在できます

UnityHubからUnityをインストールした場合  
歯車マークの `Manage` を押すと、こんな項目が出てきます

![](/pic/Fix-unity-hub-remove-issue_00.png)


けどもし、手動インストールしたものをUnityHubに認識させていたら、こんな項目になります

![](/pic/Fix-unity-hub-remove-issue_01.png)

手動インストールしたら、UnityHubから `Add modules` 出来ないんですよね

手動インストールしてしまったけど  
UnityHubで管理したらよかった、モジュールもここから追加したいし  
いったん手動インストールのバージョンは削除しよう！

と思って `Remove From Hub` を押下するのですが  
何も起こらないです

あれ？どういうこと？

## 手動でUnityをアンインストールする

手動で入れたので、手動で削除してみます

さっきの歯車メニューで `Show in Exproler` を押すと、Unityの対象バージョンがインストールされているディレクトリが開きます

その中に、`Uninstall.exe` があるので、それを起動して  
いったん削除したいUnityバージョンをアンインストールします

<img src="/pic/Fix-unity-hub-remove-issue_02.png" style="border:solid 5px #e6e6e6"/>

その後、UnityHubに戻っても、まだ消えていないので、PCを再起動しましょう！

そしたら、消したかったUnityバージョンがUnityHubから消えています！やった！

## UnityHubでインストールする

インストールしたいバージョンが `Official releases` にあればいいですけど  
ない場合は、 `Aechive` から入れるようにします

[https://unity.com/releases/editor/archive](https://unity.com/releases/editor/archive)

このページの `All versions` に入れたいバージョンがすんなりあればいいけど、ない時もあります

そんなときは、URLを推測して書いてみると、あったりします  
（それを使っていいかどうかはよく判らないですが、アクセスしてみたらありました）

とにかく、対象のバージョンを見つけたら、`See all` のリンクをクリックして対象バージョンのページに行きます  
上のほうにある `→ INSTALL`  を選びましょう

<img src="/pic/Fix-unity-hub-remove-issue_03.png" style="border:solid 5px #e6e6e6"/>

`Manual installs` の項目から exe などをダウンロードしてしまわないように気を付けてください


これで一件落着です

`Remove from Hub` が効かなくて焦ったので、よかった～