+++
date = "2014-02-24"
draft = false
title = "Anonymous Struct of C"
tags = ["Cpp"]
+++

Hi,

It is very tedious to think about the name of a struct when you use to struct only once.  
In such a case, Anonymous structure comes in handy.

It is assigned to the anonymous struct

```cpp
struct{
        char    1st_name[0x80];
        char    2nd_name[0x80];
 
    } name_tbl[] = {
        { "Isami",   "Kondou" },
        { "Toshizo", "Hijikata" },
        { "Kogorou", "Kathura" },
    };
 
    for( i = 0; i < sizeof( name_tbl ); i ++ ){
        printf( "%s\n", name_tbl[ i ].1st_name );
        printf( "%s\n", name_tbl[ i ].2nd_name );
    } 
```

This code is very straightforward!  

This method is effective only when there is no necessity that you use 2 times and this structure. 
