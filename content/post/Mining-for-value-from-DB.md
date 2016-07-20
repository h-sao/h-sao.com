+++
date = "2014-04-10T18:00:15+09:00"
draft = false
title = "[Oracle]テーブル・カラム名が不明でもDBに入っている値を検索したい"
tags = ["Oracle", "SQL"]
+++

見知らぬデータベースを渡され、中を解析したい時…

ありますよね？ありますよね！？

全テーブルの全列に入っている値を横断検索したいという要求、あるんじゃないでしょうか？

探したら同じことを考えている人が居ました

- 全テーブルの全列の値の検索方法について - Oracle Community  
[https://community.oracle.com/thread/2334738?tstart=0](https://community.oracle.com/thread/2334738?tstart=0)
 
方法は2種類  
一つは **dmp 出力して、テキスト検索** をするというもの  
だいぶ力技ですけど、確かに、確実に検索できますね…

もう一つの方法は、**PL/SQL で全データを総検索** するというもの

この方法、  
やってみると、良い感じに動きます  
お勧めです  
テーブル名やカラム名が判らなくても、動きますしね！

めっちゃ感激したので、この記事にも SQL を載せておきます

このサンプルでは、全ての値に 「あ」 が入っているデータを検索します  
どのテーブルの、どのカラムに、何件入っているかが判ります

う、嬉しい～!!!

```sql
declare
 sSQL VARCHAR2(128);
 sDATA VARCHAR2(500);
 type cursor_type is ref cursor;
 cur_search cursor_type;
 vCount INTEGER;
 ERR_CODE NUMBER := 0;
 ERR_MSG VARCHAR(255);
begin
 for vRec in (select COLUMN_NAME,TABLE_NAME from USER_TAB_COLS where DATA_TYPE like '%CHAR%')
 loop
  begin
    open cur_search for 'select count(*) as cnt from ' || vRec.TABLE_NAME || ' where ' || vRec.COLUMN_NAME || ' like ''%あ%''';
        fetch cur_search into vCount;
        if vCount > 0 then
          dbms_output.put_line(vRec.TABLE_NAME || '.' || vRec.COLUMN_NAME || ':' || vCount || '件あり');
        end if;
        close cur_search;
  exception
   WHEN OTHERS THEN
    ERR_CODE := SQLCODE;
    ERR_MSG  := SUBSTRB(SQLERRM,1,255);
    dbms_output.put_line('error:' || ERR_CODE || ' ' || ERR_MSG || ' ' || vRec.TABLE_NAME || '.' || vRec.COLUMN_NAME);
  end;
 end loop;
end;
```

DBの構造を解析するツールは結構探せばあるのですが、データの中身となると、調べるのは手間ですよね…  
このSQLには助けられました～


話は変わりますが、  
DB系のクライアントツールで、わたしがよく使っているのは  **A5:SQL Mk-2** （フリーソフト）です
 
- A5:SQL Mk-2 : SQL Development Studio  
[http://a5m2.mmatsubara.com/](http://a5m2.mmatsubara.com/)
 
Oracle, SQLServer, MySQL など多くのDBに対応しています  
一番うれしいのは、Oracle への接続に、Oracle Client が無くても接続できる所ですね

> OCI経由か、直接 IPアドレスなどを指定して接続するかを選択できる！！
 
ER図も自動生成してくれるし、  
めっちゃ、現場で役立ってます