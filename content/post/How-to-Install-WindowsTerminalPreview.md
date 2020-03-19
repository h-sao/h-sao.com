+++
date = "2020-03-19T23:00:00+09:00"
draft = false
title = "[WindowsTerminal] Preview版のインストール＆Git Bashを追加したい"
tags = ["WindowsTerminal"]
+++

 **Windows Terminal** …どんなもんだろなと思って、使ってみました

もともとは、**Gitのブランチ名をコマンドプロンプトに表示できたらいいのにー** と思って調べたのでした  
Windows Terminal に Git Bash (正確には `Git for Windows` ) が登録できるようなので、それをやってみました

## Windows Terminal (Preview)のインストール

普通に 「Windows Terminal download」で検索すると、わたしの場合「マイクロソフトストア」のリンクがトップに来ましたけど  
ストアとかよくわからんし…

よく見ると
Windows Terminal は GitHub に公開されてます

- [https://github.com/microsoft/terminal](https://github.com/microsoft/terminal)

ここから Preview版をGet★

- [https://github.com/microsoft/terminal/releases](https://github.com/microsoft/terminal/releases)

3/19時点では、**Windows Terminal Preview v0.10.761.0** のバージョンでした

---

以下、やったこと

### 1) `msixbundle` を実行するとこんな画面↓↓↓

<img src="/pic/How-to-Install-WindowsTerminalPreview_02.png" style="border:solid 5px #e6e6e6"/>

### 2) インストールを押すと、わたしの環境ではエラーになりました

<img src="/pic/How-to-Install-WindowsTerminalPreview_00.png" style="border:solid 5px #e6e6e6"/>

エラーメッセージ内容：

> Microsoft.WindowsTerminal_0.10.761.0_x64__8wekyb3d8bbwe をインストールできません。このパッケージはデバイスと互換性がありません。このパッケージをインストールするには、OS バージョン 10.0.18362.0 またはそれ以降の Windows.Mobile デバイス ファミリが必要です。現在、デバイスでは OS バージョン 10.0.17763.1098 が実行されています。 (0x80073cfd)

おぉ。。。めっちゃ判りやすいメッセージが！

### 3) よくわからんけど、Windows Updateしてみます

<img src="/pic/How-to-Install-WindowsTerminalPreview_01.png" style="border:solid 5px #e6e6e6"/>

### 4) もう一度ダウンロードした `msixbundle` を実行

### 5) 正常にインストールできたみたい！

![](/pic/How-to-Install-WindowsTerminalPreview_03.png)

### 6) 開くウィンドウのタイプを選べます

![](/pic/How-to-Install-WindowsTerminalPreview_04.png)

よしよし

## タブに Git Bash を追加

先人が居てました。。。！

- Adding Git-Bash to the new Windows Terminal - stackoverflow  
[https://stackoverflow.com/questions/56839307/adding-git-bash-to-the-new-windows-terminal](https://stackoverflow.com/questions/56839307/adding-git-bash-to-the-new-windows-terminal)

- Windows TerminalにPowerShell Coreを追加する - kkamegawa's weblog  
[https://kkamegawa.hatenablog.jp/entry/2019/06/23/135124](https://kkamegawa.hatenablog.jp/entry/2019/06/23/135124)


あと中の人のスコット・ハンセルマンのブログも参考になりました

- Now is the time to make a fresh new Windows Terminal profiles.json  
[https://www.hanselman.com/blog/NowIsTheTimeToMakeAFreshNewWindowsTerminalProfilesjson.aspx](https://www.hanselman.com/blog/NowIsTheTimeToMakeAFreshNewWindowsTerminalProfilesjson.aspx)

真似してわたしもやってみます

### 7) PowerShell で GUIDを取得します

GUIDの設定は、生成できるものであればなんでも良いみたいです

- mehulmpt/profiles.json - コメントに注目  
[https://gist.github.com/mehulmpt/16826be279bb9bf70310b465ca9c5de3](https://gist.github.com/mehulmpt/16826be279bb9bf70310b465ca9c5de3)


GUID生成サイトもありました(。・_・。)

- [http://new-guid.com/](http://new-guid.com/)

わたしは PowerShell で作ってみます

```
> [guid]::NewGuid()
```

こんな感じで取得できました

![](/pic/How-to-Install-WindowsTerminalPreview_05.png)


### 8) Windows Terminal の settings を開きます  

これは json ファイルになってました

わたしの環境では、以下のパスに存在していました  
※ xxx... は編集しています、適当な文字列でした

> C:\Users\akiko.kawai\AppData\Local\Packages\Microsoft.WindowsTerminal_xxxxxxxxxxxxx\LocalState\profiles.json

デフォルトの中身はこんな感じ↓↓↓

```c
// To view the default settings, hold "alt" while clicking on the "Settings" button.
// For documentation on these settings, see: https://aka.ms/terminal-documentation

{
    "$schema": "https://aka.ms/terminal-profiles-schema",

    "defaultProfile": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",

    "profiles":
    {
        "defaults":
        {
            // Put settings here that you want to apply to all profiles
        },
        "list":
        [
            {
                // Make changes here to the powershell.exe profile
                "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
                "name": "Windows PowerShell",
                "commandline": "powershell.exe",
                "hidden": false
            },
            {
                // Make changes here to the cmd.exe profile
                "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
                "name": "cmd",
                "commandline": "cmd.exe",
                "hidden": false
            },
            {
                "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
                "hidden": false,
                "name": "Azure Cloud Shell",
                "source": "Windows.Terminal.Azure"
            }
        ]
    },

    // Add custom color schemes to this array
    "schemes": [],

    // Add any keybinding overrides to this array.
    // To unbind a default keybinding, set the command to "unbound"
    "keybindings": []
}
```


### 9) `profiles` の個所に以下を追加します

```c
{
    // さっき PowerShellで作ったGUID
    "guid": "{99999999-9999-9999-9999-999999999999}",
    // ウィンドウの半透明度合い
    "acrylicOpacity" : 0.75,
    "closeOnExit" : true,
    "colorScheme" : "Campbell",
    // Git Bash の exe のある場所
    "commandline" : "\"%PROGRAMFILES%\\git\\usr\\bin\\bash.exe\" -i -l",
    "cursorColor" : "#FFFFFF",
    "cursorShape" : "bar",
    "fontFace" : "Consolas",
    "fontSize" : 10,
    "historySize" : 9001,
    // CMDアイコンならこれ
    //"icon" : "ms-appx:///ProfileIcons/{0caa0dad-35be-5f56-a8ff-afceeeaa6101}.png",
    // Git for Windows の ico ファイルを直接指定することも出来る
    "icon" : "%PROGRAMFILES%\\git\\mingw64\\share\\git\\git-for-windows.ico"
    // 追加タブに表示する名前
    "name" : "GitBash",
    "padding" : "0, 0, 0, 0",
    "snapOnInput" : true,
    // 初期ディレクトリ、 "C:\\dev" などでもOK
    "startingDirectory" : "%USERPROFILE%",
    "useAcrylic" : true
}
```

---

#### ◆アイコンについて

アイコンの登録は ms-appxのURL のまんまが使えるみたいです

参考：

- Feature Request - List Icons in the default profiles.json - microsoft/terminal  
[https://github.com/microsoft/terminal/issues/1456](https://github.com/microsoft/terminal/issues/1456)

試しに Linux アイコンにしてみたら、ペンギンになりました(。・_・。)ノ

![](/pic/How-to-Install-WindowsTerminalPreview_09.png)

CMDアイコンだとこんな感じ

![](/pic/How-to-Install-WindowsTerminalPreview_10.png)


---

#### ◆GUIDについて

今回は GUID は適当なものを PowerShell で作りましたが  

`profiles.json` の中の `"defaultProfile"` の値を  
自分の Git Bash の GUIDに設定すると  
Windows Terminal のデフォルト起動時のウィンドウに出来ました

---

#### ◆profiles.json

`profiles.json` の全体像はこんな感じ

```c
// To view the default settings, hold "alt" while clicking on the "Settings" button.
// For documentation on these settings, see: https://aka.ms/terminal-documentation

{
    "$schema": "https://aka.ms/terminal-profiles-schema",

    "defaultProfile": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",

    "profiles":
    {
        "defaults":
        {
            // Put settings here that you want to apply to all profiles
        },
        "list":
        [
            {
                // Make changes here to the powershell.exe profile
                "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
                "name": "Windows PowerShell",
                "commandline": "powershell.exe",
                "hidden": false
            },
            {
                // Make changes here to the cmd.exe profile
                "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
                "name": "cmd",
                "commandline": "cmd.exe",
                "hidden": false
            },
            {
                "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
                "hidden": false,
                "name": "Azure Cloud Shell",
                "source": "Windows.Terminal.Azure"
            },
            {
                // さっき PowerShellで作ったGUID
                "guid": "{99999999-9999-9999-9999-999999999999}",
                // ウィンドウの半透明度合い
                "acrylicOpacity" : 0.75,
                "closeOnExit" : true,
                "colorScheme" : "Campbell",
                // Git Bash の exe のある場所
                "commandline" : "\"%PROGRAMFILES%\\git\\usr\\bin\\bash.exe\" -i -l",
                "cursorColor" : "#FFFFFF",
                "cursorShape" : "bar",
                "fontFace" : "Consolas",
                "fontSize" : 10,
                "historySize" : 9001,
                // CMDアイコンならこれ
                //"icon" : "ms-appx:///ProfileIcons/{0caa0dad-35be-5f56-a8ff-afceeeaa6101}.png",
                // Git for Windows の ico ファイルを直接指定することも出来る
                "icon" : "%PROGRAMFILES%\\git\\mingw64\\share\\git\\git-for-windows.ico"
                // 追加タブに表示する名前
                "name" : "GitBash",
                "padding" : "0, 0, 0, 0",
                "snapOnInput" : true,
                // 初期ディレクトリ、 "C:\\dev" などでもOK
                "startingDirectory" : "%USERPROFILE%",
                "useAcrylic" : true
            }
        ]
    },

    // Add custom color schemes to this array
    "schemes": [],

    // Add any keybinding overrides to this array.
    // To unbind a default keybinding, set the command to "unbound"
    "keybindings": []
}
```

### 10) おぉ！Bashタブがリアルタイムに追加されてる！

Windows Terminal を開きっぱなしの状態でも  
追加のタブを見ると Git Bash 項目が見えました  
やったー

![](/pic/How-to-Install-WindowsTerminalPreview_07.png)

### 11) 作業したいブランチ名もちゃんと表示されてます

当然といえば当然

![](/pic/How-to-Install-WindowsTerminalPreview_08.png)


---

結構いろいろと簡単にやりたいことが出来た印象です

調べてもすぐ情報が見つかったし  
WindowsTerminal、早く Preview 取れて欲しいですね♪