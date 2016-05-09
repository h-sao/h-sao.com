+++
date = "2016-05-07T15:23:30+09:00"
draft = false
title = "[Git] Gitで利用するsshキーペアの作成、ssh接続、複数ID接続"
tags = ["Git", "GitHub", "GitExtensions", "ssh"]
+++


しょっちゅう作っては、作り方を忘れ、また検索で調べる…ということをしているので、sshキーの生成&接続について、忘れないようにメモしておきます

# GitサーバへSSHキーを使って接続したい

Gitサーバに接続するには、

- http
- ssh

この2種類の接続方法があるみたいです

Git接続用のクライアントアプリは、どっちで接続するか選択できるものもあるので  
Sourcetreeなどを使っていると、あまり認識してない人もいるかもしれません

今回、こんなクライアント環境で試しました

- 自端末：Windows10 x64
- Gitサーバ：GitHub
- Gitクライアントアプリ：GitExtensions
- Gitアクセスには、複数アカウントを利用している

これを試そうと思ったモチベーションは、push のたびに IDとパスワードを毎回聞かれるのがやだ、だからキーペアを利用して ssh アクセスにするんだー  
というところから始まってます

GitHub のIDとパスワードをスキップして、セキュアアクセスするのだ！という方の参考になればと思います！

> あと、番外編として、 非推奨ですが https アクセスでIDとパスワードを毎回聞かれないようにする方法も最後にメモしておきます


# キーの生成方法 ssh-keygen

ここは普通に  
公開鍵と秘密鍵を作ればいいだけなので、知ってる人に取ったら何をいまさら…になりますが、一応メモ  

Gitがインストールされているなら **ssh-keygen** が使えますので、コマンドを打つだけ  
(コマンドを打たなくても、Gitのクライアントアプリが勝手にやってくれる場合もあります)


GitBash などを起動します

<img src="/pic/Generate-ssh-key-for-GitHub_00.png" style="border:solid 5px #e6e6e6"/>

コマンドはこちら 

```
$ ssh-keygen -t rsa
```

基本的には、何か聞かれてもエンターで進めばOKです

デフォルトでは **c:\\Users\\[ユーザ名]\\.ssh\\** 以下に**秘密鍵（id_rsa）**と**公開鍵（id_rsa.pub）**のキーペアが作成されます

Windowsだとキーの保存位置は c:\\Users\\[ユーザ名]\\.ssh\\ にしないといけません

わたしは、Github用のキーだと判るように、名前を github_rsa として作成しました  
（既に別の用途で id_rsa を使っているからです、つまり複数アカウントを利用しています）

実行結果はこれ↓↓↓

<pre><div class="hljs">(SHA256以下の箇所は、適当に x で書き換えています)

haruka.sao@MyPC MINGW64 ~
<span class="hljs-keyword">$ ssh-keygen -t rsa</span>
Generating public/private rsa key pair.
<span class="hljs-string">Enter file in which to save the key (/c/Users/haruka.sao/.ssh/id_rsa):</span> /c/Users/haruka.sao/.ssh/github_rsa
<span class="hljs-string">Enter passphrase (empty for no passphrase):
Enter same passphrase again:</span>
Your identification has been saved in /c/Users/haruka.sao/.ssh/github_rsa.
Your public key has been saved in /c/Users/haruka.sao/.ssh/github_rsa.pub.
The key fingerprint is:
SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx haruka.sao@MyPC
The key's randomart image is:
+---[RSA 2048]----+
|      x          |
|      x          |
|      x          |
|      x          |
|      x          |
|      x          |
|      x          |
|      x          |
|      x          |
+----[SHA256]-----+

haruka.sao@MyPC MINGW64 ~
$
</div></pre>


キーペアができたできた

# SSHキーをGitHubに登録

c:\\Users\\[ユーザ名]\\.ssh\\ に保存された id_rsa.pub （わたしの場合はgithub_rsa.pub）を GitHubに登録します

自分のアカウントの **Settings > SSH and GPG keys > New SSH key** をクリックすると、先ほど作った公開鍵を登録することができます

