+++
date = "2016-05-15T23:30:00+09:00"
draft = false
title = "[C++] VisualStusio2015でClangを使う設定"
tags = ["cpp", "Clang"]
+++

今日は Clang with Microsoft CodeGen の設定などについて、メモしておきます

先人たちが既に色々と試している内容と対して変わりませんが、そもそも日本語の情報も少ないので、何かの足しになればと思います

# "Clang with Microsoft CodeGen" is here!

最近はすっかり、クロスプラットフォームやオープンソースに力を入れているマイクロソフト社のツールセットの中に、Clang対応ってのがあります

2015年の年末にですが、Clang が正式に Visual Studio 2015 Update1 で利用できるよ～  
とVC++チームブログで発表されていました  


- Clang with Microsoft CodeGen in VS 2015 Update 1 - Visual C++ Team Blog  
[https://blogs.msdn.microsoft.com/vcblog/2015/12/04/clang-with-microsoft-codegen-in-vs-2015-update-1/](https://blogs.msdn.microsoft.com/vcblog/2015/12/04/clang-with-microsoft-codegen-in-vs-2015-update-1/)  

- Clang with Microsoft CodeGen (March 2016) released- Visual C++ Team Blog  
[https://blogs.msdn.microsoft.com/vcblog/2016/03/31/clang-with-microsoft-codegen-march-2016-released/](https://blogs.msdn.microsoft.com/vcblog/2016/03/31/clang-with-microsoft-codegen-march-2016-released/)

良いですねー(^^)  
現在のVS2015には **Clang with Microsoft CodeGen** というツールセットが提供されており、目玉はなんといっても、Clang のデバックを Visual Studioのエディターで確認できるところでしょうか！

他にも、Clang でコンパイルしたobjと VC でコンパイルしたobjがリンクできるところもすごいです  
既存資産をフル活用出来そうですね


>（2016/5/15現在、Visual Studio 2015 Update2 が最新です）  
> [https://www.visualstudio.com/ja-jp/news/vs2015-update2-vs.aspx](https://www.visualstudio.com/ja-jp/news/vs2015-update2-vs.aspx)


-----

LLVM側にも記事が上がっています

- Getting Started with the LLVM System using Microsoft Visual Studio - LLVM本家  
[http://llvm.org/docs/GettingStartedVS.html](http://llvm.org/docs/GettingStartedVS.html)

-----

日本でも既に先人が試されているので、読んでみると良いと思います

- Clang/C2をコマンドプロンプトで使ってみる - イグトランスの頭の中（のかけら）  
[http://dev.activebasic.com/egtra/2015/12/09/850/](http://dev.activebasic.com/egtra/2015/12/09/850/)

- Clang with Microsoft CodeGenがでたので試す - C++初心者Advent Calendar 2015 9日目  
[http://qiita.com/yumetodo/items/bd41f31f39dd590e8c80](http://qiita.com/yumetodo/items/bd41f31f39dd590e8c80)

-----

ちなみに、この Clang with Microsoft CodeGen は VC++チームブログにも "a.k.a. Clang/C2" と記載されており、別名を **Clang/C2** というようです

また、今回対応している **Clang 側のバージョンは 3.7** です

# clang.exe の場所

### Clang

```xml
C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\Clang 3.7\bin\x86\clang.exe
```

### ちなみにVC++

```xml
C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin\cl.exe
```


どちらでも、コマンドプロンプトでコンパイル可能です


# VSでのコンパイラの切り替え設定


Visual Stidon 2015 上で "Alt + F7" を押すか  

> プロジェクト > [プロジェクト名]のプロパティ  

で設定UIを表示させます

> 構成プロパティ > 全般 > プラットフォームツールセット 

この項目を "Clang 3.7 with Microsoft CodeGen (v140_clang_3_7)" に変更するだけです


<img src="/pic/Use-Clang-on-VisualStudio_00.png" style="border:solid 5px #e6e6e6"/>

結構簡単＼(^o^)／

# ソリューション作成は何でもOK

上記の様に、コンパイラが簡単に切り替えられるので、空のプロジェクトでも、Win32コンソールアプリケーションでもOKです

一応、Clangっぽいプロジェクトはあるんですけど、いきなり dll というのもなんだか敷居が高いので、

<img src="/pic/Use-Clang-on-VisualStudio_01.png" style="border:solid 5px #e6e6e6"/>

exe を作りたいだけなら、Win32コンソールアプリケーションで良いと思います

# [余談]文字コードはUTF8の方が良い(様な気がする)

最初に書いておくと、Sift-JIS でも Clang/C2 でコンパイルは通ります

…なのですが、一応、なんとなく


個々の環境によって異なるのかもしれませんが  
わたしがVS2015で作成した cpp ファイルは、デフォルトで Sift-JIS になります

既に、[VC++チームブログ](https://blogs.msdn.microsoft.com/vcblog/2015/12/04/clang-with-microsoft-codegen-in-vs-2015-update-1/) にはこんな感じで↓↓↓


> If source files in the project you are trying to convert are UTF-16 encoded, you will need to resave them as UTF-8 when Clang gives you an error about UTF-16 encoded source with BOM.

UTF-16はだめらしいです  
Sift-JIS は無関係かもしれませんが、一応…


# サンプルコード

[cpprefjp](http://cpprefjp.github.io/) を参照して、サンプルコードを書いてみました

- constexprの制限緩和
[http://cpprefjp.github.io/lang/cpp14/relaxing_constraints_on_constexpr.html](http://cpprefjp.github.io/lang/cpp14/relaxing_constraints_on_constexpr.html)

VC2015 Update2 では未対応だけど、Clang 3.7 は対応しているというコードです

```cpp
#include "stdafx.h"
#include <iostream>

constexpr int f()
{
	int result = 0;
	return result;
}

int main()
{
	std::cout << "Hello: " << f() << std::endl;

	return 0;
}
```

## Visual Studio 2015 (v140) でコンパイルすると、エラー

![](/pic/Use-Clang-on-VisualStudio_02.png)

VS2015 ではまだ、constexpr関数内の変数宣言がエラーになる様です

## Clang 3.7 with Microsoft CodeGen (v140_clang_3_7) でのコンパイル


<img src="/pic/Use-Clang-on-VisualStudio_03.png" style="border:solid 5px #e6e6e6"/>

Clang はOKです  
いい感じに動作しました！

-----

VC++も Clang も、VS上でデバック出来るのは良いですね  
噂には聞いていましたが、自分で動作させてみると、感激するもんがあります

今はまだプレビューらしいので、早く安定化して欲しいですね～