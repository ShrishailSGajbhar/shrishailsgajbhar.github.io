---
layout: post
title: "[Programming] Test Driven Developement (TDD) using C++ and Catch2"
tags: ["programming","C++","TDD","Unit Testing","Catch2"]
comments: true
---

> “You can’t achieve the goal of unit testing by just throwing more tests at the project.You need to consider both the test’s value and its upkeep cost.”<br>
> — Vladimir Khorikov, [Unit Testing: Principles, Practices, and Patterns](https://www.manning.com/books/unit-testing)

In my first [blogpost](https://shrishailsgajbhar.github.io/post/Programming-TTD-Using-Cpp-Google-Test), I explained the process of test driven developement (TDD) by developing the *classic* fizzbuzz function. The unit tests used in this case were  written using [googletest](https://github.com/google/googletest) framework. In this post, I will explain the use of another popular unit testing framework called as [Catch2](https://github.com/catchorg/Catch2).  Although except few changes, the steps are similar to this [blogpost](https://shrishailsgajbhar.github.io/post/Programming-TTD-Using-Cpp-Google-Test), for the sake of completion I will repeat the same steps in this post also.

### Salient features of Catch2:

* This testing framework can be used in two ways:
  
  * as a single header include file
  
  * as a statically compiled library with systemwide installation (Recommended)

* Supports Behaviour Driven Developement (BDD) style

* No external dependancies required if you have C++11 compliant compiler.

* For more details check features section of this [page](https://github.com/catchorg/Catch2/blob/devel/docs/why-catch.md#top).

### What is a Unit Test?

Verbatim from [1]

> unit test is an automated test that:
> 
> * Verifies a small piece of code (also known as a unit)
> 
> * Does it quickly,
> 
> * And does it in an isolated manner

According to the author of [1] Valdimir Khorikov, the unit tests should be designed carefully as they can be good as well bad for the project. The unit tests should be such that they should avoid the *regression* (not a ML term here!). 

### What is regression?

Verbatim from [1]

> A regression is when a feature stops working as intended after a certain event (usually, a code modification). The terms regression and software bug are synonyms and can be used interchangeably.

So lets write some unit tests using Catch2 even though they are bad, since it is better to write bad unit tests rather than not writing them at all !

Before, moving forward a brief about my developement environment.

- OS: Ubuntu 20.04

- C++ Compiler: Clang 10.0.1

- Text Editor: VS Code

- Build System Generator: CMake 3.19.0

# How to install *Catch2* on Ubuntu globally.

I prefer and will make use of the statically compiled version of Catch2 since the compile times are much faster and provides more flexibility,

You may install it as:

```bash
$ git clone https://github.com/catchorg/Catch2.git
$ cd Catch2
$ cmake -Bbuild -H. -DBUILD_TESTING=OFF
$ sudo cmake --build build/ --target install
```

**Note: The above steps will install Catch2 as statically compiled library which will be available system wide (globally). But if you just want to play around or test this framework then the option of single header file inclusion is sufficient and is also available.**

Three different steps of the TDD approach are: 

- Writing the failing unit test (**Red phase**)

- Writing the just enough code to make the failing unit test pass (**Green phase**)

- Refactoring the code if possible

##### Let us understand above 3 steps using an example.

##### Problem Statement:

> Write a program that prints all the numbers from 1 to 100. For multiples
>  of 3, instead of the number, print "Fizz", for multiples of 5 print 
> "Buzz". For numbers which are multiples of both 3 and 5, print 
> "FizzBuzz".

Let us solve the above problem using TDD approach by developing a library function **fizzBuzz** which will be used to achieve the final result.

##### Solution:

The project directory structure is as follows before starting the developement.

![](/assets/images/20201028/pic1.png)

The project directory name is **catch2_demo** which contains root **CMakeLists.txt**. The **include** folder contains the header file for our fizzBuzz function. The **src** folder contains **fizzbuzz.cpp** having our function to be tested and being developed. There is a **tests** folder which contains its own **CMakeLists.txt** and **test_tdd_fizzbuzz.cpp** where we will put the unit tests which will provide us feedback to implement the fizzBuzz function.

Here is the sample to do list for writing the test cases while developing the function (Taken from [2], an excellent course to learn TDD based C++ developement).

- [ ] Can I call the fizzBuzz function?

- [ ] Get "1" when I pass 1

- [ ] Get "2" when I pass 2

- [ ] Get "Fizz" when I pass 3

- [ ] Get "Buzz" when I pass 5

- [ ] Get "Fizz" when I pass 6 (multiple of 3)

- [ ] Get "Buzz" when I pass 10 (multiple of 5)

- [ ] Get "FizzBuzz" when I pass 15 (a mupltiple of 3 and 5)

Before diving into the explanation, take a look at the CMakeLists.txt files in the root i.e., **tdd_fizzbuzz** and **tests** directories. Also, take a look at fizzbuzz.h and fizzbuzz.cpp files.

**CMakeLists.txt in root directory:**

```cmake
cmake_minimum_required(VERSION 3.15)
project(catch2_demo)
# Generate the library libfizzbuzz
add_library(libfizzbuzz src/fizzbuzz.cpp)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include/)
# For Catch2 Testing
add_subdirectory(tests)
```

**CMakeLists.txt in tests directory:**

```cmake
find_package(Catch2 REQUIRED)
include(CTest)
include(Catch)
set(TEST_BINARY ${PROJECT_NAME}_test)
add_executable(${TEST_BINARY}
               test_tdd_fizzbuzz.cpp)
target_link_libraries(${TEST_BINARY} libfizzbuzz Catch2::Catch2)
catch_discover_tests(${TEST_BINARY})
```

**fizzbuzz.h**

```cpp
#pragma once
#include <string>
std::string fizzBuzz(int n);
```

**fizzbuzz.cpp**

```cpp
#include "fizzbuzz.h"
// function fizzBuzz to be implemented
```

Now, let us write a failing test case.

```cpp
#include <catch2/catch_test_macros.hpp>
#include <catch2/catch_session.hpp>
#include <string>
#include "fizzbuzz.h"

int main( int argc, char* argv[] ) {
  // global setup...

  int result = Catch::Session().run( argc, argv );

  // global clean-up...

  return result;
}

TEST_CASE("Can call FizzBuzz function") {
  REQUIRE(fizzBuzz(1) == "1");
}
```

Now the above test case is going to fail because the function fizzBuzz is not yet implemented and will have a compilation error. Note that compilation error is also an indicator of a failing test case.

So, we have successfully written a failing test case for "Can I call the fizzBuzz function?" which completes our first step (**Red phase**).

In the second step, let us make it pass. For that we will write the **fizzBuzz** function as follows:

```cpp
#include "fizzbuzz.h"
#include <string>

std::string fizzBuzz(int n) {
  return std::to_string(n);
}
```

Now, if we compile again the test binary with name **tdd_fizzbuzz_test** will produce following output. The above binary is in */build/tests/* folder

![](/assets/images/20201028/pic2.png)

**Steps used to compile and run the program:**

```bash
mkdir build && cd build && cmake ..
make
```

After, we complete our second step, the next step is to refactor the code. Since we have just started coding, as of now refactoring is not required at this stage.

With the code as in **fizzBuzz** function following test cases for following assertions will get passed.

- [x] Can I call the fizzbuzz function?

- [x] Get "1" when I pass 1

- [x] Get "2" when I pass 2

#### Next, we should get "Fizz" when we pass 3

Step:1 Write a failing test case as:

```cpp
TEST_CASE("For input as 3 output should be Fizz") {
  REQUIRE(fizzBuzz(3) == "Fizz");
}
```

The test binary output looks like:

![](/assets/images/20201028/pic3.png)

So out of two tests written, one case is passed and other is failed. Let's pass the failing test case by adding following code in our fizzBuzz function.

```cpp
#include "fizzbuzz.h"
#include <string>

std::string fizzBuzz(int n) {
  if (n == 3) {
    return "Fizz";
  }
  return std::to_string(n);
}
```

With above fizzBuzz function, now all two test cases will be passed.

Here, you may take decision about refactoring accordingly.

Similarly to above test case, we can also complete the case "Get "Buzz" when I pass 5".

Thus we will also mark above two tasks of our todo list.

- [x] Get "Fizz" when I pass 3

- [x] Get "Buzz" when I pass 5

Now let us move towards the next two tasks of our checklist

- [ ] Get "Fizz" when I pass 6 (multiple of 3)

- [ ] Get "Buzz" when I pass 10 (multiple of 5)

the unit tests for which written as follows are going to fail.  

```cpp
 TEST_CASE("For input as multiple of 3 output should be Fizz") {
   REQUIRE(fizzBuzz(6) == "Fizz");
 }

 TEST_CASE("For input as multiple of 5 output should be Buzz") {
   REQUIRE(fizzBuzz(10) == "Buzz");
 }
```

We can pass the above tests by modifying the function as:

```cpp
#include "fizzbuzz.h"
#include <string>

std::string fizzBuzz(int n) {
  if ((n%3)==0) {
    return "Fizz";
  } else if ((n % 5) == 0) {
    return "Buzz";
  }
  return std::to_string(n);
}
```

The test binary output now is:

![](/assets/images/20201028/pic4.png)

For the current version of our fizzBuzz function the test case for "Get "FizzBuzz" when I pass 15 (a mupltiple of 3 and 5)" is going to fail.

The test case in this case can be written as:

```cpp
TEST_CASE("For input multiple of both 3 & 5 output should be FizzBuzz") {
   REQUIRE(fizzBuzz(15) == "FizzBuzz");
 }
```

The test binary will be now

![](/assets/images/20201028/pic5.png)

Now with final modification to our fizzBuzz function as

```cpp
#include "fizzbuzz.h"
#include <string>

std::string fizzBuzz(int n) {
  if (n%3==0 && n%5==0) {
    return "FizzBuzz";
  } else if (n % 3 == 0) {
    return "Fizz";
  } else if (n%5==0) {
      return "Buzz";
  } else {
    return std::to_string(n);
  }
}
```

our all test cases pass

![](/assets/images/20201028/pic6.png)

Hurray...! All test cases passed and our fizzBuzz function is now tested and completed. We can now use it in the main program to obtain the solution of problem statement.

To do this, I have

- created main.cpp and put it in the src folder

- modified the root CMakeLists.txt to create the binary for main.cpp

The main.cpp and modified root CMakeLists.txt files are as follows:

**main.cpp**

```cpp
#include <iostream>
#include <string>
#include "fizzbuzz.h"

int main() {
  for (int i = 1; i <= 100; ++i) {
    std::cout<<fizzBuzz(i)<<std::endl;
  }
}
```

**CMakeLists.txt** (root)

```cmake
cmake_minimum_required(VERSION 3.15)
project(catch2_demo)

# Generate the library libfizzbuzz

add_library(libfizzbuzz src/fizzbuzz.cpp)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src/)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include/)

# For Catch2 Testing
add_subdirectory(tests)

add_executable(tdd_fizzbuzz_bin src/main.cpp)
target_link_libraries(tdd_fizzbuzz_bin libfizzbuzz)
```

Following bash commands wil give us the final output.

```bash
# Assuming we are in project root directory i,e., catch2_demo.

# To delete the current  build folder
rm -r build
# Create a new build folder, go to that folder and run root CMakeLists.txt
mkdir build && cd build && cmake .. 
# execute the tdd_fizzbuzz_bin i.e., main executable
./tdd_fizzbuzz_bin
```

![](/assets/images/20201028/pic7.png)

# References

[1] Unit Testing: Principles, Practices, and Patterns by Vladimir Khorikov, Manning Publications (2019).

[2] [Test-Driven Development in C++](https://www.lynda.com/C-tutorials/Test-Driven-Development-C/746312-2.html) by Richard Wells hosted on [Lynda.com](https://www.lynda.com/).