<img src="/pic/Generate-ssh-key-for-GitHub_01.png" style="border:solid 5px #e6e6e6"/>

先ほど作った id_rsa.pub 公開鍵を登録しましょう！  
くれぐれも id_rsa の秘密鍵の方ではないので、お間違え無く！

id_rsa.pub の中身を見ればわかりますが、「**ssh-rsa ……**」 から始まっているファイルになります

<img src="/pic/Generate-ssh-key-for-GitHub_02.png" style="border:solid 5px #e6e6e6"/>

登録すると、こんな感じの記載になります

<img src="/pic/Generate-ssh-key-for-GitHub_03.png" style="border:solid 5px #e6e6e6"/>

最後に、自分の秘密鍵が c:\\Users\\[ユーザ名]\\.ssh\\ に設置されていることを再確認！

<img src="/pic/Generate-ssh-key-for-GitHub_04.png" style="border:solid 5px #e6e6e6"/>


# OpenSSH モード

GitExtensions で ssh 接続したい場合、  
私的にはお勧めなのは、OpenSSHモードにすることです

 ※ PuTTYアクセスは、PuTTYの独自フォーマットのキーを登録するなどが必要ですので、わたしは利用をやめました

<img src="/pic/Generate-ssh-key-for-GitHub_05.png" style="border:solid 5px #e6e6e6"/>

これで下準備はOK！

# PCから ssh で接続テスト

Git bash で接続テストしてみましょう

さっきの Git bash で、リポジトリの下まで移動します  
もしくは、GitExtensions から起動すると、初期ディレクトリはそのリポジトリの下になります


<img src="/pic/Generate-ssh-key-for-GitHub_08.png" style="border:solid 5px #e6e6e6"/>

確認パターンは２パターンあります


## (パターン1) id_rsa で登録している ssh キーを利用する場合

このコマンドで確認します

```xml
$ ssh -T git@github.com
```

**id_rsa** というファイル名を、ssh 接続では自動的に認識するようです

初回は、known_hosts に GitHub サーバを登録するよ？と聞いてくるので、 yes を入力します

> Hi [GitHubユーザ名]! You've successfully authenticated, but GitHub does not provide shell access.

が表示されればOKです

わたしの実行結果はこちら↓↓↓

<pre><div class="hljs">
<span class="hljs-keyword">$ ssh -T git@github.com</span>
The authenticity of host 'github.com (192.30.252.122)' can't be established.
RSA key fingerprint is xx:xx:xx:xx:xx:xx:
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,192.30.252.122' (RSA) to the list of known hosts.
<span class="hljs-string">Hi h-sao! You've successfully authenticated,</span> but GitHub does not provide shell access.

</div></pre>

known_hosts ファイルがこんな感じで作られました

<img src="/pic/Generate-ssh-key-for-GitHub_09.png" style="border:solid 5px #e6e6e6"/>



## (パターン2) 独自の名前で作った ssh キーを利用する場合

わたしの場合は、先ほど github_rsa という名前でキーを生成したので、こちらのパターンになりました

まず、 github_rsa という独自名称を ssh アクセス時に認識させる必要があります

c:\\Users\\[ユーザ名]\\.ssh\\ 直下に config という名前のファイルを作成します

> ※この部分は、Gitうんぬんは関係のない、ssh 接続の話です  
> 　Windows の ssh 接続では c:\\Users\\[ユーザ名]\\.ssh\\config はすべてのSSH接続で共有されます


c:\\Users\\[ユーザ名]\\.ssh\\config の内容はこんな感じです

<pre><div class="hljs">
Host <span class="hljs-string">my.github.server</span>
  HostName  <span class="hljs-string">github.com</span>
  Port      <span class="hljs-string">22</span>
  User      <span class="hljs-string">h-sao</span>
  IdentityFile  <span class="hljs-string">~/.ssh/github_rsa</span>
  TCPKeepAlive <span class="hljs-string">yes</span>
  IdentitiesOnly <span class="hljs-string">yes</span>

</div></pre>

- **Host**  
 アクセス識別子なので、どんな名前でもOK
- **HostName**  
 実際にアクセスするアドレス
