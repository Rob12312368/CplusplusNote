# CplusplusNote

## Description
This repo aims to keep record of what I do not know before learning C++ from [learncpp.com](https://learncpp.com).

## Basics
1. list initialization is preferred over copy and direct initialization.
- multiple reasons: prevention of narrow conversion(int a{5.67}), consistent syntax, etc.
```
int a{5}; // list init
int a{}; // init a to 0
int a = 5; // copy init
int a(5); // direct init
```
2. '\n' is preferred over endl
- reason: endl moves the cursor to the next line of the console, and it flushes the buffer, which is **less efficient** than letting the system to flulsh itself periodically.
```
cout << "hello world" << '\n'; // note the single quote around \n
```
3. cin is smart enough to get the valid part of an input.
- reason: assuming we want to get an integer, we will get 123 from the input of "123abc"

  
