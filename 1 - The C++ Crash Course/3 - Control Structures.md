# Control Structures

### If you've ever read some code, you've probably seen control statements like `if`, `else`, `for`, or `while`. "Control structures" (formed from control statements) make up the basis of logic in code. They are called control structures because they affect the "control flow," or the order in which the code "flows" or executes. Normally, the control flow begins at the top of the main function and "flows" down.

```
int a = 0;

void increment()
{
    a++; // 5th
}

int main()
{
    print(a); // This runs first and prints 0

    a += 3; // This runs 2nd

    print(a); // 3rd (this prints 3)

    increment() // 4th, which jumps up to the increment function

    print(a); // And finally this runs 6th (and prints 4)
}
```

### You can think of control flow as a laser pointer which follows the lines of codes and runs each one in order. As stated before, control structures allow us to change control flow.

## IF
The most recognizable control structure starts with the control statement `if`. The idea is simple, if an inputted boolean is `true` (or 1), control flow moves inside of the control structure. Otherwise, control flow passes over it.

```
void check(bool z)
{
    if(z)
    {
        print("Yes");
    }
}
int main()
{
    bool a = true; // Same thing as a = 1
    bool b = 0; // Same thing as b = false

    check(a); // This will result in "Yes" being printed to the console
    check(b); // Nothing will happen here
}
```

## ELSE
The `if` control statement can be paired with `else`. `else` defines an alternative control structure to the `if` structure.

```
void check(bool z)
{
    if(z)
        print("Yes"); // The bracets are not required if only one line follows the control statement
    else
        print("No")
}

int main()
{
    bool a = true; // Same thing as a = 1
    bool b = 0; // Same thing as b = false

    check(a); // This will result in "Yes" being printed to the console
    check(b); // This will result in "No"
}
```

`if` and `else` can be combined to have powerful logic in your code.

```
std::string typeOfNumber(int num)
{
    if(num == 0) // This could also be replaced with if(!num) since 0 is the same thing as false
        return "Neither";
    else if(num % 2 == 0) // If dividing by 2 leaves no remainder. This is the same as if(!(num % 2))
        return "Even";
    else
        return "Odd";
}

int main()
{
    print(typeOfNumber(3)); // Odd
    print(typeOfNumber(0)); // Neither
    print(typeOfNumber(-2)); // Even
}
```

In this case, the `else` statements aren't even necessary, since once the program runs `return`, it exits the function. The following example is equivalent to above.
```
std::string typeOfNumber(int num)
{
    if(!num)
        return "Neither";
    if(num % 2) // If dividing by 2 results in any remainder other than 0, it is interpreted as true
        return "Odd";
    return "Even";
}

int main()
{
    print(typeOfNumber(3)); // Odd
    print(typeOfNumber(0)); // Neither
    print(typeOfNumber(-2)); // Even
}
```

More resources on if/else: [The Cherno video](https://www.youtube.com/watch?v=qEgCT87KOfc&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=12), [W3Schools](https://www.w3schools.com/cpp/cpp_booleans_expressions.asp), 

## FOR

## WHILE

More resources on for/while: [The Cherno video](https://www.youtube.com/watch?v=_1AwR-un4Hk&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=14) and W3Schools ([while](https://www.w3schools.com/cpp/cpp_while_loop.asp) and [for](https://www.w3schools.com/cpp/cpp_for_loop.asp)).