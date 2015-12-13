+++
date = "2014-01-31"
draft = false
title = "C,C++,C++/CLI sample program"
tags = ["Cpp"]
+++

Hi,

## C program

```cpp
c00.c
#include <stdio.h>

int main()
{
    printf("This is a native C program.\n");
    return 0;
} 
```

compile

```
> cl.exe c00.c
```

## C++ program

```cpp
c01.cpp
#include <iostream>

int main()
{
    std::cout << "This is a native C++ program." << std::endl;
    return 0;
}
```

compile

```
> cl.exe /EHsc c01.cpp
```

## C++/CLI program

```cpp
c02.cpp
int main()
{
    System::Console::WriteLine("This is a Visual C++ program.");
}
```

compile

```
> cl.exe /clr c02.cpp
```

That is very interesting!
