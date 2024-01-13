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

  
## Function and Files
1. If I have a function called "foo" in a.cpp, and want to use it in b.cpp(assume two files are in the same directory), all I need to do is to add forward declaration of foo in b.cpp. For **better practice**, create a header file and include it instead.
- reason: write a header file can gather all the function header, so you would not need to add multiple declaration in b.cpp
2. Only declaration and definition are allowed in global namesapce. However, usage of **global variable** is strongly discouraged.
3. Avoid "using namespace std;" at all cost.
- reason: it violated the intention of namespace, and can cause bug like this (copied from the site):
```
#include <iostream> // imports the declaration of std::cout

using namespace std; // makes std::cout accessible as "cout"

int cout() // defines our own "cout" function in the global namespace
{
    return 5;
}

int main()
{
    cout << "Hello, world!"; // Compile error!  Which cout do we want here?  The one in the std namespace or the one we defined above?

    return 0;
}
```
