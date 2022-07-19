+++
date = "2022-07-19T01:00:00+09:00"
draft = false
title = "[Windows] 壁紙のレジストリデータ"
tags = ["Windows"]
+++

PCの壁紙にしてる画像データ…どこにあるんだっけ？  
デスクトップでは見えてるんだけど…

そんな時は一度、レジストリの中を見てみたら解決するかもしれません

Windows11の環境で見てみました

-----

## 壁紙のレジストリ設定

1. `regedit` でアプリを検索すると `Registry Editor` が出てきます  
![](/pic/Registory-WallpapersSettings-onWindows_00.png)  
  
1. `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Wallpapers`  
の中に、壁紙として登録されている画像情報のパスが入っています  
![](/pic/Registory-WallpapersSettings-onWindows_01.png)


おお。。。ここにありましたか。。。！

しかし、  
元のファイルを消しちゃったけど、デスクトップの壁紙だけは残ってるってときは  
どこにあるんでしょうかね…  
どなたか知ってる方いらっしゃれば教えてくださいｍｍ

---

（参考） 

- Windows 10 最近使用したデスクトップの背景画像をWindowsの設定から削除して更新する  
[https://www.billionwallet.com/windows10/backimage-remove.html](https://www.billionwallet.com/windows10/backimage-remove.html)
