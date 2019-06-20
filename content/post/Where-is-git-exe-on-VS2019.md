+++
date = "2019-06-20T16:00:00+09:00"
draft = false
title = "[Git] Visual Studio 2019の中のGitコマンドを探せ！"
tags = ["Git"]
+++

Gitクライアント…

自然に入ってますよね、わざわざ入れなくても。。。( \*´艸｀)

Visual Studio 2019 が入ってる環境だったので、Gitクライアントくらいあるじゃろーと思って、検索してみたらありました！

わたしの場合の環境は、ここ↓↓↓

> C:\Program Files (x86)  
　\Microsoft Visual Studio  
　　\2019  
　　　\Enterprise  
　　　　\Common7  
　　　　　\IDE  
　　　　　　\CommonExtensions  
　　　　　　　\Microsoft  
　　　　　　　　\TeamFoundation  
　　　　　　　　　\Team Explorer  
　　　　　　　　　　\Git  
　　　　　　　　　　　\cmd  
　　　　　　　　　　　　\git.exe


```
C:\dev>"C:\Program Files (x86)\Microsoft Visual Studio\2019\Enterprise\Common7\IDE\CommonExtensions\Microsoft\TeamFoundation\Team Explorer\Git\cmd\git.exe" --version
git version 2.21.0.windows.1

C:\dev>
```

バージョンもそこそこ新しいです！

パス長いけど、これ使うか～( ^^) _旦~~