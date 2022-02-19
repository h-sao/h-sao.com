+++
date = "2022-02-19T09:00:00+09:00"
draft = false
title = "[C++] ラムダのパラメータリスト()が省略できるようになった"
tags = ["cpp"]
+++


> この記事は [meetup app osaka@6](https://connpass.com/event/240210/) の記事です。

C++23 でタイトルの通り、パラメータリストのカッコが省略できるようになりました

- Make () more optional for lambdas  
[http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2020/p1102r2.html](P1102R2)

gccなら12以上、Clangは14以上で動作するようです

```cpp
[=]
// ()    // mutable 書いてても省略可能に！
mutable
{}
```

(もう少し追記するとおもいますが、今はこれで…)
