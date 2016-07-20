+++
date = "2016-07-20T20:00:00+09:00"
draft = false
title = "[GitHub] GitHubリポジトリを別アカウントに移動させる"
tags = ["Github"]
+++

旧 Github Micro Plan では、プライベートリポジトリは5つまでしか持てませんでしたが  
最近は Presonal Plan に変更されて、リポジトリ制限数が無制限になりましたね

- GitHub価格ページ  
[https://github.com/pricing](https://github.com/pricing)

それを受けて、手元で持っている複数の Micro Plan アカウントを、一つのアカウントにまとめる作業を行いました


あるGithubのリポジトリを別アカウントに移管する、という意味になります

> 移動元個人アカウント(XXX)のリポジトリ(X-REPO)  
> 　　　↓  
> 移動先個人アカウント(YYY)


ドキドキしたけど、結構すんなり出来たので、別にドキドキする必要なかった(^^;)

とはいえ、わたしの様な小心者が、他にもいらっしゃるかもしれないので  
（あと、忘れた頃の未来の自分のため^^;）  
手順を詳細に載せときます

---
# GiuHub上での操作(移動元)

1. 移動元(XXX)のアカウントで GitHub にログインします

1. 移動元(XXX)のGitHubのリポジトリ(X-REPO)の **Settings＞ Options** に行きます  
 <img src="/pic/Transfer-GitHub-repository_00.png" style="border:solid 5px #e6e6e6"/>

1. 一番下に Denger Zone があるので、そこの **Transfer** をクリックして、移動させます  
（まだ、いきなりは移動しないよ）  
![](/pic/Transfer-GitHub-repository_01.png)

1. **移動元(XXX)のリポジトリ名(X-REPO)**、**移動先のアカウント名(YYY)** を入力します  
 <img src="/pic/Transfer-GitHub-repository_02.png" style="border:solid 5px #e6e6e6"/>  
これで、リポジトリを移動させる準備が出来ました

1. 移動元リポジトリ(X-REPO)は、移動先アカウント(YYY)の許可待ち状態になります  
![](/pic/Transfer-GitHub-repository_03.png)  
この状態であれば、Abort transferを押して、まだ移管を取り消すことが出来ますね！

# その後、移動先アカウント(YYY)にメールが届く

1. こんなメールが移動先アカウント(YYY)に届きます

```xml
＜タイトル＞
[GitHub] Repository transfer from @XXX (XXX/X-REPO)

＜内容＞ 
Hello YYY,

@XXX wants to transfer the XXX/X-REPO repository to YYY/X-REPO. To accept the transfer, visit this link:

    (★ここにリンクが記載されている)

If you don’t accept the transfer, it’ll expire in about a day.

Questions? Problems? Email support@github.com any time.

Thanks,
Your friends at GitHub

```

ここで★のリンクをクリックすると、突然の移動完了です  
お気を付けください(^_^;)

というわけで、ドキドキしながら★をクリックします

# すぐに、移動元アカウント(XXX)にメールが届く

1. 移動元アカウント(XXX)の方に、通知メールが届きます

```xml

YYY added you to X-REPO

You can now push to this repository.

---
View it on GitHub:
https://github.com/YYY/X-REPO

```

これで、完全に移動が完了しました

---

結構すんなりすぎて、あっけない感じです

issue、Collaborators など、リポジトリに必要な情報がまるっと移動できるので、とても便利でした

