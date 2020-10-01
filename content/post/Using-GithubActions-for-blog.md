+++
date = "2020-09-28T12:00:00+09:00"
draft = false
title = "[Github] Github Actionsでブログを更新するようにしました"
tags = ["github"]
+++

元々、このブログは `hugo` で作った静的HTMLを `wercker` でオートデプロイして作っていました  
だけど `wercker` は設定の変更が多くてすぐにデプロイ出来なくなっていたんですよね~で、辞めました  
これを作った当時は `wercker` がイケイケドンドンだったので  
`CircleCI` から乗り換える人も多かったのですが  
今となってはカジュアルデプロイは `CircleCI` …いやいや `Github Actions` があるやないか！  
ということで、 `Github Actions` でデプロイするようにしてみました

## Github Actions の設定ON

ここをポチっと押すと、Actionsの設定画面へ

<img src="/pic/Using-GithubActions-for-blog_00.png" style="border:solid 5px #e6e6e6"/>

## Github Actions の設定ファイルを書いてみる

知識なしで初めてみましたが、すんなり簡単にビルド＆デプロイすることが出来ました

というのも、`GitHub Actions for GitHub Pages` というデプロイするためのオールインワンアクションを公開してくれている方がいらっしゃいました  
設定ファイルをまんま真似するだけで使えます

- [https://github.com/peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)

そしてこの方、`hugo` 対応のアクションも公開して下さっています！

- [https://github.com/peaceiris/actions-hugo](https://github.com/peaceiris/actions-hugo)

ここの例に書いてる `yaml` ファイルのまんまで良い感じにデプロイ出来ました

こんな感じ↓↓↓

- [https://github.com/h-sao/h-sao.com/blob/master/.github/workflows/actions.yml](https://github.com/h-sao/h-sao.com/blob/master/.github/workflows/actions.yml)

```sh
# This is a basic workflow to help you get started with Actions

name: Deploy Github Pages to Hugo

# Controls when the action will run. Triggers the workflow on push or pull request
# events but only for the master branch
on:
  push:
    branches: [ master ]

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2
      
      - name: Hugo setup
        uses: peaceiris/actions-hugo@v2
        with:
          # The Hugo version to download (if necessary) and use. Example: 0.58.2
          hugo-version: 0.54.0 
          # Download (if necessary) and use Hugo extended version. Example: true
          # extended: true 

      # Runs a single command using the runners shell
      - name: Build Hugo
        run: hugo -v

      # Runs a set of commands using the runners shell
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

超カンタンでした(。・_・。)ノ

## CNAME 対応

Github Pages でカスタムドメインを使う際には  
Github Pages の公開ブランチ（わたしの場合 `gh-pages` ）のルートにドメイン名を記載した `CNAME` ファイルを設置する必要があるのですが  
これが `peaceiris/actions-gh-pages` を使ってデプロイした際に、消えてしまってました

アクションの方の設定で対応することも出来たようなのですが  
わたしはHugoそのものの設定を変えて、対応しました

Hugo の変換元のディレクトリに `static/CNAME` を設置するだけです  
これも超カンタン

- [https://gohugo.io/hosting-and-deployment/hosting-on-github/#use-a-custom-domain](https://gohugo.io/hosting-and-deployment/hosting-on-github/#use-a-custom-domain)

## 感想

SSHキーの外部登録などもしなくて良いし、Github の中で閉じてビルド＆デプロイ出来るのが良いですね！

コネコネしたいときは `CircleCI` とか使うかもなぁと思いつつ、Github へのアクションメインの自動化であれば、これで充分ですね  
今後も使っていくと思います~ 


## 参考

- ポートフォリオサイトを Hugo で Github Pages + Github Actions で構築する話  
[https://mather.github.io/posts/2019/12/08/hugo_with_github_pages/](https://mather.github.io/posts/2019/12/08/hugo_with_github_pages/)

- Hugoで生成したコードをgithub pagesへデプロイするたびにカスタムドメインが初期化される  
[https://blog.app.melvins-nest.com/posts/lost-cname/](https://blog.app.melvins-nest.com/posts/lost-cname/)
