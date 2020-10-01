+++
date = "2020-10-02T05:30:00+09:00"
draft = false
title = "[C++] 実行環境の差異によるintワーニングが出ない書き方"
tags = ["cpp"]
+++

C++のワーニングを放置していたのですが、そっと [@srz_zumix](https://twitter.com/srz_zumix) さんが教えてくれてました

忘れないうちに"φ(・ェ・o)~メモメモ


---

## x86環境とx64環境が混在してる場合にエラーが出やすい

こんなやつとか出ます…

```cpp
// Warning が出る例
std::vector<int> my_vec = { 0, 42 };
uint32_t max_loop_counter = my_vec.size();  // ←ここね
uint32_t counter = 0;

// 以下、counter を max_loop_counter までなんらかの処理
```

これは x86環境だと warning は出ないのですが
x64 環境想定でコンパイルすると、ワーニングが出ます


> (Clangの例)  
warning: Implicit conversion loses integer precision: 'std::\__1::vector<int, std::__1::allocator<int> >::size_type' (aka 'unsigned long') to 'u_int32_t' (aka 'unsigned int') [-Wshorten-64-to-32]

(*￣□￣*;  
ええ、ええ、判っていますとも…  
int 型が処理系によって異なるのは…

そこで、やっつけで修正するならこれ

```cpp
// ベタにワーニングを取ろうとするとこんな感じ
uint32_t max_loop_counter = static_cast<uint32_t>(my_vec.size());
```

なのですが、もっとスマートは方法があります

```cpp
// 今っぽい感じに
std::vector<int> my_vec = { 0, 42 };
auto max_loop_counter = my_vec.size();
decltype(max_loop_counter) counter = 0;
```

エレガントになりました…！素晴らしい(^^♪


## 警告のコンパイラー対応表がある

- [C++] 警告のコンパイラー対応表を作り始めました - ブログズミ  
[https://srz-zumix.blogspot.com/2020/09/c.html](https://srz-zumix.blogspot.com/2020/09/c.html)

- [https://github.com/srz-zumix/awesome-cpp-warning](https://github.com/srz-zumix/awesome-cpp-warning)

[@srz_zumix](https://twitter.com/srz_zumix) さんが最近始められたプロジェクトらしいです

ワーニング対応表欲しいな…と思っていたところなので、結構良いかもしれない…

そして今日もC++修練は続く…