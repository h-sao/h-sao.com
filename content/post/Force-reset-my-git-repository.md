+++
date = "2014-02-03T18:54:41+09:00"
draft = false
title = "[Git] ローカルを強制上書きして、作業をなかったことにしたい"
tags = ["git"]
+++

ローカルの作業が何やらおかしくなったから  
リモートのファイル内容に戻したい～という時  
以下のコマンドでさくっと戻りました 

```cpp
git fetch origin
git reset --hard origin/master
```

ツールなどで強制的にsyncさせようとしても

> failed to sync this branch 

という悲しいお知らせが出て、結局 Git Bash に行くことになりますので……

（参考リンク）

- gitでリモートのブランチにローカルを強制一致させたい時  
[http://qiita.com/ms2sato/items/72b48c1b1923beb1e186](http://qiita.com/ms2sato/items/72b48c1b1923beb1e186)

- How to reset my local repository to be just like the remote repository HEAD  
[http://stackoverflow.com/questions/1628088/how-to-reset-my-local-repository-to-be-just-like-the-remote-repository-head](http://stackoverflow.com/questions/1628088/how-to-reset-my-local-repository-to-be-just-like-the-remote-repository-head)
