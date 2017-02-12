+++
date = "2014-07-17T21:57:00+09:00"
draft = false
title = "[PowerShell] ファイル分割スクリプト"
tags = ["PowerShell"]
+++

大きなファイルを分割する必要があって  
めっちゃ困りましたけど、また PowerShell で解決しました  
なかばやけくそ

＜sep.ps1＞

```c
$my_path_name = "c:/tmp/"
$my_file_name = "abc"
$my_file_kind = ".log"
$cut_num      = 2000    # 切り取る行数

$my_file = $my_path_name + $my_file_name + $my_file_kind

$count = 0;
Get-Content $my_file -ReadCount $cut_num -Encoding UTF8 | 
    ForEach-Object { 
        $count ++
        $cfs = "{0:D3}" -f $count;
        $_ > ($my_path_name+$my_file_name+'_'+$cfs+$my_file_kind)
    }
```

結果

![](/pic/Divide-File-Using-PowerShell_00.png)

あまり何も考えず、元ネタはこの方のブログですー  

- 参考サイト：powershellが超便利 - fivegangstersのブログ  
[http://blog.livedoor.jp/fivegangsters/tag/powershell](http://blog.livedoor.jp/fivegangsters/tag/powershell)