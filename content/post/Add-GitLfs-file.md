+++
date = "2022-08-03T16:00:00+09:00"
draft = false
title = "[Git] コミット済みのファイルを Git LFS 管理対象にしたいとき"
tags = ["git", "github", "gitlfs"]
+++

バイナリファイルコミットしちゃたけど、これ `Git LFS` 管理対象にしたかったよねー。。。

というときの作業メモです

わたしの環境は `Windows` での動作確認になります

1) `git lfs install` で `Git LFS` をインストールします  
わたしの環境にはすでに入っていたみたい…

```
>git lfs install
Updated Git hooks.
Git LFS initialized.
```

2) `git lfs version` でバージョンを確認しておく

```
>git lfs version
git-lfs/3.2.0 (GitHub; windows amd64; go 1.18.2)
```

3) `git lfs track` とすると、`.gitattributes` に格納されている対象データの情報が見れます

例えばこんな感じ

```
>git lfs track
Listing tracked patterns
    *.dll (.gitattributes)
    *.exe (.gitattributes)
```

4) `git lfs track` でLFS管理に入れたいファイルを指定します

```
>git lfs track "*.o"
Tracking "*.o"
```

ファイル単体を指定したければ、直接指定すればOK

```
>git lfs track "*aaa/bbb/ccc.bin"
```

5) `.gitattributes` に追加した対象ファイルが記載されているので  
それをコミット＆プッシュすればOK！

6) まだLFS管理してなかったときにコミットしてしまったファイルは  
いったん削除コミットをしてから、もう一度コミット＆プッシュするのが良さそうでした


---

（参考リンク）

- Git LFS を使ってみる - すらりん日記  
https://blog.techlab-xe.net/git-lfs-%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6%E3%81%BF%E3%82%8B/
- Git LFS について(オブジェクトとロック) - すらりん日記  
https://blog.techlab-xe.net/git-lfs-object-and-lock/
