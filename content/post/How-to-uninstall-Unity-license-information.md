+++
date = "2017-11-02T09:00:00+09:00"
draft = false
title = "[Unity] Unityのライセンス情報をアンインストールしたい"
tags = ["Unity"]
+++

Unityをアンインストールしたけど、あれ？ライセンス情報が残ってる(?_?)  
というのが調べたきっかけです

PCからUnityの情報を完全消去したいときの参考に

## ライセンス情報 (Unity5.x系)

ライセンスファイルは、Unityをアンインストールしても消えません＞＜

以下にライセンスファイル(.ulfファイル)が残っています

- C:/ProgramData/Unity/Unity_v5.x.ulf
- C:/Users/(username)/AppData/Local/VirtualStore/ProgramData/Unity/    
（UACで C:/ProgramData/Unity/ へアクセス制限されてる時）

で…

確かに上記を消したら、アクティベーション情報は消えるのですが  
Unityにログインしたユーザ情報は残ってる様です

キャッシュ的なものも消さないと、完全アンインストールにはならないみたい

## その他、アンインストールで消えないディレクトリ

- C:/Users/(username)/AppData/Local/Unity
- C:/Users/(username)/AppData/Roaming/Unity  
上記２点には、エディタに関する何か？キャッシュ的な何か？が残っています  
起動した際の、Unityプロジェクトの表示とか…  
おそらくこの２つのディレクトリ以下に、ユーザ情報も入ってるみたいです

- C:/Users/(username)/AppData/RoamingLocalLow/Unity  
この下もちょっとよく判らないですが、  
このディレクトリ配下の「Asset Store-5.x」の下には  
アセットストアからダウンロードしてきたパッケージファイルが入っていました  
（.unitypackage）



## まとめ

逆の言い方をすると、  
上に書いたディレクトリ４つ

- C:/ProgramData/Unity/
- C:/Users/(username)/AppData/Local/Unity
- C:/Users/(username)/AppData/Roaming/Unity 
- C:/Users/(username)/AppData/RoamingLocalLow/Unity

を消せば、Unityをアンインストールしなくても  
ライセンス情報のリセット、ユーザ情報のリセットが可能でした

> もちろん  
> C:/Users/(username)/AppData/RoamingLocalLow/Unity/Asset Store-5.x  
> の下は、ライセンスファイルとは無関係みたいです

## Unity2017系は？

Unity2017 では、ライセンスディレクトリは同様で、ライセンスファイル名が

- Unity_lic.ulf

になってるみたいです  
（消さないといけないディレクトリまでは、2017系では調べてないので、ごめんなさい）



参考：

- アクティベーションに関する FAQ - Unity公式  
[https://docs.unity3d.com/ja/current/Manual/ActivationFAQ.html](https://docs.unity3d.com/ja/current/Manual/ActivationFAQ.html)(日本語)  
[https://docs.unity3d.com/Manual/ActivationFAQ.html](https://docs.unity3d.com/Manual/ActivationFAQ.html)(英語)

- Unityの再インストール - FreelyApps  
[http://freelyapps.blog.jp/archives/1032543378.html](http://freelyapps.blog.jp/archives/1032543378.html)
