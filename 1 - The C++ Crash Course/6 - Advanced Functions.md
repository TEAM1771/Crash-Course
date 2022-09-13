# Advanced Functions

## Overloading

### Overloading involves making multiple functions with different return types or parameters

The concept of overloading allows you and others who may use your code to select a function depending on their needs. For example, you may have a function which accepts an `int` and another version which accepts a `double`, and both of those return the same variable type.

```
int divide(int a, int b)
{
    return a / b;
}

double divide(double a, double b)
{
    return a / b;
}
```

This can also be used for constructors and functions in classes or structures. In the following example, there are 4 constructors to accomodate 4 different scenarios.

```
class Robot
{
    int ID;
    std::string name;

    public: 
    
    Robot() // User provides no info
    {
        ID = 0; // ID is assumed to be 0
        std::cout << ID;

        name = ""; // Name is set to blank
    }

    Robot(int robots_ID) // User only provides ID
    {
        ID = robots_ID;
        std::cout << ID;

        name = ""; // Name is set to blank
    }
        
    Robot(std::string robots_name) / User only provides name
    {
        ID = 0; // ID is assumed to be 0
        std::cout << ID;

        name = robots_name;
    }

    Robot(int robots_ID, std::string robots_name) // User provides both
    {
        ID = robots_ID;
        std::cout << ID;

        name = robots_name;
    }
};

Robot robot_1 = Robot(1, "Robosaurus"); // ID = 1, prints 1, name = "Robosaurus"
Robot robot_2 = Robot(2); // ID = 2, prints 2, name = ""
Robot robot_3 = Robot("O-Bob"); // ID = 0, prints 0, name = "O-Bob"
Robot robot_4 = Robot(); // ID = 0, prints 0, name = ""
```

### More resources: [Microsoft Docs](https://docs.microsoft.com/en-us/cpp/cpp/function-overloading?view=msvc-170), [Geeks4Geeks](https://www.geeksforgeeks.org/function-overloading-c/)

## Default Parameters

Some scenarios can also be handled by using **default** parameters. A default parameter is one which is supplied with a "default" value that is used when the function caller does not provide an argument for that parameter. This applies to functions in and outside classes/structs.

```
class Robot
{
    int ID;
    std::string name;

    public: 
    

    Robot(int robots_ID = 0, std::string robots_name = "")  // If the user doesn't provide a name, it is set to "". 
                                                            // If they don't provide a name or an ID, ID is also set to 0.
    {
        ID = robots_ID;
        std::cout << ID;

        name = robots_name;
    }
};

Robot robot_1 = Robot(1, "Robosaurus"); // ID = 1, prints 1, name = "Robosaurus"
Robot robot_2 = Robot(2); // ID = 2, prints 2, name = ""
Robot robot_3 = Robot(); // ID = 0, prints 0, name = ""
Robot robot_3 = Robot(""); // This code won't work. The parameters must always be given in order
```

Default parameters do not allow for switching up the order of parameters. In addition to the `robot_3` above, the following example wouldn't work:

```
int modify(int a = 0, int b);
```

Once you set an default parameter, the rest must also have defaults. This limits its use case in scenarios where the order in which arguments are provided does not follow a pattern.

However, in functions handling extra parameters that are rarely changed, defaults parameters are very useful. The following is an example from our code.

```
SparkMax1771(int id,
                rev::CANSparkMax::IdleMode idle_mode,
                double min_position = -std::numeric_limits<double>().infinity(),
                double max_position = std::numeric_limits<double>().infinity(),
                CANSparkMax::ControlType control_type = ControlType::kPosition,
                MotorType motor_type = MotorType::kBrushless);
```

The class is titled "SparkMax1771" and handles the SparkMax motor controllers on our robot. It takes in the `id` of the SparkMax on our robot's CAN network, followed by the `idle_mode`, which defines what the motor will do when it is told to stop: just gently slow down or quickly brake. These are both mandatory, as they differ for every motor. 

However, the rest of the variables are optional. `min_position` and `max_position` define the limits for our motor (this is useful for motors that may break something if they go too far, such as on our turret and climber, but for motors involving parts that can spin infinitely, a limit of +/- infinity will do). We never change the ControlType from its default, which determines how the motor is controlled. The last variable, `motor_type`, is also never modified from the default in our code because all of our motors are brushless (a type of motor technology). However, these optionaly variables are included so that we can re-use the code year-to-year and accomodate different scenarioes.

The following are all examples from our code of making an **instance** of `SparkMax1771`:

```
// The turret is constructed with an "id" of 4, an "idle_mode" of Coast, and is locked to position zero
// We lock the turret to 0 to make sure it doesn't move until the climber is confirmed to be out of the way of the turret
SparkMax1771 turret_turny_turny{4, CANSparkMax::IdleMode::kCoast, Turret::POSITION::ZERO, Turret::POSITION::ZERO};

// The intake is constructed with an "id" of 9, an "idle_mode" of Brake, and is locked between its max up and down positions
SparkMax1771 intake{9, rev::CANSparkMax::IdleMode::kBrake, UP_POS, DOWN_POS};

// The hood is constructed with an "id" of 5, an "idle_mode" of Coast, and is locked between its max up and down positions
SparkMax1771 hood{5, rev::CANSparkMax::IdleMode::kCoast, Hood::MAX, Hood::DOWN};
```

### More information from [Geeks4Geeks](https://www.geeksforgeeks.org/default-arguments-c/)