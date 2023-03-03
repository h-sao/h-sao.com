+++
date = "2023-3-03T15:00:00+09:00"
draft = false
title = "[C++] C++の新しいパターンマッチ"inspect" "
tags = ["cpp"]
+++

この記事は [meetup app osaka@7 online](https://meetupapp.connpass.com/event/276301/) への参加記事です

先日 2023/2/20 ごろにISO WG21 C++ 委員会メンバーのAntony PolukhinさんがMedium(Yandexサイト)で
「C++23が完成したよ～」と記事を書かれていました

- C++23 Is Finalized. Here Comes C++26  
[https://medium.com/yandex/c-23-is-finalized-here-comes-c-26-1677a9cee5b2](https://medium.com/yandex/c-23-is-finalized-here-comes-c-26-1677a9cee5b2)

それでちょっと新しいC++の機能には何があるのかなと調べていたところ  
知らない文法が増えそうな予感の「Pattern Matching」`inspect` を見つけたので  
それについてメモしてます

> これはどこのC++に入るかは決まってないので、将来的には採用されないかもしれないです

---

＜試した環境＞

- Compiler Explorer  
[https://godbolt.org/](https://godbolt.org/)
  - c++ compiler  
 `x86-64 clang (experimental pattern matching)`
  - option  
 `-std=c++20`

- サンプルコードはこちら↓↓↓  
[https://godbolt.org/z/a95M9nxPY](https://godbolt.org/z/a95M9nxPY)

---

もともとパターンマッチするといえば、こんな感じで `switch case` を使ってると思います

```c++
int x(42);
switch (x) {
    case 0: std::cout << "zero"; break;
    case 42: std::cout << "forty-two"; break;
    default: std::cout << "something else";
}

// output
// "forty-two"
```

これが、`inspect` を用いるとこんな感じに書けます

```c++
// number
int x(42);
inspect(x) {
    0 => { std::cout <<  "zero"; }
    42=> { std::cout <<  "forty-two"; }
    _=> { std::cout <<  "something else"; }
};

// output
// "forty-two"
```

文字列マッチもこんな感じに

```c++
// string
std::string s = "foo";
inspect (s) {
    "foo" => { std::cout << "got foo";}
    "bar" => { std::cout << "got bar";}
    __ => { std::cout << "don't care";}
};

// output
// "foo"
```

タプル的な値のマッチも比較的少ない行数で書ける

```c++
// tuple
auto box = std::pair(0, 42);
auto&& [i, j] = box;
inspect (box) {
    [0, 0]=> { std::cout << "all zero";}
    [1, 0]=> { std::cout << "i=1, j=0";}
    [0, 1]=> { std::cout << "i=0, j=1";}
    [__, __]=> { std::cout << i << ',' << j;}
};

// output
// 0,42
```

ポリモーフィックに型判定も出来るらしい  
というのも、現時点ではまだ実装されていないみたいで動作しなかったです

```c++
// polymorphic type
struct Fruits { virtual ~Fruits() = default; };
struct Orange : Fruits { int ore; };
struct Apple : Fruits { int ap; };
auto o = new Orange();
inspect (o)
{
//    2023/3/3 時はコンパイルエラー
    <Orange> => { std::cout <<  "Orange"; }
    <Apple>  => { std::cout <<  "Apple"; }
};
```

これが出来れば、あとはテンプレートのオペレーター演算子も比較的少ない行数で実装できるという…

---

まだまだ未来の機能ですが、差分学習しておかないと完全に置いて行かれますね…  
C++の言語機能は複雑すぎます

とはいえ業務コードでも、競プロなどでも  
少ない行数で書けるのはとても良いと思うので（＝バグの入る余地が減る）  
どんどん進化していってもらいたいですね！

＜参考＞

- **Pattern Matching Document #: P1371R2 Date: 2020-01-13** - Open Standards  
[https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2020/p1371r2.pdf](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2020/p1371r2.pdf)

- **Pattern Matching for C++** - Welcome to Bjarne Stroustrup's homepage!  
[https://www.stroustrup.com/pattern-matching-November-2014.pdf](https://www.stroustrup.com/pattern-matching-November-2014.pdf)

- **C++ の歩き方 | cppmap
C++23 以降に向けた提案** - C++ の歩き方 | cppmap  
[https://cppmap.github.io/standardization/cppx/](https://cppmap.github.io/standardization/cppx/)
