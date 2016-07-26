+++
date = "2016-07-21T20:30:00+09:00"
draft = false
title = "[Windows10] PCのGPUを確認する"
tags = ["Windows10"]
+++

PCのGPU、つまりグラフィックボードってなんだろな？と思った時の操作メモです

対象は Windows10

(1) まず Windows10 のディスプレイ画面を表示させます  

> デスクトップ画面を右クリック  
>　＞ディスプレイ設定

もしくは

> Windowsのスタートボタン  
> 　＞設定  
> 　　＞システム

(2) 「ディスプレイの詳細設定」を選択！ 

<img src="/pic/Check-gpu-on-pc_00.png" style="border:solid 5px #e6e6e6"/>

(3) ディスプレイのカスタマイズの項目の中の、「アダプターのプロパティの表示」を選択！ 

<img src="/pic/Check-gpu-on-pc_01.png" style="border:solid 5px #e6e6e6"/>

(4) グラフィックボードの情報が出てきます

<img src="/pic/Check-gpu-on-pc_02.png" style="border:solid 5px #e6e6e6"/>

このPCはインテルボードでした

---
別の調べ方としては、DirectX 診断ツール（dxdiag）を使う方法もあり

dxdiag で検索すると、一致するコマンドが出てきます

<img src="/pic/Check-gpu-on-pc_03.png" style="border:solid 5px #e6e6e6"/>

実行すると、こんな感じで、表示を出してくれます

<img src="/pic/Check-gpu-on-pc_04.png" style="border:solid 5px #e6e6e6"/>

---

GPU…気になりますよね…  
CPUは多少型が古くても、それほど差が出ないけど  
GPUだけは新しい型のものにしとかないと…とかとか  
私的にはこだわりたいのです！！！  
（実行出来てるかどうかは…涙）
