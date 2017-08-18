+++
date = "2017-08-18T18:40:00+09:00"
draft = false
title = "[Android] 謎のエラー「Unable to start debugging.」が出たときの対応"
tags = ["cpp", "VisualStudio", "Android"]
+++

## Androidデバッグで実行時エラー

環境

- Windows10 x64
- Visual Studio 2017
- Project type: Native-Activity Application (Android)

ある時…Visual Studio で作っている Android プロジェクトが、実行時エラーになりました

<img src="/pic/How-to-fixed-error-Unable-to-start-debugging_00.png" style="border:solid 5px #e6e6e6"/> 

> Unable to start debugging. Check your debugger settings by opening project properties and navigating to 'Configuration Properties--> Debugging'

デバック出来ないからね！プロジェクトの設定見てね！的な意味みたいですが  
デバッグモードで動かしてるし…  
一体どこの何の事を言ってるのか判らない…＞＜。  

リリースモードならOKなのかしら？と思っても、同様のエラー"(-""-)"


## 解決案（わたしはこれでは解決しませんでしたが）

検索しても、実のある回答は特に出てこず…  
唯一、解決っぽいのが、NVIDIAさんのフォーラムに載ってました

* **CodeWorks for Android 1R4: Unable to start debugging - NVIDIA DEVELOPER**  
[https://devtalk.nvidia.com/default/topic/957019/codeworks-for-android-1r4-unable-to-start-debugging/](https://devtalk.nvidia.com/default/topic/957019/codeworks-for-android-1r4-unable-to-start-debugging/)

元ネタリンクはMicrosoftさんのGitHub情報みたいです

* **MEF - GitHub Microsoft/VSProjectSystem**  
[https://github.com/Microsoft/VSProjectSystem/blob/master/doc/overview/mef.md#mef-inside-visual-studio](https://github.com/Microsoft/VSProjectSystem/blob/master/doc/overview/mef.md#mef-inside-visual-studio)

簡単に言うと、VSの **Developer Command Prompt** を使って、キャッシュクリアしてみたら？というもの  
やってみました

1. Developer Command Prompt を検索して立ち上げる  
<img src="/pic/How-to-fixed-error-Unable-to-start-debugging_01.png" style="border:solid 5px #e6e6e6"/>

1. 以下のコマンドを打つ  

> Devenv /UpdateConfiguration  
Devenv /ClearCache

<img src="/pic/How-to-fixed-error-Unable-to-start-debugging_02.png" style="border:solid 5px #e6e6e6"/>

もっともらしい対策なんですけど、わたしの実行時エラーは解決しませんでした  
何がダメなのだろ…((+_+))


## わたしが解決した方法

で、実際の解決策ですが…  
すみません、散々あーだこーだ書いてますが  
**スタートアッププロジェクトの設定もれ** でした…( ;∀;)  
いやはや、お恥ずかしい…

![](/pic/How-to-fixed-error-Unable-to-start-debugging_03.png)


Androidプロジェクトが参照する、別のプロジェクト達が  
同じソリューションファイルの中に複数存在しています

なのに、Androidプロジェクトを起動プロジェクトに設定しておらず…  

ポカミスとはまさにこの事！( ;∀;)

なぜこれに気が付かなかったのか…相当焦ってて、周りが見えてなかったみたいです

二度と同じ過ちを繰り返さないように…＞＜。  
あと、同様のエラーで困ってる人の何かしらの参考になればと思います＞＜。