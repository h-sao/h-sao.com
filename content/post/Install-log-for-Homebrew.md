+++
date = "2015-05-11T03:05:44+09:00"
draft = false
title = "[Mac] Homebrewインストールログ"
tags = ["Mac"]
+++

**Homebrew** とは、Mac上のアプリケーションパッケージを管理するソフトです

詳しくはこちら↓↓↓に書かれているのですが、自分のメモとログを残します

- MacにHomebrewをインストールする - Qiita  
[http://qiita.com/is0me/items/475fdbc4d770534f9ef1](http://qiita.com/is0me/items/475fdbc4d770534f9ef1)

- Homebrewの本家  
[http://brew.sh/](http://brew.sh/)


Macでのインストールログ

```
## Homebrew入ってない状態
MacProSao:~ Sao$ brew
-bash: brew: command not found
 
## VIMでバッシュプロファイル作成
MacProSao:~ Sao$ vim .bash_profile

MacProSao:~ Sao$ more .bash_profile 
export PATH=/usr/local:$PATH

MacProSao:~ Sao$ pwd
/Users/Sao

## /usr/local の作成 
MacProSao:usr Sao$ sudo mkdir /usr/local/

WARNING: Improper use of the sudo command could lead to data loss
or the deletion of important system files. Please double-check your
typing when using sudo. Type "man sudo" for more information.

To proceed, enter your password, or type Ctrl-C to abort.

Password:
MacProSao:usr Sao$ ls -al
total 8
drwxr-xr-x@   12 root  wheel    408  5 11 02:24 .
drwxr-xr-x    30 root  wheel   1088  5 11 02:17 ..
drwxr-xr-x     5 root  wheel    170  9 10  2014 X11
lrwxr-xr-x     1 root  wheel      3  2 22 18:01 X11R6 -> X11
drwxr-xr-x  1053 root  wheel  35802  5  2 23:30 bin
drwxr-xr-x   257 root  wheel   8738  5 11 01:25 include
drwxr-xr-x   271 root  wheel   9214  5 11 01:25 lib
drwxr-xr-x   170 root  wheel   5780  4 26 13:43 libexec
drwxr-xr-x     2 root  wheel     68  5 11 02:24 local
drwxr-xr-x   244 root  wheel   8296  4 26 13:40 sbin
drwxr-xr-x    44 root  wheel   1496  5 11 01:25 share
drwxr-xr-x     4 root  wheel    136  2 22 17:56 standalone

## Homebrewのインストール
MacProSao:usr Sao$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
==> This script will install:
/usr/local/bin/brew
/usr/local/Library/...
/usr/local/share/man/man1/brew.1
==> The following directories will be made group writable:
/usr/local/.
==> The following directories will have their group set to admin:
/usr/local/.

Press RETURN to continue or any other key to abort
==> /usr/bin/sudo /bin/chmod g+rwx /usr/local/.
==> /usr/bin/sudo /usr/bin/chgrp admin /usr/local/.
==> /usr/bin/sudo /bin/mkdir /Library/Caches/Homebrew
==> /usr/bin/sudo /bin/chmod g+rwx /Library/Caches/Homebrew
==> Downloading and installing Homebrew...
remote: Counting objects: 3572, done.
remote: Compressing objects: 100% (3421/3421), done.
remote: Total 3572 (delta 35), reused 1459 (delta 18), pack-reused 0
Receiving objects: 100% (3572/3572), 2.72 MiB | 2.47 MiB/s, done.
Resolving deltas: 100% (35/35), done.
From https://github.com/Homebrew/homebrew
 * [new branch]      master     -> origin/master
HEAD is now at c007efc Riak 2.1.1
==> Installation successful!
==> Next steps
Run `brew help` to get started

## プロファイル読み込み 
MacProSao:~ Sao$ source .bash_profile 

## バージョン確認
MacProSao:~ Sao$ brew -v
Homebrew 0.9.5
 
MacProSao:~ Sao$ 
```

Hugoを入れるためにインストールしてみました  
ついにわたしもGoを使う日が来るなんて…(ﾟ∇ﾟ ;)