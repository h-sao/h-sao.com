+++
date = "2019-05-29T12:00:00+09:00"
draft = false
title = "[Docker] Docker for Macをインストールして Ubuntu の起動や停止まで"
tags = ["mac", "docker"]
+++

Macbook Pro に Docker をインストール＆テスト稼動させた時のメモ

> 数年前までわたしはmacのこと本当によく知らなかったので、初めて mac を使ったときには、こういったインストール方法もわからなかった…  
> 誰かの役に立つかも知れないので、初歩的なところも書いておきます  
> 勉強のためのメモなので、用語の使い方が間違っている可能性があります  
> 間違いあれば、遠慮なく @hr_sao まで教えてもらえると嬉しい＞＜

# Docker for Mac のインストール開始

(1) このURLからインストールします  

- Install Docker Desktop for Mac  
[https://docs.docker.com/docker-for-mac/install/](https://docs.docker.com/docker-for-mac/install/)

(2)  Docker.dmg をダウンロードします

dmg ファイルは Windows で言う所の iso ファイルです  
ダブルクリックしたら、マウント状態になります  

(3) インストールします

マウントした直後はディレクトリが自動で開くので、  
その中の Docker のクジラアイコンを、Applications フォルダにドラッグ＆ドロップすれば、インストールは完了

![](/pic/How-to-install-docker-for-mac_00.png)

(4) マウント解除します

その後は Docker.dmg のマウントを解除し（デスクトップにできたアイコンから解除できる）  
ダウンロードした dmg ファイルはゴミ箱にポイしてOK


> これだけの事なんだけど、最初は全く判らなかった…   
何年もWindowsやLinux系OSで仕事してるのに、
***Docker のクジラアイコンを、Applications フォルダにドラッグ＆ドロップ***  
この作業の意味が、ほんと、よく判らなかった＞＜。  
イラストの矢印の通り、クジラアイコンを、イラストの上にドロップしようとして何もおこらないのに腹を立ててました  
ユニバーサルデザインなんだろうけど、作業者にある程度の前提知識は必要なデザインなんだと思う  
もちろん、「**Applications フォルダに** ドラッグ＆ドロップ」に気がつかない私が悪いんですけど

(5) Dockerを初回起動させます

Applicationフォルダの中の Docker をダブルクリック！

![](/pic/How-to-install-docker-for-mac_08.png)

(6) インストールの様子


![](/pic/How-to-install-docker-for-mac_01.png)

![](/pic/How-to-install-docker-for-mac_02.png)

![](/pic/How-to-install-docker-for-mac_03.png)

↓メニューバーにクジラのアイコンが現れ、running になってるのを教えてくれます

![](/pic/How-to-install-docker-for-mac_04.png)

↓バージョンを確認してみました（2019/5/29現在は Version 2.0.0.3）

![](/pic/How-to-install-docker-for-mac_05.png)

↓ Preferences から利用するリソースの設定ができます

![](/pic/How-to-install-docker-for-mac_07.png)

さて、これで準備完了( * ´ω`* )

# Ubuntu を起動させてみる

macOSのターミナルを起動します

以下、作業の流れをそのまま記載しています

- ubuntuのイメージを検索

{{< highlight bash "hl_lines=2 6" >}}
# ubuntuのイメージはまだ持っていない
SAO-MAC:~ sao.haruka$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE

# ubuntuをsearch
SAO-MAC:~ sao.haruka$ docker search ubuntu
NAME                                                      DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
ubuntu                                                    Ubuntu is a Debian-based Linux operating sys…   9572                [OK]                
dorowu/ubuntu-desktop-lxde-vnc                            Docker image to provide HTML5 VNC interface …   303                                     [OK]
rastasheep/ubuntu-sshd                                    Dockerized SSH service, built on top of offi…   217                                     [OK]
consol/ubuntu-xfce-vnc                                    Ubuntu container with "headless" VNC session…   179                                     [OK]
ubuntu-upstart                                            Upstart is an event-based replacement for th…   98                  [OK]                
ansible/ubuntu14.04-ansible                               Ubuntu 14.04 LTS with ansible                   97                                      [OK]
neurodebian                                               NeuroDebian provides neuroscience research s…   57                  [OK]                
1and1internet/ubuntu-16-nginx-php-phpmyadmin-mysql-5      ubuntu-16-nginx-php-phpmyadmin-mysql-5          50                                      [OK]
ubuntu-debootstrap                                        debootstrap --variant=minbase --components=m…   40                  [OK]                
i386/ubuntu                                               Ubuntu is a Debian-based Linux operating sys…   18                                      
1and1internet/ubuntu-16-apache-php-5.6                    ubuntu-16-apache-php-5.6                        14                                      [OK]
ppc64le/ubuntu                                            Ubuntu is a Debian-based Linux operating sys…   13                                      
1and1internet/ubuntu-16-apache-php-7.0                    ubuntu-16-apache-php-7.0                        13                                      [OK]
1and1internet/ubuntu-16-nginx-php-phpmyadmin-mariadb-10   ubuntu-16-nginx-php-phpmyadmin-mariadb-10       11                                      [OK]
1and1internet/ubuntu-16-nginx-php-5.6                     ubuntu-16-nginx-php-5.6                         8                                       [OK]
1and1internet/ubuntu-16-nginx-php-5.6-wordpress-4         ubuntu-16-nginx-php-5.6-wordpress-4             7                                       [OK]
1and1internet/ubuntu-16-apache-php-7.1                    ubuntu-16-apache-php-7.1                        6                                       [OK]
darksheer/ubuntu                                          Base Ubuntu Image -- Updated hourly             5                                       [OK]
1and1internet/ubuntu-16-nginx-php-7.0                     ubuntu-16-nginx-php-7.0                         4                                       [OK]
1and1internet/ubuntu-16-apache-php-5.6-zend-2             ubuntu-16-apache-php-5.6-zend-2                 2                                       [OK]
pivotaldata/ubuntu                                        A quick freshening-up of the base Ubuntu doc…   2                                       
1and1internet/ubuntu-16-php-7.1                           ubuntu-16-php-7.1                               1                                       [OK]
smartentry/ubuntu                                         ubuntu with smartentry                          1                                       [OK]
1and1internet/ubuntu-16-sshd                              ubuntu-16-sshd                                  1                                       [OK]
pivotaldata/ubuntu-gpdb-dev                               Ubuntu images for GPDB development              0                                       
{{< / highlight >}}

- 公式のubuntuのイメージを手元にダウンロードする

{{< highlight bash "hl_lines=2 13" >}}
# ubuntuをpull
SAO-MAC:~ sao.haruka$ docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
6abc03819f3e: Pull complete 
05731e63f211: Pull complete 
0bd67c50d6be: Pull complete 
Digest: sha256:f08638ec7ddc90065187e7eabdfac3c96e5ff0f6b2f1762cf31a4f49b53000a5
Status: Downloaded newer image for ubuntu:latest
SAO-MAC:~ sao.haruka$ 

# imagesの中にubuntuが入ってる
SAO-MAC:~ sao.haruka$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              7698f282e524        12 days ago         69.9MB
SAO-MAC:~ sao.haruka$ 
{{< / highlight >}}

- ubuntuの起動と起動確認

{{< highlight bash "hl_lines=2 13" >}}
# dockerでubuntuサーバを何も考えずに起動
SAO-MAC:my_docker sao.haruka$ docker run -it ubuntu

# docker経由でubuntuにアクセスできた！
root@387ef5142f09:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@387ef5142f09:/# 
{{< / highlight >}}

このままexitするとubuntuが止まってしまうので、`control + P、Q` で抜ける

{{< highlight bash "hl_lines=2" >}}
# dockerでubuntuが起動してることを確認
SAO-MAC:my_docker sao.haruka$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
387ef5142f09        ubuntu              "/bin/bash"         4 minutes ago       Up 4 minutes                            dazzling_leakey
SAO-MAC:my_docker sao.haruka$ 
{{< / highlight >}}

- 起動中のubuntuに再接続

{{< highlight bash "hl_lines=2 6" >}}
# さっきのubuntunに再ログイン(コンテナIDを使ってログインしてみる)
SAO-MAC:my_docker sao.haruka$ docker attach 387ef5142f09
root@387ef5142f09:/# 

# exitで抜ける
root@387ef5142f09:/# exit
exit
{{< / highlight >}}

- 先ほどexitしたのでubuntuが停止していることの確認

{{< highlight bash "hl_lines=2 6" >}}
# 起動中のコンテナの確認（何も動いてない）
SAO-MAC:my_docker sao.haruka$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

# 停止しているコンテナも表示させてみる (Exited状態)
SAO-MAC:my_docker sao.haruka$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
387ef5142f09        ubuntu              "/bin/bash"         8 minutes ago       Exited (0) 10 seconds ago                       dazzling_leakey
SAO-MAC:my_docker sao.haruka$ 
{{< / highlight >}}

- 停止させたコンテナを再起動させる

{{< highlight bash "hl_lines=2 6" >}}
# 停止したものを再起動させるときは start コマンドで(今度はコンテナ名でログインしてみる)
SAO-MAC:my_docker sao.haruka$ docker start dazzling_leakey
dazzling_leakey

# コンテナが起動してる
SAO-MAC:my_docker sao.haruka$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
387ef5142f09        ubuntu              "/bin/bash"         10 minutes ago      Up 3 seconds                            dazzling_leakey
SAO-MAC:my_docker sao.haruka$ 
{{< / highlight >}}

- ubuntu内のexitではなく、docker側からコンテナを停止させる

{{< highlight bash "hl_lines=2" >}}
# ubuntu停止
SAO-MAC:my_docker sao.haruka$ docker stop dazzling_leakey
dazzling_leakey
SAO-MAC:my_docker sao.haruka$ 
{{< / highlight >}}

- テスト操作を終えたので、作ったコンテナを削除する  
（もし作業内容を保存したければ、commitでイメージを作れるけどそれはまた次回以降）

{{< highlight bash "hl_lines=2" >}}
# コンテナはExited状態で停止している
SAO-MAC:my_docker sao.haruka$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
387ef5142f09        ubuntu              "/bin/bash"         2 hours ago         Exited (0) 6 minutes ago                       dazzling_leakey

# コンテナを削除
SAO-MAC:my_docker sao.haruka$ docker rm dazzling_leakey
dazzling_leakey

# ubuntuコンテナが無くなった
SAO-MAC:my_docker sao.haruka$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
SAO-MAC:my_docker sao.haruka$
{{< / highlight >}}

ふぅ、、、これで Docker 初学者になれたかしら

＜参考＞

色々なサイトがありましたが、わたしの理解が一番進んだのは、以下のサイトでした

- 初心者による初心者のためのDocker入門 その１ dockerコマンド編 - Qiita  
[https://qiita.com/k5n/items/2212b87feac5ebc33ecb](https://qiita.com/k5n/items/2212b87feac5ebc33ecb)