+++
date = "2020-05-08T15:00:00+09:00"
draft = false
title = "[c++] 2次元配列的な書き方"
tags = ["cpp"]
+++

自分メモ

## 2次元配列

`C++11` 以前では、2次元配列はこんな感じで書いていました

```cpp
// 2-dimensional array (raw type)
constexpr static float xy_raw[][2] = {
      { 0.f,  10.f}  // 0
    , { 20.f, 30.f}  // 1
    , { 40.f, 50.f}  // 2
};
```

これでも悪くは無いのですが、生配列を扱うと範囲外アクセスに気が付かない可能性が出てきます
そこで `C++11` から境界チェックが出来る配列の `std::array` が登場しました  
これを使えば、C-styleの配列のノリで扱うことが可能です

- std::array - cpprefjp  
[https://cpprefjp.github.io/reference/array/array.html](https://cpprefjp.github.io/reference/array/array.html)

さらに2次元配列にしたければ `std::array` を `std::vector` に入れることで実現可能です

```cpp
// 2-dimensional array (using stl)
const std::vector<std::array<float, 2>> xy_array = {
      { 0.f,  10.f}  // 0
    , { 20.f, 30.f}  // 1
    , { 40.f, 50.f}  // 2
};
```

悪くなさそう

## 動作確認


```cpp
#include <iostream>
#include <vector>
#include <array>

int main() {
  // ---------------------------------
  // 2-dimensional array (raw type)
  constexpr static float xy_raw[][2] = {
        { 0.f,  10.f}  // 0
      , { 20.f, 30.f}  // 1
      , { 40.f, 50.f}  // 2
  };

  std::cout << "xy_raw[0][1]: " << xy_raw[0][1] <<  "\n"; // 10

  // xy_raw[1][2] は out of rangeだけど warningになってる
  // これはたまたま xy_raw[2][0] と同じなので動作しているだけ
  std::cout << "xy_raw[1][2]: " << xy_raw[1][2] <<  "\n"; // 40

  // ---------------------------------
  // 2-dimensional array (using stl)
  const std::vector<std::array<float, 2>> xy_array = {
        { 0.f,  10.f}  // 0
      , { 20.f, 30.f}  // 1
      , { 40.f, 50.f}  // 2
  };

  std::cout << "xy_array[2][0]: " << xy_array.at(2).at(0) <<  "\n";  //40  

  // compile error
  //std::cout << "xy_array[1][2]: " << xy_array.at(1).at(2) <<  "\n";  // out of range accsess
}
```

### 結果

> xy_raw[0][1]: 10  
xy_raw[1][2]: 40  
xy_array[2][0]: 40

コンパイル時に教えてくれるので助かります


### 参考

- コンパイル結果：  
[https://repl.it/repls/StudiousImaginativeParallelcomputing](https://repl.it/repls/StudiousImaginativeParallelcomputing)


- C++ - Vector of Float Arrays - stackoverflow
https://stackoverflow.com/questions/33711878/c-vector-of-float-arrays