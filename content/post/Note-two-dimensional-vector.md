+++
date = "2022-03-20T02:00:00+09:00"
draft = false
title = "[C++] 2次元Vectorのメモ"
tags = ["cpp"]
+++

HackerRankやってるときによく２次元配列が出てくるので、ちゃちゃっとやりたいときのメモです

```cpp
// ２次元配列の宣言,,,出来れば v じゃなくて用途名にしたいところ…
std::vector<vector<int>> v(2, vector<int>(3));
// こんな配列が出来てる↓↓↓
v.at(0).at(0);
v.at(0).at(1);
v.at(0).at(2);
v.at(1).at(0);
v.at(1).at(1);
v.at(1).at(2);

// 呼び先でも内容を更新したいときは参照が楽…
void func(vector<int> &t)
void func(vector<vector<int>> &v)

// べた書きループはかなり楽
for(unsigned int i = 0; i < v.size(); ++i){
    for(unsigned int j = 0; j < v.at(i).size(); ++j){
        // 何か処理
    }
}
// べた書きループ、
// サイズチェックが気になる or 他でも利用するなるなら…
unsigned int row = v.size();
for(unsigned int i = 0; i < row; ++i){
    auto &box = v.at(i);
    unsigned int col = box.size();
    for(unsigned int j = 0; j < col; ++j){
        auto &ck_data = v.at(i).at(j);
        // 何か処理
    }
}
```

業務や後に残るコードではなく、とにかくちゃちゃっとやりたい、という要望って  
自分はこれまで出会ったことが無かったので  
この手のコーディングは、自分的にはかなり新しいというか新鮮な感じがします