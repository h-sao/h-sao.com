+++
date = "2022-05-22T16:00:00+09:00"
draft = false
title = "[git] macでgitを初めて使う時"
tags = ["git", "mac", "xcode"]
+++

mac で初めてコマンドから git を使う時の設定メモです

- わたしの環境
  - MacBook Pro 16-inch, 2019
    - macOS Monterey

## git コマンドが使えなかった

こんなエラーが出ました。。。

```bash
% git 
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
% 
```

開発者ツールが入ってないらしい

## Xcode をインストールする

```bash
% xcode-select --install
code-select: note: install requested for command line developer tools
% 
```

こんな感じでインストールが進みます

![](/pic/Using-git-command-line-on-mac_00.png)

![](/pic/Using-git-command-line-on-mac_01.png)

![](/pic/Using-git-command-line-on-mac_02.png)

実際のダウンロードは、わたしの環境では20分くらい、インストールは10分くらいでした

![](/pic/Using-git-command-line-on-mac_03.png)

そしてインストール完了のウィンドウが出てほっ…

## Gitコマンドが使えるように

```bash
% git                   
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

These are common Git commands used in various situations:
...
```

無事、コマンドラインから git が使えるようになりました〜