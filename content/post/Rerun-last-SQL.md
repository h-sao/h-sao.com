+++
date = "2014-01-18T00:53:07+09:00"
draft = false
title = "[Oracle] 直前のSQLを再実行する"
tags = ["SQL"]
+++

OracleのコマンドラインでSQLを実行させるときのメモ

直前のSQLコマンドを実行は / (スラッシュ)  
この場合、直前のバッファの内容は表示しません

```sql
SQL> /
```

直前のバッファの内容を表示して実行するのは run コマンド

```sql
SQL > run
```

直前のバッファの内容を修正するには edit コマンド  
タイプミスなどを修正できます  
ただし、環境変数 "_editor" にエディタの設定をしておく必要があります  

```sql
SQL > define  _editor=vi
SQL > edit
```

-----
（参考）

- OracleのSQL*Plusで直前に実行したSQLを再実行  
[http://margrave.seesaa.net/article/127477048.html](http://margrave.seesaa.net/article/127477048.html)

- SQL バッファの編集、エディタで編集する  
[http://www.shift-the-oracle.com/sqlplus/command/edit.html](http://www.shift-the-oracle.com/sqlplus/command/edit.html)

- 直前に実行したSQLをエディタで編集する   
[http://sakusakuit.blog.fc2.com/?m&no=15](http://sakusakuit.blog.fc2.com/?m&no=15)
