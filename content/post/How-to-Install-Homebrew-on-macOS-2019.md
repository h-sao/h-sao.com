+++
date = "2019-05-30T12:00:00+09:00"
draft = false
title = "[macOS] Homebrewインストールログ2019版"
tags = ["mac"]
+++

以前、2015年にも **Homebrew** をインストールしたログをこのブログに残していました  
その時よりは、ずいぶん手順が楽になってたので、もう一度載せておきます

- （以前に載せていたインストールログの様子）[Mac] Homebrewインストールログ - Effectiveさお  
[http://h-sao.com/blog/2015/05/11/install-log-for-homebrew/](http://h-sao.com/blog/2015/05/11/install-log-for-homebrew/)


今回は、これの2019年度版です  
2019/5/30現在、Homebrew のバージョンは 2.1.4 でした

- Homebrewの本家  
[http://brew.sh/](http://brew.sh/)


macOSでのインストールログ（長いので省略版）

{{< highlight bash "hl_lines=2 6 16 18 66" >}}
# Homebrew入ってない状態
SAO-MAC:~ sao.haruka$ brew -V
-bash: brew: command not found

# Homebrew本家に載っているインストールコマンドを入力！
SAO-MAC:~ sao.haruka$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
==> This script will install:
/usr/local/bin/brew
/usr/local/share/doc/homebrew
/usr/local/share/man/man1/brew.1
   ...(省略)
/usr/local/Caskroom
/usr/local/Homebrew
/usr/local/Frameworks

Press RETURN to continue or any other key to abort
==> /usr/bin/sudo /bin/chmod u+rwx /usr/local/bin /usr/local/lib
Password:
==> /usr/bin/sudo /bin/chmod g+rwx /usr/local/bin /usr/local/lib
==> /usr/bin/sudo /usr/sbin/chown sao.haruka /usr/local/bin /usr/local/lib
   ...(省略)
==> /usr/bin/sudo /usr/bin/chgrp admin /Users/sao.haruka/Library/Caches/Homebrew
==> Downloading and installing Homebrew...
remote: Enumerating objects: 20, done.
remote: Counting objects: 100% (20/20), done.
remote: Compressing objects: 100% (17/17), done.
remote: Total 123873 (delta 5), reused 13 (delta 3), pack-reused 123853
Receiving objects: 100% (123873/123873), 29.26 MiB | 6.32 MiB/s, done.
Resolving deltas: 100% (90744/90744), done.
From https://github.com/Homebrew/brew
 * [new branch]      master     -> origin/master
 * [new tag]         0.1        -> 0.1
 * [new tag]         0.2        -> 0.2
 * [new tag]         0.3        -> 0.3
   ...(省略) 
 * [new tag]         2.1.3      -> 2.1.3
 * [new tag]         2.1.4      -> 2.1.4
HEAD is now at 7c3bb36ca Merge pull request #6181 from Homebrew/dependabot/bundler/Library/Homebrew/ruby-progressbar-1.10.1
==> Homebrew is run entirely by unpaid volunteers. Please consider donating:
  https://github.com/Homebrew/brew#donations
==> Tapping homebrew/core
Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core'...
remote: Enumerating objects: 4996, done.
remote: Counting objects: 100% (4996/4996), done.
remote: Compressing objects: 100% (4787/4787), done.
remote: Total 4996 (delta 51), reused 809 (delta 16), pack-reused 0
Receiving objects: 100% (4996/4996), 4.01 MiB | 4.19 MiB/s, done.
Resolving deltas: 100% (51/51), done.
Tapped 2 commands and 4782 formulae (5,038 files, 12.5MB).
Already up-to-date.
==> Installation successful!

==> Homebrew has enabled anonymous aggregate formulae and cask analytics.
Read the analytics documentation (and how to opt-out) here:
  https://docs.brew.sh/Analytics

==> Homebrew is run entirely by unpaid volunteers. Please consider donating:
  https://github.com/Homebrew/brew#donations
==> Next steps:
- Run `brew help` to get started
- Further documentation: 
    https://docs.brew.sh
SAO-MAC:~ sao.haruka$ 

# インストールしたバージョンの確認
SAO-MAC:~ sao.haruka$ brew --version
Homebrew 2.1.4
Homebrew/homebrew-core (git revision 8bdd; last commit 2019-05-30)
SAO-MAC:~ sao.haruka$ 
{{< / highlight >}}

インストールログの完全版はgistに載せています  

- Install_log_Homebrew_on_macOS  
[https://gist.github.com/h-sao/c6ad49171ba1ff6b11cffab67730a9fe](https://gist.github.com/h-sao/c6ad49171ba1ff6b11cffab67730a9fe)

途中はキーの入力を1回、パーミッション変更のためのパスワード入力を1回、聞かれるだけでした  
コマンド一つで berw が使えるようになったので、とても楽でした
