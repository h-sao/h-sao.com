+++
date = "2017-10-20T19:00:00+09:00"
draft = false
title = "[PowerShell] ZoneIDとPowerShellの実行ポリシー"
tags = ["PowerShell"]
+++

PowerShellでちょっとしたツールを動かそうと思って、初歩的なところであれ？となったので、書いておきます

PowerShellの実行ポリシー（Execution Policy）は、

```c
> Get-ExecutionPolicy
```

このコマンドで確認できます

Windows10のデフォルトでは **Restricted** になっていて、ps1 ファイルをコマンドライン上から実行することが出来ません  
なので

```c
> Set-ExecutionPolicy RemoteSigned
```

を実行して、 **RemoteSigned** にセキュリティポリシーを変更します

しかし、**RemoteSigned** になってることを確認したのに、  
しかしなぜか、ps1 ファイルの実行時にセキュリティエラーとなってしまいました

なんだこれ（↓）は…？

```c
セキュリティ エラー: PSSecurityException
```

なんでだろーと思ってたのですが、何てことはない  
実行しようとしてた ps1 ファイルは、インターネット越しに入手したものでした  
（自分のサイトから落としてきたんだけどね）

こんな感じ↓↓↓

<img src="/pic/ZoneID_and_PowerShell_ExcutionPolicy_00.png" style="border:solid 5px #e6e6e6"/>

どうやら、インターネット越しに入手したファイルには **ZoneID** というのが付与されるらしく  
Windowsはローカルファイルと区別出来るようにしてくれてるんですね～

ローカルファイルとして扱いたい時には、ポチっと「**ブロックの解除**」を押せばOKです

ちゃんと、ps1 ファイルが動作するようになりました(^^♪

ZoneID…知らなかった…＞＜。


参考：  
- WindowsでPowerShellスクリプトの実行セキュリティポリシーを変更する - @IT  
  [http://www.atmarkit.co.jp/ait/articles/0805/16/news139.html](http://www.atmarkit.co.jp/ait/articles/0805/16/news139.html)
