+++
date = "2021-12-01T00:30:00+09:00"
draft = false
title = "[C++] About C++ Modules"
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

- gcc
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

## モジュールをバイナリ化する

モジュールを記載する .ixx ファイルは、.ifc にプリコンパイルでバイナリ化されます

ややこしいのが、コンパイラによってこのあたりの呼び名が異なってて、こんな感じ

| | MSVC | Clang |
| ---- | ---- | ---- |
| モジュールファイル名　　→ | .ixx | .cppm |
| モジュールファイルをプリコンパイルしたもの→ | .ifc | .pcm |

ちなみに「モジュールファイルをプリコンパイルしたもの」を **BMI** と呼び、  
Binary Module Interface の略になります

この BMI ファイルを各 cpp ファイルなどが import することになります  


## VC++でのコンパイル方法

単純のため、まずはコマンドラインでやってみます  
コマンドは２つ

```bash
> cl /c /std:c++20 /EHsc hello.ixx
> cl /std:c++20 /EHsc /reference MyHello=MyHello.ifc main.cpp hello.obj
```

1つ目のコマンドでモジュールをプリコンパイルし、  
2つ目のコマンドで obj をリンク＆ main.exe の出力をします

＜1. モジュールのプリコンパイル＞

```bash
C:\my\dev\sample_module01>cl /c /std:c++20 hello.ixx
Microsoft (R) C/C++ Optimizing Compiler Version 19.30.30705 for x86
Copyright (C) Microsoft Corporation.  All rights reserved.

hello.ixx

```
これで、hello.obj と MyHello.ifc が出来ました

> cl.exe はモジュールのコンパイル時に同時に二つのファイルを出力してくれます（.obj と .ifc）

> Clang は別々のコマンドで出力します  
つまり .pcm ( VC++で言うところの .ifc) を出力した後、それを元に .o ( VC++の .obj) を出力します


＜2. 実行ファイル生成＞


```bash
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

main.exe が出来ました

実行してみます

```bash
C:\my\dev\sample_module01>main.exe
42

```

問題なし


## コンパイルオプションの注意点

VC++のコマンドラインオプションが
- モジュールを作ったとき
- モジュールを使うとき
これらの間で一致していないとき、コンパイラはワーニングを出しました（C5050）

> main.cpp(7): warning C5050: Possible incompatible environment while importing module 'MyHello': _CPPUNWIND is defined in current command line and not in module command line

そのため今回、hello.ixx 内では `<iostream>` は利用していないのですが、`/EHsc` オプションを付けています


（参考）
- Using C++ Modules in MSVC from the Command Line Part 1: Primary Module Interfaces - C++ Team Blog  
[https://devblogs.microsoft.com/cppblog/using-cpp-modules-in-msvc-from-the-command-line-part-1/](https://devblogs.microsoft.com/cppblog/using-cpp-modules-in-msvc-from-the-command-line-part-1/)



## BMIってなによ？

Modules を使うと、プリコンパイルされた Binary Module Interface というファイルが突如現れましたが、この中身ってどうなってるんでしょうね？

Modules の提案を推し進めている Gabriel Dos Reisさんによると、  
中身はASTとObjみたいなものになってるらしいです

- IFCバイナリ形式のモデル  
[A Principled, Complete, and Efficient Representation of C++ Gabriel Dos Reis and Bjarne Stroustrup](https://www.stroustrup.com/gdr-bs-macis09.pdf)


もちろん、コンパイラによって形式は様々なんでしょう

## じゃそのモジュール配布できるんじゃない？

と思ったのですが、モジュールはあくまでヘッダファイルの代替品なので、ライブラリのように配布はできないのです。。。

コンパイラによってAST＋αに解析されるので…おそらく同じコンパイラでもバージョンによって内容が異なったりするかもしれません


- [Practical Cpp Modules - CppCon 2019](https://github.com/CppCon/CppCon2019/blob/master/Presentations/practical_cpp_modules/practical_cpp_modules__boris_kolpackov__cppcon_2019.pdf)

