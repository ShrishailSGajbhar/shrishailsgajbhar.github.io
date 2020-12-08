---
layout: post
title: "[Programming] Test Driven Developement (TDD) using Python and pytest"
tags: ["programming","python","TDD","Unit Testing","pytest"]
comments: true
---
# Test Driven Developement using Python and pytest

> Test driven developement can help us to minimize different kinds of errors such errors of implementation, errors of interpretation etc. while we develop any application. 

Previously in [post1](https://shrishailsgajbhar.github.io/post/Programming-TTD-Using-Cpp-Google-Test) and [post2](https://shrishailsgajbhar.github.io/post/Programming-TTD-Using-Cpp-Catch2), I have explained the use of test driven developement (TDD) using C++ and its two popular unit testing frameworks namely **googletest** and **Catch2**, respectively.

In this blogpost,  I will explain the use of TDD using python and its unit testing framework called as [pytest](https://docs.pytest.org/en/latest/contents.html). You can find the list of other python unit testing frameworks [here](https://www.softwaretestinghelp.com/python-testing-frameworks/).

Before, moving forward a brief about my developement environment.

- OS: Ubuntu 20.04

- Text Editor: VS Code

- Python version: 3.8.3

## How to install pytest ?

To install pytest through terminal type

```bash
pip install -U pytest
```

You can check the pytest versions as 

```bash
pytest -V
```

In my case, I have python and pytest versions as:

![](/assets/images/20201105/pic1.png)

Now, we are ready with our TDD tools for python.

## What are the different steps used in TDD?

- Write the failing unit test (Red phase)

- Write just enough code to make the failing test pass (Green phase)

- Refactor the code if possible

##### Let us understand above 3 steps using an example.

##### Problem Statement:

> Write a python program that returns a list of first N numbers of the Fibonacci series. e.g., for N=4, the answer should be [0,1,1,2].

Let us solve the above problem using TDD approach by developing a python function **fibonacci(N)**.

##### Solution:

Before writing our first unit test, there are few rules we need to follow if default configuration of pytest is used. The rules are as follows:

- Create a test file having name starting with `test_`

- Define unit test functions that start with `test_` inside the test file

- Use the following command in the folder where test file resides to run your tests
  
  ```bash
   pytest test_<file_name>.py
  ```

The project directory structure is as follows before starting the developement.

![](/assets/images/20201105/pic2.png)

There are two files in the project folder **tdd_python**. The function we are going to develop will be written in **fib.py** while the unit tests used to test the code will be written in **test_fib.py**.

The `fib.py` and `test_fib.py` are modified as follows:

**fib.py**

```python
def fibonacci(N):
    pass
```

**test_fib.py**

To verify test expectations, we will make use of the `assert` statement. The assert statement in pytest are functions which test some condition.

```python
import pytest
from fib import fibonacci

# First unit test
def test_fibonacci_0():
    assert(fibonacci(0) == [])
```

In `fib.py` we have written just enough code to call the function `fibonacci(N)`whereas in `test_fib.py` we have written our first failing unit test (red phase).

The output of `pytest test_fib.py` is as follows:

![](/assets/images/20201105/pic3.png)

To pass this failing unit test to go into the **green phase** we modify the function as follows:

```python
def fibonacci(N):
    if (N == 0):
        return []
```

which makes the test pass with following output.

![](/assets/images/20201105/pic4.png)

Since we have just started to develope the code, there is no need for the refactoring.

Thus we have successfully completed our first TDD cycle (red phase -> green phase -> refactor).

Now let us write our second failing unit test as follows:

```python
def test_fibonacci_1():
    assert(fibonacci(1) == [0])
```

The output now looks as:

![](/assets/images/20201105/pic5.png)

i.e, our last unit test fails since we have not yet modified our `fibonacci` function.

We pass the above failing unit test by modifying the `fib.py` as follows:

```python
def fibonacci(N):
    if (N == 0):
        return []
    if (N == 1):
        return [0]
```

Let us write our next unit test. In this unit test, we will write the edge case where we will pass $N$ as a negative number for which the output should be an empty list. This failing unit test is written as:

```python
def test_fibonacci_Neg():
    assert(fibonacci(-4) == [])
```

To make the above failing unit test pass we write modify the `fib.py` as:

```python
def fibonacci(N):
    if (N == 0):
        return []
    if (N == 1):
        return [0]
    if (N < 0):
        return []
```

Here, we can do the **refactoring** step as follows by modifying the above code as

```python
def fibonacci(N):
    if (N <= 0):
        return []
    if (N == 1):
        return [0]
```

After refactoring, we run the `pytest test_fib.py` to check whether code is broken or not. If all unit tests are passed then the refactoring step is successful.

Next, we write our next unit test as

```python
def test_fibonacci_2():
    assert(fibonacci(2) == [0, 1])
```

which is going to fail with following output:

![](/assets/images/20201105/pic6.png)

which can be passed with following code modification:

```python
def fibonacci(N):
    if (N <= 0):
        return []
    if (N == 1):
        return [0]
    result = [0, 1]
    if (N == 2):
        return result
```

Refactoring is not done as it is not needed.

Now, we can write our next failing test as:

```python
def test_fibonacci_3():
    assert(fibonacci(3) == [0, 1, 1])
```

which can be passed with following code modification to `fib.py`

```python
def fibonacci(N):
    if (N <= 0):
        return []
    if (N == 1):
        return [0]
    result = [0, 1]
    if (N == 2):
        return result
    if (N == 3):
        nextNum = result[1]+result[0]
        result.append(nextNum)
        return result
```

The `pytest test_fib.py`output in this case is as follows:

![](/assets/images/20201105/pic7.png)

Now, we write our next failing unit test as follows:

```python
def test_fibonacci_5():
    assert(fibonacci(5) == [0, 1, 1, 2, 3])
```

Now, to pass this failing test we need to generalize the function since output for $N=4$ is needed here. Also we are in a position to generalize the function for arbitrary value of $N$ which looks as follows:

```python
def fibonacci(N):
    if (N <= 0):
        return []
    if (N == 1):
        return [0]
    result = [0, 1]
    if (N == 2):
        return result
    for i in range(2, N):
        nextNum = result[i-2] + result[i-1]
        result.append(nextNum)
    return result
```

Let us check `fibonacci(N)`function written above with one more test 

```python
def test_fibonacci_7():
    assert (fibonacci(7) == [0, 1, 1, 2, 3, 5, 8])
```

The above test passes successfully as seen in the following output:

![](/assets/images/20201105/pic8.png)

We may refactor the above code here by eliminating the `nextNum` variable. So our final **fib.py** and **test_fib.py** functions are as follows:

```python
# fib.py

def fibonacci(N):
    if (N <= 0):
        return []
    if (N == 1):
        return [0]
    result = [0, 1]
    if (N == 2):
        return result
    for i in range(2, N):
        result.append(result[i-2] + result[i-1])
    return result
```

```python
# test_fib.py

import pytest
from fib import fibonacci
# unit tests
def test_fibonacci_0():
    assert(fibonacci(0) == [])

def test_fibonacci_1():
    assert(fibonacci(1) == [0])

def test_fibonacci_Neg():
    assert(fibonacci(-4) == [])

def test_fibonacci_2():
    assert(fibonacci(2) == [0, 1])

def test_fibonacci_3():
    assert(fibonacci(3) == [0, 1, 1])

def test_fibonacci_5():
    assert (fibonacci(5) == [0, 1, 1, 2, 3])

def test_fibonacci_7():
    assert (fibonacci(7) == [0, 1, 1, 2, 3, 5, 8])
```

Happy coding with TDD..!!
