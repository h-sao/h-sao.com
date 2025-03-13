+++
date = "2025-3-13T09:30:00+09:00"
draft = false
title = "[Windows] サインイン時のデフォルトユーザー名の表示"
tags = ["windows", "Security"]
+++

WindowsのPCログイン時って、何も設定しなければ  
デフォルトでは前回ログインしたユーザー名が表示されていて、便利です

しかし、これが便利だなーと思うときと、毎回表示してほしいなーと思うときがあります

その設定の場所のメモです

環境：Windows11

Windows + R キーで `Local Group Policy Editor` を起動します

> Local Computer Policy  
　　> Windows Settings  
　　　> Security Settings  
　　　　> Local Policies  
　　　　　> Security Options  

ここの下の

> Interactive logon: Don't display username at sing in

<img src="/pic/How-to-display-username-at-sing-in_00.png" style="border:solid 5px #e6e6e6"/> 

これを有効にすることで、思ったようなサインイン表示になります