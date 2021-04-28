+++
date = "2020-11-07T10:00:00+09:00"
draft = false
title = "[Windows] そのexeがx64かx86かを見分ける方法 Part2"
tags = ["windows", "mac"]
+++

先日、こんなブログを書きました

- [Windows] そのexeがx64かx86かを見分ける方法  
[http://h-sao.com/blog/2020/10/26/how-to-check-x64-or-x86-windows-binary/](http://h-sao.com/blog/2020/10/26/how-to-check-x64-or-x86-windows-binary/)

このブログを公開したところ、Twitterで [@ripjyr](https://twitter.com/ripjyr) さんより  
macなら `file` コマンドあるよと教えていただいたので追加記事を書いておきます～

上記のブログ記事では、 exe データが 32bit なのか 64bit なのかを調べるやりかたとして  
３つのチェック方法を記載しました

今回は４つ目の方法の紹介です～！

# パターン4. file コマンドで調べる（macOSの場合）

これは mac のみの環境で、Win exe を調べたいって時のお話です

mac には `file` コマンドがあって、それを使うと簡単に Windows exe のビット（PEヘッダ）を調べることができます

```
$ file 調べたいファイル名 
```

- x86の場合

**PE32 executable (GUI) Intel 80386, for MS Windows** と表示されます！

```xml
$ file AGDRec.exe  
AGDRec.exe: PE32 executable (GUI) Intel 80386, for MS Windows
```

- x64の場合

**PE32+ executable (GUI) x86-64, for MS Windows** とでます

```xml
$ file AGDRec64.exe
AGDRec64.exe: PE32+ executable (GUI) x86-64, for MS Windows
```

真に、素の mac でバイナリデータを見たいときは、これが良いですぅ！！！

## 謝辞

@ripjyr さん、教えてくださってありがとうございました ^^
