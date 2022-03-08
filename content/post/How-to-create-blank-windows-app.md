+++
date = "2022-03-08T15:30:00+09:00"
draft = false
title = "[C++] Windowsの空のアプリケーションを作る手順メモ"
tags = ["cpp", "Windows"]
+++

いつもどうやったっけな？と思って同じ過ちを繰り返してるのでメモ

環境:  
Visual Studio 2022

作りたいソリューション

- C++プロジェクト
- ブランクプロジェクト
- Windowsデスクトップアプリケーション（コンソールではない！）



いつもわたし、空のプロジェクトを作りたいからといって  
`Empty Project` を選んでしまうのです…  
すると、コンソールベースの exe を作るソリューションになってしまいます  
ちゃうねん、これちゃうねん！

---

Windowアプリの空のソリューションを作りたい場合はこれ

1. `Create a new project`
1. `Windows Desktop Wizard` を選択
1. `Configure your new project` で名前を入れて次に進む
1. `Windows Desktop Project` で `Desktop Application (.exe)` を選択
1. 更に `Empty project` にチェックを入れる
1. これで `OK` を押せば、よし！

![](/pic/How-to-create-blank-windows-app_01.png)

これこれ、、、これやねん。。。