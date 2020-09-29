+++
date = "2019-07-28T18:00:00+09:00"
draft = false
title = "[Hugo] Hugoの環境をDocker上で動かす"
tags = ["Hugo", "Docker", "mac"]
+++

今回の環境

- macOS Mojave
- Docker Desktop
- Hugo

このブログは [Hugo](https://gohugo.io/) というスタティックジェネレーターで作っています

Hugoは golang で動いているので、ローカルに Go の環境が必要なわけですが  
以前、MacBookの中にダイレクトに Hugo をインストールしていたら、別で入っているGoのバージョンと、うまくかみ合わなくなったことがありました

解決に結構時間が取られちゃったので、もう macOSを使うときには、なんでもかんでも docker で分離しようと心に決めたのでした…

その方法です

やってみたら、超簡単でした

---

### Hugo に対応した docker image をダウンロード

{{< highlight bash "hl_lines=2" >}}
# Hugo で docker を検索
sao$ docker search hugo
NAME                  DESCRIPTION                            STARS    OFFICIAL  AUTOMATED
jojomi/hugo     hugo, see https://gohugo.io                     44               [OK]
publysher/hugo  Docker base image for static sites generated…   37               [OK]
monachus/hugo   Docker image for building and running Hugo (…   28               [OK]
klakegg/hugo    Minimal image and variants with batteries in…   22               [OK]
cibuilds/hugo   Docker image for Hugo, the static-site gener…   10  
…
…（いっぱいあった）
{{< / highlight >}}

今回わたしは、***klakegg/hugo*** のバージョンを使うことにしました

- ***klakegg/hugo*** - docker hub  
[https://hub.docker.com/r/klakegg/hugo](https://hub.docker.com/r/klakegg/hugo)

- ***klakegg/hugo*** - GitHub  
[https://github.com/klakegg/docker-hugo](https://github.com/klakegg/docker-hugo)


{{< highlight bash "hl_lines=2" >}}
# ダウンロード
sao$ docker pull klakegg/hugo
{{< / highlight >}}

### 起動

{{< highlight bash "hl_lines=2 5" >}}
# ローカルのマークダウンファイルの場所に移動
sao$ cd h-sao.com/

# まんまこのコマンドでOK
h-sao.com sao$ docker run --rm -it -v $(pwd):/src -p 1313:1313 klakegg/hugo:0.55.0 server

# ブラウザで http://localhost:1313 にアクセスするとOK
{{< / highlight >}}

超超超簡単でした。。。

これからも mac環境では docker さまに当分お世話になりますm(_ _)m

