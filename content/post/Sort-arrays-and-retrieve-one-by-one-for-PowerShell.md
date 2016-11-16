+++
date = "2014-07-02T17:46:00+09:00"
draft = false
title = "[PowerShell] 連想配列をソートしてforeachで一個づつ取り出す"
tags = ["PowerShell"]
+++

PowerShell で連想配列を用いた時のソート＆foreach のサンプルがなかなか探せなかったのでメモします

とある連想配列

```
PS C:\> $tbl = @{
>>       k00 = "orange";
>>       k01 = "peach";
>>       k02 = "apple"
>>  }
```

出力は簡単

```
PS C:\> $tbl

Name                           Value
----                           -----
k01                            peach
k02                            apple
k00                            orange
```

ソートの出力も簡単

```
PS C:\> $tbl.GetEnumerator() | Sort-Object Value

Name                           Value
----                           -----
k02                            apple
k00                            orange
k01                            peach

# もしくは

PS C:\> $tbl.GetEnumerator() | sort Value

Name                           Value
----                           -----
k02                            apple
k00                            orange
k01                            peach
```

これを一つづつ取り出して処理したい

## 単に foreach で取り出すとき

```
PS C:\> foreach( $k in $tbl.Keys ){
>>   Write-Output $k
>>   Write-Output $tbl[ $k ]
>>   Write-Output '-----'
>> }

k01
peach
-----
k02
apple
-----
k00
orange
-----
```

## さらにソートして foreach で取り出すとき

```
PS C:\> $tbl.GetEnumerator() | sort Value |
>>   ForEach-Object{
>>       Write-Output $_.Name
>>       Write-Output $_.Value
>>       Write-Output '-----'
>>   }

k02
apple
-----
k00
orange
-----
k01
peach
-----
```

**ForEach-Object** を使いました
ただの foreach() でも出来ると思うんですけど、私的用途が満たせたので、これ以上は調べてないです

検索で、「powershell foreach ソート -perl」 などで検索しても、日本語の情報があまり出てこなかったので、参考になれば幸いです(^^)