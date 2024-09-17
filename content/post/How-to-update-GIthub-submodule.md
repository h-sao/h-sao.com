+++
date = "2024-09-18T06:30:00+09:00"
draft = false
title = "[Git] サブモジュールの更新メモ"
tags = ["git", "GitHub"]
+++

最近は自分で苦労して調べたことをブログにメモしなくても  
何度でもAIさんが根気よく同じことを嫌がらずに教えてくれるので  
自分のブログメモが減ってしまいました…

しかしさすがに同じことを何回聞いてるねん！？となったので、メモしておきます

リポジトリ内にサブモジュールがあるときの更新方法メモです

## 手順

`develop/build00` のサブモジュール "libs/XXX" のブランチ `master` を紐付けたいとする

既にサブモジュールブランチ XXX の `master` は更新済み & Push済みとします

`develop/build00` がみんなの作業ブランチの場合、PR用のブランチ `maintenance/update-submodule00` などを作って更新対象にする

```
# 作業前におかしな状態になってないかどうかを確認する
git status

# メインリポジトリのブランチをチェックアウト
git checkout develop/build00
git pull

# 目的のブランチに居てるよね？の確認
# （この場合develop/build00に居る）
git branch

# 念のためサブモジュールを更新
git submodule update --init --recursive

# PRのための新しいブランチを作成
git checkout -b maintenance/update-submodule00

# 今のブランチは maintenance/update-submodule00 のはず
git branch

# libs/XXX サブモジュールに移動
cd libs/XXX

# サブモジュールの目的のブランチをチェックアウト
git checkout master
git pull

# 目的のブランチに居てるよね？の確認
# （この場合masterに居る）
git branch

# メインリポジトリに戻る
cd ../..

# しつこいけど、今のブランチは maintenance/update-submodule00 のはず
git branch

# メインリポジトリでサブモジュールの新しい状態をcommit
git add libs/XXX
git commit -m "Update libs/XXX submodule"

# メインリポジトリの変更をプッシュ
git push origin maintenance/update-submodule00

# 最後に Github 上で maintenance/update-submodule00 から develop/build00 へPRを出す

```

こんな感じですね
