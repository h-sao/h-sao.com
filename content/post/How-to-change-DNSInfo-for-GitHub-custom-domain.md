+++
date = "2020-02-25T12:00:00+09:00"
draft = false
title = "[GitHub Pages] カスタムドメインのDNS情報を更新する"
tags = ["GitHub"]
+++

去年、2019年の4月くらいから  
GitHub Pages にファイルをアップすると、ワーニングが出るようになった

内容はこんなやつ

```xml
Title: [h-sao/h-sao.com] Page build warning

The page build completed successfully, but returned the following warning for the `gh-pages` branch:

The custom domain for your GitHub Pages site is pointed at an outdated IP address. You must update your site's DNS records if you'd like it to be available via your custom domain. For more information, see https://help.github.com/en/github/working-with-github-pages/configuring-a-custom-domain-for-your-github-pages-site.

For information on troubleshooting Jekyll see:

  https://help.github.com/articles/troubleshooting-jekyll-builds

If you have any questions you can contact us by replying to this email.
```

(\*￣-￣)ふ～ん、よくわからんけど  
何やらDNSの設定を変えないといけないのね

めんどくさー  
後で後で…と思い続けて早10か月ほど…

さすがに対応しないと、自分のブログがアクセスできなくなるかもと焦り、重い腰を上げました

## (前提条件)このブログの運用情報

- [https://h-sao.com/](https://h-sao.com/) は GitHub Pages の上で動いてる
- `h-sao.com` ドメイン自体の管理は [Valueドメイン](https://www.value-domain.com/) を利用してる
- 動いているリポジトリは [https://github.com/h-sao/h-sao.com](https://github.com/h-sao/h-sao.com)
  - リポジトリの `master` ブランチには `Hugo` で記載したのテンプレートファイルやマークダウンで記載したコンテンツ情報を置いてる
  - 同じく `gh-pages` ブランチは、実際にサイトとして公開している html/css ファイルを置いてる
  - （おまけ）`master` に記事のマークダウンを `push` すると  
`Webhooks` に登録している `wercker` に通知が送られ  
`Hugo` でhtml生成 ＆ `gh-pages` に htmlファイルなどをオートで `push` する  
というちょっとした自動化の仕組みになってる

## カスタムドメインを GitHub Pages に割り当てる方法

調べてみると全然難しくなくて  
上記のワーニングメールに書かれてたURLにアクセスしたら、全てが書かれてました

ただわたしにとっては真剣に読む気で読まないと頭の中スルー…って感じの内容だったので、ちょっと真剣に読んでみました

英語版はこれ↓↓↓なのですけど

[https://help.github.com/en/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site](https://help.github.com/en/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site)

ご丁寧に日本語版も提供されています

- カスタムドメインとGitHub Pagesについて  
[https://help.github.com/ja/github/working-with-github-pages/about-custom-domains-and-github-pages](https://help.github.com/ja/github/working-with-github-pages/about-custom-domains-and-github-pages)

これを見ながらやってみます

まずわたしのGitHub Pagesの設定画面は、こんな感じで黄色いワーニングが出てました

![](/pic/How-to-change-DNSInfo-for-GitHub-custom-domain_00.png)

今までずーっと無視してきましたが、いよいよ解決する日が来ましたよ  

わたしが割り当てているカスタムドメインは **Apex ドメイン** になっています  
（ www.h-sao.com ではなく、 h-sao.com としてアクセス）

日本語版の「カスタムドメインとGitHub Pagesについて」の説明を読んでみると、  
**「GitHub Pages サイト用のカスタムドメインを管理する」を参照してください。** とあるので、素直にそのリンクに飛びます

- 「GitHub Pages サイト用のカスタムドメインを管理する」  
[https://help.github.com/ja/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain](https://help.github.com/ja/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain)

ここにありました！ Apexドメインを設定する方法が！  
`A` レコードのIPアドレスが書いてありました

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

あと、最初の「カスタムドメインとGitHub Pagesについて」には、こんな記載もありました

> サイトには、カスタムドメインの一方または両方を設定できます。 **Apex ドメインを使用している場合でも、www サブドメインを使用することをおすすめします。**

ふむふむ、www は付けるのを推奨してるのね、了解  
`cname` も追加してみました

```
# 設定するAレコード
a @ 185.199.108.153
a @ 185.199.109.153
a @ 185.199.110.153
a @ 185.199.111.153

# サブドメインも設定しておく
cname www h-sao.github.io.
```

Valueドメインの設定画面  
<img src="/pic/How-to-change-DNSInfo-for-GitHub-custom-domain_03.png" style="border:solid 5px #e6e6e6"/>

これを設定すると、わたしの場合はちょうど１５分後に設定が反映されました

![](/pic/How-to-change-DNSInfo-for-GitHub-custom-domain_01.png)

よかったー！