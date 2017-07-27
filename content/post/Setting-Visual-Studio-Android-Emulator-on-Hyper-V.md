+++
date = "2017-07-27T18:00:00+09:00"
draft = false
title = "[Android] Hyper-Vで使うVisual Studio Emulator for Androidの最初の一歩"
tags = ["Android", "VisualStudio", "Hyper-V"]
+++

## どうもうまくAndroidエミュレータが動かない…

いわゆる ADV Manager をつかって、Androidをエミューレートしたいのですが  
なぜかうまく起動しない…

環境

- Windows10 x64
- Visual Studio 2017

Android SDK Managerをよく見てみると、HAXM installer が "Not compatible with Windows" となっています

<img src="/pic/Setting-Visual-Studio-Android-Emulator-on-Hyper-V_00.png" style="border:solid 5px #e6e6e6"/> 

ふむ…  
無理矢理 HAXM のインストーラーをダウンロードしてきても、こんな感じでエラー

<img src="/pic/Setting-Visual-Studio-Android-Emulator-on-Hyper-V_01.png" style="border:solid 5px #e6e6e6"/> 

> This computer does not support Intel Virtualization Technology (VT-x) or it is being exclusively used by Hyper-V, HAXM cannot be installed.  
> Please ensure Hyper-V is disabled in Windows Features, or refer to the Intel HAXM documentation for more information.

なんですと…Hyper-Vが入ってたら使えないよだと…(｀・ω・´)

ワタクシ、**Android 開発のために手元の Hyper-V を辞める事は出来ないデスヨ！**  
調べてみると「**Visual Studio Emulator for Android**」というHyper-Vベースで動くエミュレーターが用意されているらしいので設定してみました


## Visual Studio Emulator for Android

すでに色んな方が同じ様な記事を書かれていますが、わたくしも今一度、再確認でございます…

### 1. まず、Hyper-V が有効なこと

（Control Panle > Programs and Features > Turn Windows features on or off）

<img src="/pic/Setting-Visual-Studio-Android-Emulator-on-Hyper-V_02.png" style="border:solid 5px #e6e6e6"/> 

### 2. Visual Studio 2017のインストーラーで「Visual Studio Emulator for Android」を選択すること

下の図では「Google Android Emulator」「HAXM」も選択してますが、はっきり言って  
この２つと、「Visual Studio Emulator for Android」は、排他の関係にあるので  
「Visual Studio Emulator for Android」だけ選択でOK

<img src="/pic/Setting-Visual-Studio-Android-Emulator-on-Hyper-V_03.png" style="border:solid 5px #e6e6e6"/> 

### 3. 「Visual Studio Emulator for Android」を立ち上げる

検索すると、インストールされてます  
<img src="/pic/Setting-Visual-Studio-Android-Emulator-on-Hyper-V_04.png" style="border:solid 5px #e6e6e6"/> 


立ち上げたらこんな画面  
![](/pic/Setting-Visual-Studio-Android-Emulator-on-Hyper-V_05.png)

これでまずは準備完了

## エミュレーターの起動

### 1. 空のプロジェクトを作る

わたしはC++で開発したかったので、Androidプロジェクトの「Native-Activity Application (Android)」で新規作成しました

<img src="/pic/Setting-Visual-Studio-Android-Emulator-on-Hyper-V_06.png" style="border:solid 5px #e6e6e6"/> 

デバックメニューに「Visual Studio Emulator for Android」の欄が増えてますね  
![](/pic/Setting-Visual-Studio-Android-Emulator-on-Hyper-V_07.png)

（注意！）**「x86」を選択しないと、このエミュレーターは出てきません！**

### 2. 「Visual Studio Emulator for Android」でAndroidを起動させる

先ほど立ち上げた「Visual Studio Emulator for Android」のメニューより、緑の三角印を押して、エミュレーターを起動させます  
![](/pic/Setting-Visual-Studio-Android-Emulator-on-Hyper-V_08.png)

![](/pic/Setting-Visual-Studio-Android-Emulator-on-Hyper-V_09.png)

起動できた(｀・ω・´)

## エミュレーターが起動しない時

最初、わたしはエミュレーターが起動途中でストップしてしまいました(-_-)zzz

![](/pic/Setting-Visual-Studio-Android-Emulator-on-Hyper-V_10.png)

> The emulator is unable to connect to the device operating system:  
Could't auto-detect the guest system IP address.  
Some functionality might be disabled.

なんじゃこりゃ…(´・ω・`)  
IPうんぬんと書かれてるので、NWプロパティを見てみます

Conrtol Panel  
　＞ Network and Internet  
　　＞ Network Connections

Hyper-Vが「Visual Studio Emulator for Android」のために作ったと思われるアダプターがありました

わたしの場合  
**vEthernet (Internal Ethernet Port Windows Phone Emulator Internal Switch)**  
という名前です

IPv4プロパティを見てみると、変なIPアドレスが割り当てられてる…？  
慌てて削除し、動的に変更する設定に変えます↓↓↓

<img src="/pic/Setting-Visual-Studio-Android-Emulator-on-Hyper-V_11.png" style="border:solid 5px #e6e6e6"/> 

いちお、NWアダプタは「識別されてないネットワーク」状態ですが  
エラー表示にはなっていません

<img src="/pic/Setting-Visual-Studio-Android-Emulator-on-Hyper-V_14.png" style="border:solid 5px #e6e6e6"/> 

この状態で、エミュレーターを起動させると、無事立ち上がりました(*^^*)良かった！

## エミュレーターでデバックする

VSに戻ってみると、こんな感じでエミュレーターがデバックで使えるようになってます！  
![](/pic/Setting-Visual-Studio-Android-Emulator-on-Hyper-V_12.png)

VSからデバック実行させると  
![](/pic/Setting-Visual-Studio-Android-Emulator-on-Hyper-V_13.png)

サンプルが動作しました

この状態でブレークポイントも効くので、まずは最初の設定完了って感じでしょうか  
パチパチ(*^^*)

※ちなみに実機でデバックする時には、「x86」じゃなくて「ARM」など、その実機に合ったCPUを選択したらOKです
