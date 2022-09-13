# Scope

### Scope referes to the where a variable can be seen and used

In other words, it means that some variables and functions can only be "seen" and accessed from certain parts of the code. This is best seen with examples.

```
int a = 3; // This is called a "global" variable, as it has "global" scope and can be accessed from anywhere in this file (and possibly by other files as well)

bool check(int b) // The "check" function is "global," so it can be seen & accessed from anywhere in the file.
// The variable 'b' is a "local" variable, and has "local" scope: it can only be seen & accessed inside this function
// This is because it is declared inside / as a part of the function. 
{
    bool c = b == a; // Variable 'c' is also "local," as it is declared inside this function.
    return c;
}

int main() // Technically, this is also a "global" function, but it is usually not called
{  
    bool c = check(a); // Bool 'c' is also "local," because it is declared inside the main function. It cannot be accessed elsewhere.
    // Normally, you cannot have 2 variables with the same name. However, because both 'c's are inside their own functions, they do not see each other.

    int a = 5; // This 'a' varible is actually a "local" variable which hides the "global" variable. This behavior is not normally recommended, as it adds confusion to the code.

    for (int i = 0; i++; i < 5) // The variable 'i' has "statement" scope, as it is only visible inside this "for" statement.
    {
        int div = i / (i + 1); // This variable also has "statement" scope
        std::cout << div;
    }

} 
```

So now you have learned "global," "local," and "statement" scope (you don't need to remember the vocabulary). An easy way to remember is that any variable created within {} or () can only be accessed within its corresponding function/statement. 

## Namespaces

### We can also use scopes to our advantage to keep code more organized and avoid confusion.

On common issue with global scope comes into play when we begin to work with multiple files (more on this in lesson 8 - Headers). Let's say we write a variable:

`constexpr units::meters_per_second_t MAX_SPEED = 9.533_fps;`

This works fine, as long as you remember what this is the max speed for. Let's say we then get into defining the max speed of our intake, and our turret. Suddenly, we have multiple variables with the same name, which is not allowed in "global" scope. Obviously, you could simply rename each MAX_SPEED variable to denote what it belongs to (DRIVETRAIN_MAX_SPEED, TURRET_MAX_SPEED, HOPCORE_MAX_SPEED). However, this quickly becomes a nightmare to read and write again and again, especially for commonly used variables and functions. 

Instead, you could try containing the variables inside {}. However, good luck trying to access that variable or any functions included in those {} elsewhere. They simply won't be accessible. 

```

{
    constexpr units::meters_per_second_t MAX_SPEED = 9.533_fps;

    void max_speed()
    {
        drive(MAX_SPEED);
    }
    
    void inverted_max_speed()
    {
        drive(-MAX_SPEED);
    }
}

int main(
)
{
    max_speed(); // Function not accessible. It is only contained within the scope of those 2 brackets.
}
```

So in conclusion, we need a solution which allows us to contain our variables and functions in a defined scope, but allows us to access those when we need them. In comes: namespaces! Namespaces allow us to define our own scope to contain our variables. 

```
namespace Drive // To make a namespace, type "namespace", and then a name 
{ // Enclose everything you want to be limited to this scope inside brackets
    constexpr units::meters_per_second_t MAX_SPEED = 9.533_fps;

    void max_speed()
    {
        drive(MAX_SPEED);
    }
    
    void inverted_max_speed()
    {
        drive(-MAX_SPEED);
    }
}

int main()
{
    Drive::max_speed(); // To access a function or variable inside a namespace, use the namespace's name, followed by "::" (2 colons).
}
```

Namespaces are a great way of grouping sets of functions or variables which belong to a specific category. For example, this is the namespace for our "Navx," a gyroscope on the robot (only function declarations shown).

```
namespace Navx
{
    void init();

    // Returns values with 0 being front and positive angles going CW
    [[nodiscard]] units::degree_t getAngle();

    void zeroYaw();

    [[nodiscard]] double getPitch();

    [[nodiscard]] frc::Rotation2d getCCWHeading();

    [[nodiscard]] frc::Rotation2d getCWHeading();

    void setNavxOffset(units::degree_t new_offset);
}
```

Everything having to do with Navx can simply be accessed with the "Navx::" prefix, which keeps the code easy to understand and cleaner.

And that's scope!