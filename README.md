# CplusplusNote (Last Update: 1/17)

## Description
This repo aims to keep record of what I do not know before learning C++ from [learncpp.com](https://learncpp.com).

## C++ Basics
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

  
## C++ Basics: Function and Files
1. If I have a function called "foo" in a.cpp, and want to use it in b.cpp(assume two files are in the same directory), all I need to do is to add forward declaration of foo in b.cpp. For **better practice**, create a header file and include it instead.
- reason: write a header file can gather all the function header, so you would not need to add multiple declaration in b.cpp
2. Source file should include its header file.
- reason: so that if there is a mismatch of function, it can be found during comipile time instead of linking time.
3. <> is for header files not written by the programmer, and "" is to tell the coompiler to search for the file the prgrammer wrote in the current directory.
4. A file should include all the needed files instead of relying on transitive includes
5. Header guard should be used to avoid duplicate definitions, and the syntax is like the following:
```
#ifndef SQUARE_H
#define SQUARE_H

int getSquareSides(); // forward declaration for getSquareSides
int getSquarePerimeter(int sideLength); // forward declaration for getSquarePerimeter

#endif
```
6. Only declaration and definition are allowed in global namesapce. However, usage of **global variable** is strongly discouraged.
7. Avoid "using namespace std;" at all cost.
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
## Debugging C++ Programs
1. Using preprocessor can be a choice of debugging.
```
#include <iostream>

#define ENABLE_DEBUG // comment out to disable debugging

int getUserInput()
{
#ifdef ENABLE_DEBUG
std::cerr << "getUserInput() called\n";
#endif
	std::cout << "Enter a number: ";
	int x{};
	std::cin >> x;
	return x;
}

int main()
{
#ifdef ENABLE_DEBUG
std::cerr << "main() called\n";
#endif
    int x{ getUserInput() };
    std::cout << "You entered: " << x << '\n';

    return 0;
}
```
2. Writing log is also pretty common. Plog needs to be installed. Spdlog is another logging choice, and it is used on larger or performance-sensitive projects.
```
#include <plog/Log.h> // Step 1: include the logger headers
#include <plog/Initializers/RollingFileInitializer.h>
#include <iostream>

int getUserInput()
{
	PLOGD << "getUserInput() called"; // PLOGD is defined by the plog library

	std::cout << "Enter a number: ";
	int x{};
	std::cin >> x;
	return x;
}

int main()
{
  //plog::init(plog::none , "Logfile.txt"); // plog::none eliminates writing of most messages, essentially turning logging off
	plog::init(plog::debug, "Logfile.txt"); // Step 2: initialize the logger

	PLOGD << "main() called"; // Step 3: Output to the log as if you were writing to the console

	int x{ getUserInput() };
	std::cout << "You entered: " << x << '\n';

	return 0;
}
```
## Fundamental Data Types
1. C++ does specify the minimum size for datatypes, but not for the maximum size. Therefore, int can be 2 bytes or 4 bytes on different machines. It is best to refer to the C++ standard and assume the most limited range for a datatype if the goal is to write a cross-platform program.
2. For better practice, initialize datatypes like the following:
```
int x{5};      // 5 means integer
double y{5.0}; // 5.0 is a floating point literal (no suffix means double type by default)
float z{5.0f}; // 5.0 is a floating point literal, f suffix means float type
```
3. cout only outputs 6 significant digits, so we can use `setprecision` to make the output more precise. Note that `setprecision` is sticky, which means it only has to be set once unless we want to change it.
```
#include <iomanip> // for output manipulator std::setprecision()
#include <iostream>

int main()
{
    std::cout << std::setprecision(17); // show 17 digits of precision
    std::cout << 3.33333333333333333333333333333333333333f <<'\n'; // f suffix means float
    std::cout << 3.33333333333333333333333333333333333333 << '\n'; // no suffix means double

    return 0;
}
```
4. Rounding error happens all the time! The way decimal number is stored in the computer is just an approximation, which means we should never assume it is 100% accurate.
```
#include <iomanip> // for std::setprecision()
#include <iostream>

int main()
{
    double d{0.1};
    std::cout << d << '\n'; // use default cout precision of 6
    std::cout << std::setprecision(17);
    std::cout << d << '\n';

    return 0;
}
```
