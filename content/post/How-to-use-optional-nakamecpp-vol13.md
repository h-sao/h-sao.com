+++
date = "2019-03-27T23:00:00+09:00"
draft = false
title = "[C++] std::optionalの使い方を紹介しました"
tags = ["cpp"]
+++

少し前ですが、```std::optional``` について nakameguro_feature.cpp vol.13 で発表したので、その資料を置いときます

- **nakameguro_feature.cpp vol.13**  
[https://ebisu-effective-modern-cpp.connpass.com/event/111469/](https://ebisu-effective-modern-cpp.connpass.com/event/111469/)  

<script async class="speakerdeck-embed" data-id="35d4887f4e6344428ae9e24b268ae643" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

私的感想としては、```optional``` を使うと  
**値取得に失敗しても、例外を書くことなく ```if``` で処理出来る** のが、なんだか良いなぁと思った次第でした

あと、```optional``` の無効状態をどう表すか？   
```reset() / std::nullopt / {}``` のどれよ？  
という話は、参加者の人たちの意見も GitHub の検索結果と同じで  
```reset()``` に軍配があがっていました('ω')

こういう意見交換の場は、本当に恵まれてるなぁ～しみじみ

次回は 3 / 28 (木)開催で、vol.18 です！

- **nakameguro_feature.cpp vol.18**  
[https://ebisu-effective-modern-cpp.connpass.com/event/125272/](https://ebisu-effective-modern-cpp.connpass.com/event/125272/)  

お時間ある方は、ぜひお立ち寄りください～！