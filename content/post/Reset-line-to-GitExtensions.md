+++
date = "2015-03-03T22:00:28+09:00"
draft = false
title = "[GitExtensions] 行単位で変更をリセットするRコマンド"
tags = ["GitExtensions", "Git"]
+++

**GitExtensions** を使ってみました


- [http://gitextensions.github.io/](http://gitextensions.github.io/)


GitExtensions では、commit 時に、行を選択して  
その選択部分だけをリセットする（コミット前に合わせる）という機能があります

- Commit - Git Extensions 2.48 documentation  
[http://git-extensions-documentation.readthedocs.org/en/latest/commit.html](http://git-extensions-documentation.readthedocs.org/en/latest/commit.html)


以下↓↓↓のような適当なファイルがコミットされていたとします


<img src="/pic/Reset-line-to-GitExtensions_00.png" style="border:solid 5px #e6e6e6"/>

ファイルの下に、変更を追加してみました

<img src="/pic/Reset-line-to-GitExtensions_01.png" style="border:solid 5px #e6e6e6"/>


Giet Extensionsを見ると、ファイルが変更されたので、Commit(1) になっています

<img src="/pic/Reset-line-to-GitExtensions_02.png" style="border:solid 5px #e6e6e6"/>

差分は、以下のような感じ  
追加した 「20」、 「100...」 の数字が、緑色の変更分として表れていますね

<img src="/pic/Reset-line-to-GitExtensions_03.png" style="border:solid 5px #e6e6e6"/>

実は、これは作成中のプログラムなどで、  
「100...」 の部分は、Commit には不要だったとします

そこで、以下のように、Commit には不要だなと思った部分を選択して [R] キーを押してみます

<img src="/pic/Reset-line-to-GitExtensions_04.png" style="border:solid 5px #e6e6e6"/>


以下↓↓↓のような感じで、ここの行消してもいいの？と聞いてくれます

<img src="/pic/Reset-line-to-GitExtensions_05.png" style="border:solid 5px #e6e6e6"/>

「はい」にすると、元のファイルから、選択した行を削除してくれます

<img src="/pic/Reset-line-to-GitExtensions_06.png" style="border:solid 5px #e6e6e6"/>

この動作は、Commitした/しないに関わらず、元ファイルに反映されます

「はい」を押したあとのファイルの状態↓↓↓

<img src="/pic/Reset-line-to-GitExtensions_07.png" style="border:solid 5px #e6e6e6"/>

あぁーあそこ、削除しとかなきゃ。。。  
というときに、元ファイルに戻ってから、再度コミットする、  
という手間が省けます

便利やね(^^)/
