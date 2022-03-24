+++
date = "2022-03-24T15:00:00+09:00"
draft = false
title = "[Hugo] GitHub Actionsでサイトを自動デプロイしたときにCNAMEを残す方法"
tags = ["hugo", "github"]
+++

`Hugo` に限らないのですが   
`gh-pages` ブランチを利用して `GitHub Pages` を作った時  
自動デプロイで、独自ドメインの設定を記載した `CNAME` が消えちゃうことがあります

そのトラブルシュートについてです

--- 

`Hugo` 自体の設定もあるのですが、それよりも  
元のコンテンツに `CNAME` ファイルを含めてしまうのがシンプルで簡単です

`Hugo` ならメインブランチの `static` ディレクトリ以下に `CNAME` を設置します

<img src="/pic/How-to-support-Hugo-auto-deploy-to-CNAME-when-using-GitHub-Actions_00.png" style="border:solid 5px #e6e6e6"/>

これだけでOK！
