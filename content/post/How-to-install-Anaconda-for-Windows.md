+++
date = "2017-03-29T20:30:00+09:00"
draft = false
title = "[Anaconda] Anaconda for Windowsインストール方法"
tags = ["Anaconda", "Python"]
+++

ちょっとPCのグラフィックボードを新しくしたので、TensorFlowでなんかモデルでも作ってみるかということで、Pythonを使うためにAnacondaをインストールしました

初回のインストールでは何も問題が起きなかったのですが、Anacondaのバージョンを上げようとして、なんか色々やってるうちに、エラーにはまったので、その解決メモです  
（ただし、あまり良い解決方法じゃありません、ごめんなさい）

# Anacondaのインストール

わたしの環境は以下です

- Windows10 x64
- NVIDIA GeForce GTX 960
- Anaconda 4.3.1 For Windows

このAnacondaは**Python 3.6** に対応しています

Anaconda 4.3系は Pythonの 2.7, 3.4, 3.5, 3.6 をサポートしている様です

- **ANACONDA CHANGELOG** - 2017-01-31 4.3.0 Highlightsより  
[https://docs.continuum.io/anaconda/changelog](https://docs.continuum.io/anaconda/changelog)


実は、わたしは何度もAncondaのインストールとアンインストールを繰り返しました  
すると、再インストール時にはエラーメッセージ

> **Failed to create anaconda menus**

がインストール時に出るようになってしまいました  
インストールする権限の問題の様です  
おかしいなぁ…  
Administrator権限で起動してるのですが、出てしまいます…（´-`）.｡oO

-----

これは、

- 再インストールするディレクトリを変える
- Just me インストール
- Administrator権限で起動

この3点をすれば、怒られずにインストールできたのですが…

わたしのインストールしたいディレクトリは c:\utils\Anaconda3 なのです！

インストールしたいディレクトリを変更するって…ちょっとヤダなぁと思い  
ダメもとで

- PCのインストール時に作った Administrator 権限のユーザでログイン
- All Users インストール
- 更にAdministrator権限で起動

にしてみたら、うまく同じインストールディレクトリに  
再インストールすることが出来ました  
あまり良い解決方法じゃないけど…もう権限周りで困りたくないです＞＜。

あなこんだ for Windows 。。。難しいよ(*´ω｀*)

インストーラーとWindowsの権限周りは、少し問題が残っている印象でした

# TensorFlow用の開発環境を切る

今の状態でダイレクトに作業しても良いんですけど  
他のプロジェクトや要件が出たときに困らないために、TensorFlow用の開発環境を切っておきます

## (1)**conda info -e** コマンドで確認します
インストール直後の環境は root 環境しかありません


```
(C:\utils\Anaconda3) C:\Users\(インストールユーザ名)>conda info -e
# conda environments:
#
root                  *  C:\utils\Anaconda3
```

## (2)**conda create** でAnacondaのTensorflow用環境を作ります  

引数：

- name: tensorflow (任意です) 
- python: **3.5** (ここ重要！)

2017/3/29現在、公式では **TensorFlowはPython 3.5系しかサポートしてない**らしいです

- **Installing TensorFlow on Windows - TensorFlow公式**  
[https://www.tensorflow.org/install/install_windows](https://www.tensorflow.org/install/install_windows)

公式の言う通り、3.5を指定することにします

```c
(C:\utils\Anaconda3) C:\Users\(インストールユーザ名)>conda create --name=tensorflow python=3.5
Fetching package metadata ...........
Solving package specifications: .

Package plan for installation in environment C:\utils\Anaconda3\envs\tensorflow:

The following NEW packages will be INSTALLED:

    pip:            9.0.1-py35_1
    python:         3.5.3-0
    setuptools:     27.2.0-py35_1
    vs2015_runtime: 14.0.25123-0
    wheel:          0.29.0-py35_0

Proceed ([y]/n)? y

python-3.5.3-0 100% |###############################| Time: 0:00:07   4.27 MB/s
setuptools-27. 100% |###############################| Time: 0:00:00   4.24 MB/s
wheel-0.29.0-p 100% |###############################| Time: 0:00:00   4.35 MB/s
pip-9.0.1-py35 100% |###############################| Time: 0:00:00   4.06 MB/s
#
# To activate this environment, use:
# > activate tensorflow
#
# To deactivate this environment, use:
# > deactivate tensorflow
#
# * for power-users using bash, you must source
#


(C:\utils\Anaconda3) C:\Users\(インストールユーザ名)>
```

## (3)**tensorflow** という環境が出来ました  
さっきの condaコマンドで確認します  

```
(C:\utils\Anaconda3) C:\Users\(インストールユーザ名)>conda info -e
# conda environments:
#
tensorflow               C:\utils\Anaconda3\envs\tensorflow
root                  *  C:\utils\Anaconda3
```

## (4)作った tensorflow環境に切り替えます

**activate**コマンドを使います  
ちなみに抜けたい時は、**deactivate**です（が、わたしはあまり使う機会がないかな…）

```c
(C:\utils\Anaconda3) C:\Users\(インストールユーザ名)>activate tensorflow

(tensorflow) C:\Users\(インストールユーザ名)>
```

前に、作った環境の名前 **(tensorflow)** が付くので、判りやすいですね！

## (余談)起動ディレクトリを変更する

> C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Anaconda3 (64-bit)

わたしの場合、ここスタートアップの場所に Anaconda Prompt が入ってました

Anaconda Prompt のプロパティを開いて、開始するディレクトリを変更すればOKです

<img src="/pic/How-to-install-Anaconda-for-Windows_00.png" style="border:solid 5px #e6e6e6"/>

わたしは起動ディレクトリを、D:\dev\anaconda に変えました

-----

ふー  
たったこんだけなんですけどねぇ…（´-`）.｡oO  
まごまごしてるわたし…  

気を取り直して、次回は、**TensorFlow on Windows**のインストール準備をします

＜その他参考＞

- Anaconda 4.3.1 For Windows (公式)  
[https://www.continuum.io/downloads](https://www.continuum.io/downloads)

- [Python]Anacondaで仮想環境を作る  
[http://qiita.com/supersaiakujin/items/50def6f33b79f9a61b18](http://qiita.com/supersaiakujin/items/50def6f33b79f9a61b18)
