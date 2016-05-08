+++
date = "2016-05-08T14:00:00+09:00"
draft = false
title = "[Unity] PC実行時のWindowsサイズの設定の仕方＆起動時フック"
tags = ["Unity"]
+++

Unityのバグなのか、わたしの使い方が悪いのか？  
PC実行時のウィンドウサイズの設定が、思うようにできなかったので  
その解決メモです

# PCで実行したら全画面…

Unityのプロジェクトファイルを新規作成して

> "File" → "Build & Run" 

を選択すると、ビルドが走り、実際に exe が実行されます

<img src="/pic/Change-Unity-window-size_01.png" style="border:solid 5px #e6e6e6"/>

へぇ、画面解像度( Screen resolution )とか、ここで変えることが出来るんだ～  
と思って解像度設定を変更し、"Play!" で起動させると、いちおう思ったサイズで起動することが出来ます

# Unityプロジェクトに Plyaer Settings がある

ユーザに毎回ウィンドウサイズを選択させるのも、なんだかねぇ…ということで、ちゃんと設定したいと思います

Unityのメニューの

> "File" → "Build Settings" 

Build Settings を選択すると、ビルドに関する設定が変更できるようです  
下にある、"Player Settings" を選択すると、
インスペクタウィンドウが出ます


<img src="/pic/Change-Unity-window-size_00.png" style="border:solid 5px #e6e6e6"/>

ここで、起動時の Player Settings が出来るようですが…

![](/pic/Change-Unity-window-size_02.png)

「Resolution」 の 「**Default Is Full Screen**」 のチェックボックスを外しましょう  
ついでに、「Standalone Player Options」 の **「Display Resolution Dialog」 を　「Disable」**　にしておきます

これで exe を実行するたびに 「FixSizeWindows Configuration」 は開かなくなります  



「Resolution」 には、「Default Screen Width/Height」 がありますが、、、  
ここで設定を変えても、ウィンドウサイズは変わりません  
（なんでやねん…＞＜）

どうやら、「前回起動したときのウィンドウサイズ」で起動するみたいです…  
Development Build にしても、しなくても、結果は一緒でした  
初期起動のサイズは、ここじゃないのかな…(´・ω・｀)



余談ですが、ここで変えた設定は、

- .\\ProjectSettings\\ProjectSettings.asset

の中に、テキストファイルで保存されます



# ウィンドウサイズはプログラムで変更

仕方がないので、スクリプトで記載します

Unityのメニュー **Assets > Create > C# Script** で新規スクリプトを作ります

わたしは適当に GameInitial と名付けました（Unity の Project 内に、GameInitial の C#ファイルが追加されます）

## いーっちばん最初のゲーム起動時をフックしたい？

もちろん、ウィンドウサイズを変更したいためです  
解像度が小さい/大きいなど  
起動したときに、クライアント端末によってウィンドウサイズを変えたいとかあると思います

たぶん、現状の Unity 5.3系の最速フック方法は、**RuntimeInitializeOnLoadMethod** ではないかと…

- RuntimeInitializeOnLoadMethodAttribute  
[http://docs.unity3d.com/ScriptReference/RuntimeInitializeOnLoadMethodAttribute.html](http://docs.unity3d.com/ScriptReference/RuntimeInitializeOnLoadMethodAttribute.html)

ユーザがアクションを起こす前に動くらしいですが  
実際やってみると、ロゴ起動画面の方が先に動くので  
ほんとの意味の一番最初のウィンドウサイズは、ここでは遅いみたいです

一応サンプルコード  
MonoBehaviour は無くても動くみたいです  
あと、オブジェクトなどにスクリプトをアタッチしなくてもOKです

```c#
using UnityEngine;

public class GameInitial //: MonoBehaviour
{
    [RuntimeInitializeOnLoadMethod]
    static void OnRuntimeMethodLoad()
    {
        Screen.SetResolution( 640, 960, false, 60);

    }

    //// Use this for initialization
    //void Start()
    //{

    //}

    //// Update is called once per frame
    //void Update()
    //{

    //}
}
```

自分が知らないだけかもしれませんが、 RuntimeInitializeOnLoadMethodAttribute より早い時点で自分の処理を差し込むことは出来ないみたい…

# ウィンドウサイズを指定できるコマンドライン引数がある！

結局、起動時のウィンドウサイズは結局どこで更新するんでしょうか…？

実は、飛び道具的ですが、exe の引数に渡せるみたいです…！（そこか…＞＜

- 「Unity スタンドアロンプレイヤーのコマンドライン引数」の項
 [http://docs.unity3d.com/ja/current/Manual/CommandLineArguments.html](http://docs.unity3d.com/ja/current/Manual/CommandLineArguments.html)


自分で作った exe の引数に

- -screen-width ：幅
- -screen-height ：高さ

を渡してあげればOKでした

```
> xxx.exe -screen-width 300 -screen-height 300
```

小さいスクリーンサイズで起動したのちに…

<img src="/pic/Change-Unity-window-size_03.png" style="border:solid 5px #e6e6e6"/>

自分の指定した Screen.SetResolution サイズで起動

<img src="/pic/Change-Unity-window-size_04.png" style="border:solid 5px #e6e6e6"/>

つまり、引数に自分で好きな数値を渡せるようになってるので  
それと、Screen.SetResolution の指定を同じ大きさにしておけば  
最初から自分自身でサイズを調整できますね


なんだろなー  
なんで起動時のサイズくらい、すんなり設定できないんでしょうか…謎や～

もしかしたら、無料版では起動時のロゴを外せないようにするために、プログラマは触れないようになってるのかなー？

ともあれ、意図したことは出来るようになっているので、まぁ良しとします！