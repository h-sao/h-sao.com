+++
date = "2020-01-29T16:00:00+09:00"
draft = false
title = "[Git] Gitの歴史を書き換える関連"
tags = ["Git"]
+++

よくやらかすので自分メモ

## 事前準備

```
# 今いるブランチの確認
$ git branch

# ブランチに移動
$ git checkout feature/xxxxx
```

PR出す前、きれいな環境にしたいなどで整えておきたいこと

```
# 手動ブランチの取得/追従
$ git checkout feature/yyyyy
$ git pull
```

## ローカル作業全てを無かったことにする

```
git fetch origin
git reset --hard origin/master
```


## pushしてしまった歴史を書き換える

```
# 自分の名前のコミットを検索
$ git log --committer="Sao Haruka"

# ↑の gitlog より戻りたいハッシュ値を探す

$ git reset [戻りたいコミットのハッシュ値] --hard

# 強制push
$ git push -f origin feature/xxxxx
```

既に `feature/xxxxx` で誰かが作業していたら強制プッシュはあきらめる


## 不要になったブランチを削除する

要らないブランチは削除しとく

```
$ git push --delete origin feature/xxxxx
```

## PRに不要なコミットが混じる

原因：

- ブランチが追従されてないのが問題
- マージ先が間違ってる

マージ先の最新を取っとくと大丈夫
