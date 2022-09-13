# Functions

### Just like variables, functions in programming languages are similiar to what you learned in math. The idea is simple: a function can take inputs, do work on them, and then return outputs. However, in reality functions are much more powerful and versatile. They are used by programmers to reduce repeated code and simplify. Let's break it down and show some examples.

### A function consists of two parts, a *declaration* and a *definition*. The declaration includes the functions *return type* (the type of variable it outputs or returns), *name*, and *parameters* (inputs). The definition is the functions logic and defines what the functions should actually do.

Here's an example.

`int add(int a, int b);` - **Declaration**

`{ return a + b; };` - **Definition**

The two can be put together to make a complete function.
```
int add(int a, int b)
{
  return a + b;
};
 ```
 
A declaration can also be made without a definition. This lets your code use the function while telling the compiler to look for the definition, which should be added later.
 
 `int add(int a, int b);` - This appears earlier
 
```
int add(int a, int b)
{
  return a + b;
};
 ``` 
This adds the definition to the function

Functions are used by "calling" them. This is done by putting the name of the function, followed by parenthesis and any arguments necessary to fill the parameters (parameters refer to the variable names in the function's declaration, while variables you use to call the function are called arguments). For example, let's call the function we made earlier.

`add(1,2);`

When calling the `add` function, the computer copies the `int`s `1` and `2` to be `a` and `b` respectively. `a` and `b` are only accessible by the add function. This idea of "scopes" will be covered in the next document. The computer continues on to the definition of the `add` function, where it finds `return a + b;`. It then computes `a + b` and "returns" that value. In this case, we did not capture this output, and it is simply discarded. The following example shows how we can save the return value.

`int result = add(1,2);`

The `int` result contains the return value, or output, from the function (`3`).

In this case, we used `1` and `2` as argument values. Both are temporary variables, also called "r-values". Functions can also be called with "l-values," or real variables. For example,

```
int x = 1;
int y = 2;

int result = add(x,y);
```

This is the most basic example of a function; most functions are not as simple. Some functions may do additional work with the variables.

```
double pattern(double z)
{
  // Any text following "//" is not interpreted by the compiler and is simply used as a note for people reading the code
  z += 3; // This is the same as z = z + 3
  z *= 10; // Equivalent to z = z * 10
  return z;
};

double a = 3.2;

double result = pattern(a);
```

Since `z` is a new variable which contains the copied value of `a`, changing it does not actually change `a`. In this case when the program finishes, `a` will still hold `3.2` and `result` will hold `62`.

Having an input and output is optional. Functions can have "side-effects," where calling the function performs some logic involving the rest of the program. The following function changes the variable `b`. The `void` type simply denotes that no value is returned. These functions do not need a `return` line because they do not return anything.

```
int b = 3;

void increment()
{ 
  b++; // Equivalent to b = b + 1 
};

increment();
increment();
```

The program exits with `b` holding a value of `5`.

Functions can also take and return `const` variables.

```
int const test(int const a, int const b)
{
  // a = 3; would not be allowed as this changes a
  return a + b; // This is fine
}
```

In this case, having `const` parameters lets the compiler know that you will not change `a` and `b` inside your function and allows for some optimization. Returning a `const` variables doesn't really do much in this case, but it is useful sometimes when dealing with memory.

### The main function

There is one special function which exists in all C++ programs. It is called the `main` function, and has an `int` return value. When a C++ program is run, it starts at the beginning of the main function. All other functions are called from this function. It is not actually required to return anything, but you can optionally return an "exit code," a number which denotes what (if any) error the program encountered.

```
int main()
{
  int a = 3;
  int b = 2;

  testfunc();

  int c = add(a,b);

  return 0; // Optional
};
```

### std::cout

Another function which you might commonly use in writing practice code (this is not as useful in robot code) is `std::cout` (console ouput). This function is used to show variables (strings, ints or doubles, booleans, etc.) as text on the program's **console** (think of it as a way of communicating with your program).

## The standard library

### A collection of features written to help save you time while writing code

Many functions and features are included in the `std` or **"standard"** library. This is an implementation of many common features to allow code to be compiled for different platforms (the process for storing text or output it is different on different computers and operating systems).

The `std` library is very large, however, and as we starting out, we probably need the features involving **input** and **output** to the console, so we can just include the `iostream` file. 

`#include <iostream>`

We'll learn more about how this works later, but all you need to know is that this let's you use many of the common `std::` functions and variables, including `std::cout` (console output), `std::cin` (console input), and `std::string` (the implementation of the `string` variable I mentioned in the Variables lesson).

To actually use `std::cout`, you must use 2 less-than signs (<<), followed by something to output.

`std::cout << "Test";`

```
int a = 3
std::cout << a; // Outputs 3
```

```
bool true_or_false = true;
std::cout << true_or_false; // Outputs 1 because true is represented as 1
```

For more on functions, check out [this video](https://www.youtube.com/watch?v=V9zuox47zr0&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=9), or [this article from W3Schools](https://www.w3schools.com/cpp/cpp_functions.asp).