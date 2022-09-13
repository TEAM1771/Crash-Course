# Variables
	
### Variables in programming languages are very similar to the X,Y, and Zs you use in your math class. The heart of any computer program is working with data. Variables can be used to hold data and allow it to be modified by the program (hence *variable*). Variables are usually stored in the memory of the computer, but we'll get into exceptions later. 

## Types of Variables

Let's talk about types of variables. All variables are eventually represented as 0s and 1s in the computers memory, but different types of variables allow programmers to more easily work with different types of data. The most familiar might be `int`, short for integer (any non-decimal number). Here's a sample line of code.

`int a = 5;`

The first word, `int`, declares the type of variable. The second is the name of that variable (can be any combination of letters). Next, an equals sign is used to *initialize* the variable with a value of 5. Finally, each line of code ends in a semicolon.

Later on in the code, this variable can be retrieved, modified, deleted, etc. 

`print(a);` - Outputs 5

`a = 3;`

`print(a);` - Outputs 3`

`int b = a - 2;`

`print(b);` - Outputs 1

As you can see, variables can also be utilized in functions (covered in the next lesson), such as the print function shown above (not a real function), and with math.

If you're still confused or want to learn more, check out this [video](https://www.youtube.com/watch?v=zB9RI8_wExo&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=8)!

## Operators

+, -, /, *, and % are all valid operations. / divides 2 variables and always rounds down (leaves out any remainder), whereas % (the modulo operator) gives you the remainder. On variables that aren't numbers, these operators usually don't work or have different behavior.

`int a = 3;`

`int b = 5;`

`int c = a + b;` 8

`int d = c * b;` 40

`int e = b - d;` -35

`int division = d / a;` 13

`int modulo = d % a;` 1

The simplest variable type is `bool`, short for boolean, and represents a `1` or `0`, aka `true` or `false`. 

- The operator `!` inverts a boolean, so `true` becomes `false` and vise-versa. 

Also, "logical operators" can be used to compare variables and output a boolean. 

- The `==` (equals) operator compares two variables to determine if they are equal.
    - 5 == 5 can be replaced with true
    - 3.2 == 5.9 can be replaced with false
    - true == false can be replaced with false (or 1 == 0 is 0)
- The `!=` (not equals) operator does the same, but inverts the result
    - 5 != 5 is false
    - 3.2 != 5.9 is true
    - true != false is true (or 1 != 0 is 1)
- The `&&` (logical and) operator results in true only if both booleans are true
    - false && false is false (or 0 && 0 is 0)
    - false && true is false (0 && 1 is 0)
    - true && false is false (1 && 0 is 0)
    - true && true is true (1 && 1 is 1)
- The `||` (logical or) operator results in true if either boolean is true
    - false || false is false (or 0 || 0 is 0)
    - true || false is true (1 || 0 is 1)
    - false || true is true (0 || 1 is 1)
    - true || true is true (1 || 1 is 1)

Some other common variable types include 
 - `double` (a way of representing numbers with decimals)
 - `char` (characters)
 - `string` (arrays or strings of characters, like words and sentences)

## Constants

To make it more confusing, not all variables are actually *variable*. Oftentimes, programmers utilize **constants** to store and/or work with data that doesn't/shouldn't be changed. 

### Constexpr

For example, there is no reason why the radius of the wheels on our robots would change while the program is running. In this case, the radius is already known when we write the software. For data like this, C++ has a feature called `constexpr`**` which allows placeholder variables to be utilized when writing the code. It is much easier to write and read code that uses the radius as a variable, so that the number has some meaning.

`constexpr units::meter_t WHEEL_RADIUS = 2_in;`

Later on, when doing the math for driving the robot, it is much more understandable to read

`driver.GetSelectedSensorVelocity() / DRIVER_TICKS_PER_WHEEL_RADIAN * WHEEL_RADIUS` 

than `driver.GetSelectedSensorVelocity() / 2653.227492901 * 0.0508`

`Constexpr` variables are not actually stored in the memory of the computer when running the code. Instead, the **compiler** (a software which turns the code we write into something computers can understand) automatically "fills in" the variable wherever it is used. The two lines above woud produced the same software when compiled.

### Classic const

Other types of data may not be known at **"compile time"** (when the program is compiled, so after it has been written but before it is actually run), but still should not be changed once they are known. This is the classic use of `const`. For example, the direction the joysticks are pointed can only be determined while the robot is enabled and recieving the data from the joysticks. However, there is no reason for the software to actually modify the data recieved. Instead, it simply needs to read the numbers and determine which way to move the robot. 

`int const jostick_x_value = PS1.getX();`
`int const jostick_y_value = PS1.getY();`

Later on, the software uses these numbers but has no reason to change them. The code is looped every 20ms, so after every 20ms the variables are deleted and it makes a new const variable with the new values.

Notice that `constexpr` is used on the **left** side of the variable type, while `const` is on the **right**.