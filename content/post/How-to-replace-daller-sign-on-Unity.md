+++
date = "2022-12-09T16:00:00+09:00"
draft = false
title = "[C#] $文字を置換したい"
tags = ["csharp", "unity"]
+++

ちょっとnewbie的ですが、`$` 文字をひっかけようとしてうまくいかなかったので、その時のメモです

今回の開発環境はこちら  

- Visual Studio 2019
- Windows10
- Unity 2021.3.0f1

---

Unityで `string` 文字列の中の `$` 文字を置換使用を思いましたが、うまく出来ませんでした

```csharp
text = text.Replace("\\$", "");
```

はて。。。なんでかな？

調べてみて、こちらなら思った通りの動作になりました

```csharp
text = Regex.Replace(text, "\\$", "");
```

正規表現だとうまく行くみたい

参考：

- Simple way to trim Dollar Sign if present in C# - stack overflow  
[https://stackoverflow.com/questions/154845/simple-way-to-trim-dollar-sign-if-present-in-c-sharp](https://stackoverflow.com/questions/154845/simple-way-to-trim-dollar-sign-if-present-in-c-sharp)
