+++
date = "2008-07-29T01:17:00+09:00"
draft = false
title = "[C/C++] 割り算を使わないで割り算する方法"
tags = ["cpp"]
+++

ビット・シフトの計算の練習問題＜上級編＞です

**Q. 割り算をシフトと引き算のみで表現しなさい**

**A. 回答**

```cpp
// x / y = ans
// x: 分子
// y: 分母
// ans:答え

ans = 0;
 while( x >= y ){
    dammy = y;
    syou = 1;
    while( x >= dammy ){            // 割られる数を超えるまで割る数をシフト
        dammy = dammy << 1;
        syou  = syou  << 1;
    }
    
    dammy = dammy >> 1;             // 超える手前まで戻す
    syou = syou >> 1;
    
    x = x - dammy;                  // 筆算
    ans = ans + syou;               // 答え
 }
 printf( "%d ・・・%d\n", ans, x );
```

新人研修などにどうでしょう？

※ 実は最後に足し算使ってますね…(>_<)