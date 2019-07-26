+++
date = "2019-07-26T17:00:00+09:00"
draft = false
title = "[VisualStudio] Visual Studioで使う改行コードを指定する方法"
tags = ["Unity", "VisualStudio"]
+++

環境

- WIndows10
- Visual Studio 2019
- Unity 2018


元々これを調べたきっかけは、Unityで作ったプロジェクトファイルを開いたときに  
**改行コード** に関する警告が必ず出ていたからです


> There are inconsistent line endings in the 'Assets/Script/xxxxx.cs' script. Some are Mac OS X (UNIX) and some are Windows.

Windowsで作業してたとしても、Unityのプロジェクトはで作られたソースコードは  
改行コードが **LF** なんですよね  
Visual Studio のデフォルトは **CR+LF**

それで改行コードが混じってますよ～とVSが警告出してくれてたんですね。。。

プロジェクト内では、Windows と macOC が入り乱れている状態なので  
**CR+LF** に統一も出来ず  
**LF**に統一するしかない…

そんなときは、`.editorcongfig` ファイルを作成するのがおススメです！


### .editorcongfig ファイル

```
[*]
end_of_line = lf
charset = utf-8-bom
```

> `end_of_line` で改行コードを LF 指定にしています  
> あとUnityで日本語を扱うなら、Bom付きのUTF-8 が良いらしい（？）

この`.editorcongfig` ファイルをUnityプロジェクト直下に置けばOK

Visual Stusio で編集したコードでも、LFの改行コードとなりました

警告無く作業が出来るようになって、小さなストレスから解放♪


- （参考）Unityと改行コード - エフアンダーバー 個人開発の記録  
[https://www.f-sp.com/entry/2017/04/06/023709](https://www.f-sp.com/entry/2017/04/06/023709)

