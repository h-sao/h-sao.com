+++
date = "2016-11-15T19:00:00+09:00"
draft = false
title = "[Maya] Maya2015設定ウィンドウの開き方"
tags = ["Maya"]
+++

Mayaはいろんな機能がありますね…  
設定の開き方すら判らないという恥ずかしい思いをよくしているので、いざというときの自分のメモです

記載環境は、Maya2015 on Windows10 です

## レンダリングエンジンの変更

Mayaのデフォルトレンダリングは **OpenGL** になっています  
**DirectX11** に変更する方法

1. プリファレンスウィンドウを開きます  
**Windows**  
　> **Settings/Prederences**  
　　> **Preferences**  
<img src="/pic/Settings-Maya2015-part1_00.png" style="border:solid 5px #e6e6e6"/>  

1. **Display** カテゴリの **Viewport 2.0** に  
Rendering engine を変更できるドロップダウンリストがあるので、そこで変更できる  
<img src="/pic/Settings-Maya2015-part1_01.png" style="border:solid 5px #e6e6e6"/>  


## レンダー設定

上記はレンダリングエンジンについてでしたが、レンダリングの設定を触るところはまた別の箇所になっています

1. レンダー設定のウィンドウを開きます  
**Windows**  
　> **Rendering Editors**  
　　> **Render Settings**  
<img src="/pic/Settings-Maya2015-part1_04.png" style="border:solid 5px #e6e6e6"/>  

1. レンダー設定ウィンドウ  
<img src="/pic/Settings-Maya2015-part1_05.png" style="border:solid 5px #e6e6e6"/>  

1. 使うレンダリングの設定を、ここで選ぶことが出来ます  
わたしの場合は Maya Hardware 2.0 を選択したりしてます  
![](/pic/Settings-Maya2015-part1_06.png)  


## ハードウェアレンダラ2.0設定

浮動小数レンダーターゲットを有効化したいとき

> この画面は、Maya2015固有みたいです

1. Mayaの作業画面（ビューポート）の **Renderer** の **Viewport2.0** の横にある、四角いチェックを押してみます  
![](/pic/Settings-Maya2015-part1_02.png)

1. **Hardware Renderer 2.0 Settings** のウィンドウが表示されます  
<img src="/pic/Settings-Maya2015-part1_03.png" style="border:solid 5px #e6e6e6"/>  

1. 精度を上げた浮動小数レンダリングをするには  
**Floating Point Render Target** の Enable にチェックを入れます  
フォーマットは、16bit と 32bit が選択できました

## カメラの設定

1. カメラアトリビュートを出すには  
Mayaの作業画面（ビューポート）の **View** の **Select Camera** を選択します  
![](/pic/Settings-Maya2015-part1_07.png) 

1. **Attribute Editor** がぽこっと現れます  
違うものが現れたときには、  
横のタブに **persp**, **perspShape**  
縦のタブに **Channel Box/Layer Editor**, **Attribute Editor** があるので  
開きたいものを選択します  
![](/pic/Settings-Maya2015-part1_08.png)

## インストールされているプラグイン

1. プラグインマネージャを開きます  
**Windows**  
　> **Settings/Prederences**  
　　> **Plug-in Manager**  
<img src="/pic/Settings-Maya2015-part1_09.png" style="border:solid 5px #e6e6e6"/>  

1. プラグイン一覧がここで見れます  
<img src="/pic/Settings-Maya2015-part1_10.png" style="border:solid 5px #e6e6e6"/> 

自分の必要なメモ書きでした  
Mayaは奥が深いですね～