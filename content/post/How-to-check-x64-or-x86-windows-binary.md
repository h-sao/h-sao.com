+++
date = "2020-10-26T12:00:00+09:00"
draft = false
title = "[Windows] そのexeがx64かx86かを見分ける方法"
tags = ["windows", "mac"]
+++

手元にあるexeが64bitなのか32bitなのかを知りたい…

開発してるとこういう時がたまにあると思う…

多分ちょっとニッチな需要だけど、検索したらちゃんと誰かが既に調べてくれていました！


# パターン1. Windows のメモ帳（notepad.exe）を使って調べる

何にもない環境でも、OS が Windows なら必ず入っているメモ帳"φ(・ェ・o)~メモメモ

これで調べるのが一番簡単な調査方法だと思います~

1. メモ帳：notepad.exe で調べたい exe ファイルを開く

1. 文字列 **PE** を検索する  
こんな感じ↓↓↓  
<img src="/pic/How-to-check-x64-or-x86-windows-binary_00.png" style="border:solid 5px #e6e6e6"/>

1. **PE** の後ろに空白がちょっとだけあり、その後に続く文字で判別できます  

- `PE  L`
  - **32bit版は `L`** が続く
- `PE  d`
  - **64bit版は `d`** が続く

ふむふむなるほど！

# パターン2. バイナリエディタを使って調べる

最近のバイナリエディターのおススメは **hexdump for VSCode** です  
昔は [TEXBIN](http://www.net3-tv.net/~m-tsuchy/tsuchy/dlpage.htm) を使ってましたが、VSCode 内で完結するのが良いですね~

- **hexdump for VSCode**  
[https://marketplace.visualstudio.com/items?itemName=slevesque.vscode-hexdump](https://marketplace.visualstudio.com/items?itemName=slevesque.vscode-hexdump)

![](/pic/How-to-check-x64-or-x86-windows-binary_01.png)  

1. `VSCode` を開く

1. 調べたい exe をドラッグ＆ドロップする

1. こんなメッセージが出る…（だが気にしない）  
```xml
The file is not displayed in the editor becouse it is either binary or users an unsupported eext encoding.
Do you want to open it anyway?
```

1. 右上に出てる `Show Hexdump` をポチッ！  
![](/pic/How-to-check-x64-or-x86-windows-binary_02.png)  

1. バイナリモードで見て、`0x0100` 前後（ファイルによる）の番地あたりに  
**0x5045** （つまり **PE** ）から始まる文字列があるので探す

1. こんな感じ

- `50 45 00 00 4c` →つまり `PE  L`
  - **32bit版**
- `50 45 00 00 64` →つまり `PE  d`
  - **64bit版**

![](/pic/How-to-check-x64-or-x86-windows-binary_03.png)  

ふむふむ、この方法も良い感じです(・ω・)ノ

# パターン3. hexdump コマンドで調べる（macOSの場合）

mac しか持ってない環境で VSCode も入ってなくて Windows の exe のビットを調べたい時って一体…！？  
とは思いますが(><)  
先日、わたし自身が必要だったので、これも調べてしまいました  

VSCodeがあればパターン2 の方法で調べられるのですが  
mac の標準環境だけで調査したいときは、**hexdump** が標準で装備されているので  
それを使うのも手です(。・_・。)ノ

```
# [-n バイト数] で先頭からのバイト数の分だけ表示出来る
$hexdump -n 400 バイナリを調べたいファイル名
```

![](/pic/How-to-check-x64-or-x86-windows-binary_04.png)  

上記の例だと、x86 のようですね  
良い感じです

~~素の mac でバイナリデータを見たいときは、これが良いかも~~  
パターン４がありました（追記）

# パターン4. file コマンドで調べる（macOSの場合）

後日、こちらに追記しました

- [Windows] そのexeがx64かx86かを見分ける方法 Part2  
[http://h-sao.com/blog/2020/11/07/how-to-check-x64-or-x86-windows-binary-part2/](http://h-sao.com/blog/2020/11/07/how-to-check-x64-or-x86-windows-binary-part2/)

## 備考

いくつか他の exe のバイナリデータを見てみました

だいたいファイルの先頭の `0x0080` ~ `0x0140` くらいまでに PEヘッダがあるようです

## 参考

- How to check if a binary is 32 or 64 bit on Windows? - superuser (StackExchange)
[https://superuser.com/questions/358434/how-to-check-if-a-binary-is-32-or-64-bit-on-windows](https://superuser.com/questions/358434/how-to-check-if-a-binary-is-32-or-64-bit-on-windows)

- 今回の調査対象にしたツール（x86版とx64版が配布されてる）  
動画キャプチャツール「AG-デスクトップレコーダー」  
[http://t-ishii.la.coocan.jp/download/AGDRec.html](http://t-ishii.la.coocan.jp/download/AGDRec.html)
