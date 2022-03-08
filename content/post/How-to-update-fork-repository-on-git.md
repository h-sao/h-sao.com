+++
date = "2022-02-27T01:00:00+09:00"
draft = false
title = "[Git] フォーク元のリポジトリに追いつきたいときメモ"
tags = ["git", "GitHub"]
+++

よくフォークしたリポジトリを最新化する方法を忘れては検索を繰り返してるので、いい加減自分のブログにメモしておきます

最初の `fork` 以降、フォーク元のリポジトリに追いつきたいときのお話です

---

## 1. 自分のローカルのリポジトリに登録されているリモート情報を見てみる

```xml
$ git remote -v
```
わたしの実行結果はこんな感じ、まだフォーク元との関連はなし

```xml
$ git remote -v
origin	https://github.com/h-sao/xxx.git (fetch)
origin	https://github.com/h-sao/xxx.git (push)
```

## 2. フォーク元のURLを `upstream` に登録します

（フォーク元は zzz/xxx という名前で公開されている場合です）

```xml
$ git remote add upstream https://github.com/zzz/xxx.git
```

ちゃんと登録されてるかどうかは、さっきの `git remote -v` で確認します

```xml
$ git remote -v
origin	https://github.com/h-sao/xxx.git (fetch)
origin	https://github.com/h-sao/xxx.git (push)
upstream	https://github.com/zzz/xxx.git (fetch)
upstream	https://github.com/zzz/xxx.git (push)
```

`upstream` にフォーク元が登録されていますね

＜余談＞

> もし `upstream` の内容を間違って登録してしまったときは  
`$ git remote remove upstream`  
このコマンドで削除することができます

## 3. フォーク元リポジトリを `fetch` and `mearge` します

```xml
$ git fetch upstream
$ git merge upstream/master
```

＜余談＞

> ※ これ↑↑↑ は主流のブランチが `master` の場合。ものによっては `main` の場合もあるので確認が必要！  
@qt_luigi さんからのコメント頂いたので、記事に追記しておきます～


ローカルのリポジトリ内容が、フォーク元と同じに最新化されました！

---

あとは別ブランチでプルリクエストを出すなりなんなりすればOK

ローカルの `master` はこのまま `push` しておくのも忘れずに♪

一度この作業をやっておくと、GitHub側に `Fetch upstream` というボタンが出てくるので、次からはこのボタンをポチッとするだけで↓↓↓

<img src="/pic/How-to-update-fork-repository-on-git_00.png" style="border:solid 5px #e6e6e6"/>

内容がフォーク元に追いつきます

便利ですね〜★

（参考）

- 他人のリポジトリをフォークして、最新版を追従する方法 - akitoshiblog
[https://akitoshiblog.hatenablog.jp/entry/2020/09/22/155538](https://akitoshiblog.hatenablog.jp/entry/2020/09/22/155538)

