+++
date = "2019-10-15T12:00:00+09:00"
draft = false
title = "[Git] Commit&PushしてしまったAuthor情報を変更したい"
tags = ["Git"]
+++

最近、GitKraken を使って更新をしています  
Pro バージョンだとアカウント情報の切り替えが簡単に出来るので  
会社用や個人用に切り替えて作業するのがとても便利です

しかしたまに切り替え忘れて、本来のアカウントじゃないやつでコミットしてしまって  
意図しないメアドやユーザ名が入ったりして…(-"-;A ...アセアセ

そんなときの変更の方法です

## Author 変更方法

直前のコミットを変更したい場合です

```
$ git commit --amend
```

を使うと、コミット履歴が書き換えられるんですけど  
これで書き換えると、コミットの作者（Author）は変わらず、コミッター（Commiter）が変わるだけです

Authorを書き換えるには

```
$ git commit --amend --author="Sao Haruka <sao@tmp.com>"
```

こうすると書き換わります  
こんな感じね↓↓↓

```bash
$ git commit --amend --author="Sao Haruka <sao@tmp.com>"
[feature/xxxxx 9999999999] [WIP] Added hogehoge
 Date: Thu Oct 10 22:30:00 2019 +0900
 1 files changed, 1 insertions(+), 1 deletions(-)

```

書き換わったー！

## サーバーに push しちゃってるとき

もし自分だけの作業ブランチでやってるのであれば、サーバーの履歴を強制的に書き換えることが出来ます  
（共通ブランチでは、強制書き換えはやめよう）

```
$ git push -f [repository] [branch]
```

恐る恐るやってみる

```bash
# まずは強制オプションをつけずにやってみる(もちろんエラー)

$ git push origin feature/xxxxx
To tmp.com:ohoho/hoge.git
 ! [rejected]              feature/xxxxx -> feature/xxxxx (non-fast-forward)
error: failed to push some refs to 'tmp.com:ohoho/hoge.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

# 強制オプションをつけてpush

$ git push -f origin feature/xxxxx
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Delta compression using up to 1 threads
Compressing objects: 100% (1/1), done.
Writing objects: 100% (1/1), 1 KiB | 1 MiB/s, done.
Total 1 (delta 1), reused 0 (delta 0)
```

事なきを得た(_・ω・)_ﾊﾞｧﾝ…

参考：

- Git の Commit Author と Commiter を変更する  
[https://qiita.com/sea_mountain/items/d70216a5bc16a88ed932](https://qiita.com/sea_mountain/items/d70216a5bc16a88ed932)

- typoしてpushしてしまったコミットコメントを修正してpushしなおす方法  
[https://qiita.com/ykawakami/items/71b462057a8d714d7382](https://qiita.com/ykawakami/items/71b462057a8d714d7382)