- **Port**  
  アクセスするポート番号、ssh(Secure Shell)の番号  
  OpenSSHのデフォは22番なので、わざわざ書かなくてもいいのですが一応記載
- **User**  
  GitHubユーザ名を入れます
- **IdentityFile**  
  利用する ssh 秘密鍵ファイルのパス
- **TCPKeepAlive**：yes  
  これもデフォは yes なのですが、念のため  
- **IdentitiesOnly**：yes  
  .ssh/config の設定を増やしていくと「Too many authentication failures」が出ることがあるらしいです、変なエラーはやなので、あらかじめ設定しておきます



> ＜参考リンク＞
> 
> - SSHに公開鍵認証で接続する際に Too many authentication failures が出る - 6vox  
> [http://blog.6vox.com/2014/11/ssh-too-many-authentication-failures.html](http://blog.6vox.com/2014/11/ssh-too-many-authentication-failures.html)

> - OpenSSH ssf_configの設定項目 - Life with IT  
> [http://l-w-i.net/t/openssh/conf_001.txt](http://l-w-i.net/t/openssh/conf_001.txt)


ここまで準備したら、接続テストしましょう  
config に設定した名前で　Host の名称アクセスすることが出来ます！

```xml
$ ssh -T my.github.server
```

うまくいくと、パターン1と同じように、known hostに登録するかどうかを聞かれて、ファイルが作成されます  

<img src="/pic/Generate-ssh-key-for-GitHub_10.png" style="border:solid 5px #e6e6e6"/>


また、

> Hi [GitHubユーザ名]! You've successfully authenticated, but GitHub does not provide shell access.

こんな感じで、自分のGitHub名でアクセス出来たことが判ります  
いちおキャプチャ置いときます

<img src="/pic/Generate-ssh-key-for-GitHub_11.png" style="border:solid 5px #e6e6e6"/>


これで、クライアントPCから GitHub に IDとパスワードを利用せずに接続することが出来ました！

# まだGitExtensions から ssh アクセスは出来ない

さて、この状態で、GitExtensions のプッシュボタンを押して Pushしようとしても、IDとパスワードを聞かれます

<img src="/pic/Generate-ssh-key-for-GitHub_06.png" style="border:solid 5px #e6e6e6"/>

Pushボタンを押すと…

<img src="/pic/Generate-ssh-key-for-GitHub_07.png" style="border:solid 5px #e6e6e6"/>

GitHub のアカウントとパスワードを聞かれます…  
どうやら **https アクセスがデフォルト**みたいです

ちゃんと自身の Git リポジトリに ssh 接続するんだよー  

を認識させてあげないといけません

# Git リポジトリに ssh 接続設定を教える

Git リポジトリの設定を Git bash のコマンドで見ることが出来ます

```
$ git config -l
```

これらの設定は、Git リポジトリ直下にある .\\.git\\ 以下にあります

> この下の config ファイルなどを直接編集しても反映されますが、コマンドを使った方が良いでしょう

わたしの場合の実行結果はこちら

<pre><div class="hljs">
haruk@MACBOOKPROAKIKO ~/xxx (master)
<span class="hljs-keyword">$ git config -l</span>
core.symlinks=<span class="hljs-string">false</span>
core.autocrlf=<span class="hljs-string">true</span>
color.diff=<span class="hljs-string">auto</span>
color.status=<span class="hljs-string">auto</span>
color.branch=<span class="hljs-string">auto</span>
color.interactive=<span class="hljs-string">true</span>
pack.packsizelimit=<span class="hljs-string">2g</span>
help.format=<span class="hljs-string">html</span>
http.sslcainfo=<span class="hljs-string">/bin/curl-ca-bundle.crt</span>
sendemail.smtpserver=<span class="hljs-string">/bin/msmtp.exe</span>
diff.astextplain.textconv=<span class="hljs-string">astextplain</span>
rebase.autosquash=<span class="hljs-string">true</span>
user.name=<span class="hljs-string">Sao Haruka</span>
user.email=<span class="hljs-string">xxx@yyy.tmp.com</span>
core.autocrlf=<span class="hljs-string">True</span>
core.excludesfile=<span class="hljs-string">C:\Users\haruk\Documents\gitignore_global.txt</span>
core.editor=<span class="hljs-string">"C:/utils/GitExtensions/GitExtensions.exe" fileeditor</span>
merge.tool=<span class="hljs-string">kdiff3</span>
diff.guitool=<span class="hljs-string">kdiff3</span>
difftool.kdiff3.path=<span class="hljs-string">C:/utils/KDiff3/kdiff3.exe</span>
mergetool.kdiff3.path=<span class="hljs-string">C:/utils/KDiff3/kdiff3.exe</span>
core.repositoryformatversion=<span class="hljs-string">0</span>
core.filemode=<span class="hljs-string">false</span>
core.bare=<span class="hljs-string">false</span>
core.logallrefupdates=<span class="hljs-string">true</span>
core.symlinks=<span class="hljs-string">false</span>
core.ignorecase=<span class="hljs-string">true</span>
core.hidedotfiles=<span class="hljs-string">dotGitOnly</span>
remote.origin.url=<span class="hljs-keyword">https://github.com/h-sao/xxx.git</span>
remote.origin.fetch=<span class="hljs-string">+refs/heads/*:refs/remotes/origin/*</span>
branch.master.remote=<span class="hljs-string">origin</span>
branch.master.merge=<span class="hljs-string">refs/heads/master</span>

</div></pre>


上記の「**remote.origin.url**」が Git サーバにアクセスするときの URL になるので、これを ssh でアクセスするように変更します


デフォルトの id_rsa を利用するときは

**git@github.com:[ユーザID]/[リポジトリ].git**

を設定します

```xml
$ git remote set-url origin git@github.com:h-sao/xxx.git
```

id_rsa じゃない、別名の ssh キーファイルを利用するときの設定は

**[Host名]:[ユーザID]/[リポジトリ].git**

になります

```xml
$ git remote set-url origin my.github.server:h-sao/xxx.git
```

GitExtensions で Push してみましょう

<img src="/pic/Generate-ssh-key-for-GitHub_12.png" style="border:solid 5px #e6e6e6"/>

URL の表記がちょっと変わりましたね  
無事、IDとパスワードを聞かれることなく、プッシュが成功しているはずです＼(^o^)／  

<img src="/pic/Generate-ssh-key-for-GitHub_13.png" style="border:solid 5px #e6e6e6"/>


やったー


＜参考リンク＞

- gitHubでssh接続する手順~公開鍵・秘密鍵の生成から~ - Qiita  
 http://qiita.com/shizuma/items/2b2f873a0034839e47ce

# (番外編) httpsアクセスでIDとパスワードを聞かれないようにする

ええ、今回、本当に色々と試しましたとも…

ssh ではなく https アクセスで、毎回アカウント情報を入力しない方法も調べました

あまりセキュアじゃないので、お勧めできませんが  
一応記載しておきます

```
$ git config -l 
```

で調べた 「**remote.origin.url**」 の初期の記載はこれでした

<pre><div class="hljs">
remote.origin.url=<span class="hljs-keyword">https://github.com/h-sao/xxx.git</span>

</div></pre>

この https アクセスの URL 中に、IDとパスワードを埋め込めばOKです

**https://[ユーザID]:[パスワード]github.com/h-sao/xxx.git**

やってみたけど、パスワードが丸々画面に表示されるので、よくないです…  
確かに、ssh キーファイルなど用意しなくてもいいので、便利ではありますが…＞＜；

<img src="/pic/Generate-ssh-key-for-GitHub_14.png" style="border:solid 5px #e6e6e6"/>

利用は自己責任でお願いします

＜参考リンク＞

- Windowsにgitをインストールしてgithubにpushするまで - karakaram-blog  
[http://www.karakaram.com/git-install](http://www.karakaram.com/git-install)

## 追記記事書きました(2016/5/9)

- [SSH] 複数キー接続のconfig記載について  
[http://h-sao.com/blog/2016/05/09/add-ssh-config-for-git/](http://h-sao.com/blog/2016/05/09/add-ssh-config-for-git/)