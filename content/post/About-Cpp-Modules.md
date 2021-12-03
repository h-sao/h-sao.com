+++
date = "2021-12-01T00:30:00+09:00"
draft = false
title = "[C++] Modulesのコンパイル（MSVC ver）とBMIについて"
tags = ["cpp"]
+++


> この記事は [C++ Advent Calendar 2021](https://qiita.com/advent-calendar/2021/cxx) 1日目（初日！）の記事です。

少し前からC++に **Modules** がやってきました  
C++20 対応のメジャーどころのコンパイラ（MSVC/gcc/Clang など）で使うことができます

個人的にはビックウェーブが来たーーーと思ってまして、つねづねポチポチと Modules について調べていました

それを少しまとめたいと思います

## モジュールの説明

昔ながらのプリコンパイルヘッダの概念を、今風にした感じでしょうか

ヘッダファイルをインクルードしていたものを Modules に置き換えることが出来ます

ヘッダファイルだと、  

インクルードの順番に気を付けたり  
コンパイル時間が長くなったり  
インクルードガード書いたりと  
ゆーてローテク文字列だったのですが、

それがバイナリとして公式に提供されました

## Modulesを記載するファイル

各コンパイラによって（推奨）拡張子が異なります

- MSVC (cl.exe)
 - Foo.ixx  

- Clang
 - Foo.cppm

- GCC
  - Foo.cc
  - Foo.cp
  - Foo.cxx
  - Foo.cpp
  - Foo.c++
  - Foo.C

（参考）

- MSVC  
[Overview of modules in C++](https://docs.microsoft.com/en-us/cpp/cpp/modules-cpp?view=msvc-170)
- GCC  
[GCC and File Extensions - Development with GNU/Linux](http://labor-liber.org/en/gnu-linux/development/index.php?diapo=extensions)
https://blog.feabhas.com/2021/08/c20-modules-with-gcc11/

以下、、MSVCで話を進めます  
（コンパイラによって異なる所が多いので）

## モジュールファイルについて

MSVC の場合、モジュールにしたいファイルは **.ixx** という拡張子を付けます  
（Clangだと .cppm、GCCだと .cxx とかになります）

サンプルはこんな感じ

hello.ixx

```cpp
// hello.ixx:
export module MyHello;

export int f() {
    return 42;
 }
```

利用する側はこんな感じで↓↓↓ **import** すれば使えます  
（従来は `#include "hello.h"` とか書いていた場所です！）


```cpp
// main.cpp 
#include <iostream>
#include <cstdlib>

import MyHello;  // モジュール使うよの宣言

int main() {
  auto a = f();

  std::cout << f();
}
```

サンプルだけ見ると、なんてことのないコードですね  
メイン関数は `#include` してた箇所を `inport` に変更しただけです

## モジュールをバイナリ化する

モジュールを記載する `.ixx` ファイルは、`.ifc` にプリコンパイルでバイナリ化されます

ややこしいのが、コンパイラによってこのあたりの呼び名が異なってて、こんな感じ

| | MSVC | Clang |
| ---- | ---- | ---- |
| モジュールファイル名　　→ | `.ixx` | `.cppm` |
| モジュールファイルをプリコンパイルしたもの→ | `.ifc` | `.pcm` |

ちなみに「モジュールファイルをプリコンパイルしたもの」を **BMI** と呼び、  
**Binary Module Interface** の略になります

この BMI ファイルを各 cpp ファイルなどが `import` することになります  

（参考）

- Practical Cpp Modules - CppCon 2019　　
[https://github.com/CppCon/CppCon2019/blob/master/Presentations/practical_cpp_modules/practical_cpp_modules__boris_kolpackov__cppcon_2019.pdf](https://github.com/CppCon/CppCon2019/blob/master/Presentations/practical_cpp_modules/practical_cpp_modules__boris_kolpackov__cppcon_2019.pdf)


## VC++でのコンパイル方法

単純のため、まずはコマンドラインでやってみます  

利用したVC++バージョンはこちら

![](/pic/About-Cpp-Modules_00.png)


コマンドは２つ

```bash
> cl /c /std:c++20 /EHsc hello.ixx
> cl /std:c++20 /EHsc /reference MyHello=MyHello.ifc main.cpp hello.obj
```

1つ目のコマンドでモジュールをプリコンパイルし、  
2つ目のコマンドで `obj` をリンク、そして `main.exe` の出力をします

＜1. モジュールのプリコンパイル＞

```
C:\my\dev\sample_module01>cl /c /std:c++20 hello.ixx
Microsoft (R) C/C++ Optimizing Compiler Version 19.30.30705 for x86
Copyright (C) Microsoft Corporation.  All rights reserved.

hello.ixx

```
これで、`hello.obj` と `MyHello.ifc` が出来ました

> （余談）cl.exe はモジュールのコンパイル時に同時に２つ（`.obj` と `.ifc`）のファイルを出力してくれて楽です

> Clang は別々のコマンドで出力するので、cl.exe とは少し作り方が異なっています  
`.pcm` ( VC++で言うところの `.ifc`) を出力した後、それを元に `.o` ( VC++の `.obj`) を出力します  
このあたりはまだ次回に投稿したいです  


＜2. 実行ファイル生成＞


```
C:\my\dev\sample_module01>cl /std:c++20 /EHsc /reference MyHello=MyHello.ifc main.cpp hello.obj
Microsoft (R) C/C++ Optimizing Compiler Version 19.30.30705 for x86
Copyright (C) Microsoft Corporation.  All rights reserved.

main.cpp
Microsoft (R) Incremental Linker Version 14.30.30705.0
Copyright (C) Microsoft Corporation.  All rights reserved.

/out:main.exe
main.obj
hello.obj
```

`main.exe` が出来ました

実行してみます

```
C:\my\dev\sample_module01>main.exe
42

```

問題なし(^^♪


## （備考）コンパイルオプションの注意点

VC++のコマンドラインオプションが

- モジュールを作ったとき
- モジュールを使うとき

これらの間で一致していないとき、ワーニングが出ました（`C5050`）

> main.cpp(7): warning C5050: Possible incompatible environment while importing module 'MyHello': _CPPUNWIND is defined in current command line and not in module command line

そのため今回、`hello.ixx` 内では `<iostream>` は利用していないのですが、`/EHsc` オプションを付けています


（参考）

- Using C++ Modules in MSVC from the Command Line Part 1: Primary Module Interfaces - C++ Team Blog  
[https://devblogs.microsoft.com/cppblog/using-cpp-modules-in-msvc-from-the-command-line-part-1/](https://devblogs.microsoft.com/cppblog/using-cpp-modules-in-msvc-from-the-command-line-part-1/)



## BMIってなによ？

プリコンパイルされた Binary Module Interface って何なのでしょうか？  
この中身は一体？

さんざん調べましたが、大した情報が載ってないですね。。。  

これ、BMIというのは Modules を扱うための概念のようです

これはコンパイルの過程のものなので、各コンパイラベンダーが作る箇所であり、明確に公開情報として詳細が提示されているわけではなさそうです  
現時点では概念がちらほらと記載されているだけのようでした

実際に、プリコンパイルで作られた `.ifc` や `.pcm`  の中身は

- プリプロセスで利用した情報とか
- ASTのダンプ
- Objみたいなもの

がひとまとめにされてるようです

~~という記述を見つけましたが、出展元が判らなくなったので、思い出したら追記します。。。~~

> (2021/12/4追記)  
CppCon2019の資料が参考になりました！  
Practical Cpp Modules - CppCon 2019　　
[https://github.com/CppCon/CppCon2019/blob/master/Presentations/practical_cpp_modules/practical_cpp_modules__boris_kolpackov__cppcon_2019.pdf](https://github.com/CppCon/CppCon2019/blob/master/Presentations/practical_cpp_modules/practical_cpp_modules__boris_kolpackov__cppcon_2019.pdf)



もちろん、コンパイラによって、さらにバージョンによっても形式は様々なんでしょう



## モジュールの配布ってどうやるの？

モジュールのバイナリを配布することはできないです  
共有する場合はあくまでソースコードと共に配布になります

また、作られた BMI、つまり `.ifc` や `.pcm` ファイルは不変的なバイナリではないみたいです

ライブラリのように配布はできないのですね。。。

> （余談）Modules の提案を推し進めている [Gabriel Dos Reisさん](https://cppcast.com/modules-gaby-dos-reis/) は、Common Module Interface Format を作りたいみたいですが…  
具体的に今の段階では、配布目的のものは出ていないようです

（参考）

- C++ Modules: What You Should Know  
[https://github.com/cppp-france/CPPP-19/blob/master/C%2B%2B_modules_what_you_should_know-Gabriel_Dos_Reis/C%2B%2B_modules_what_you_should_know-Gabriel_Dos_Reis.pdf](https://github.com/cppp-france/CPPP-19/blob/master/C%2B%2B_modules_what_you_should_know-Gabriel_Dos_Reis/C%2B%2B_modules_what_you_should_know-Gabriel_Dos_Reis.pdf]

## 所感

まだ2021年12月の時点では、Modules 機能が完全に動作するコンパイラもないみたいですし  
そもそも各コンパイラによって記載方法や推奨が異なるので、なんとも道半ばな印象はあります

Modules を利用するための手順やお作法も多く、また情報も限られているので、取り組みにくいですね

マイクロソフトの [VC++チームブログ](https://devblogs.microsoft.com/cppblog/) は、コンパイラベンダーとして結構 Moduleds の具体的な情報を出しているように感じました  
（Clangももうちょっと頑張って。。。あとGCC。。。おまえはやる気あるのか。。。）

Modules に関して取り組まれている人たちの歴史はとても長く  
かつ現在のヘッダファイルがベストだとは思えないので  
個人的に一押ししたい機能だと改めて思いました

## 所感その２

C++ Advent Calendar！

ずいぶん昔に一度だけ参加したことがあったのですが、それからなーんにもしてませんでした

今回、意を決して投稿できて良かったです

かなりよく調べたつもりですけど、間違いなどあったら気軽に [@hr_sao](https://twitter.com/hr_sao) まで教えていただけると嬉しいです

C++楽しい～♪
