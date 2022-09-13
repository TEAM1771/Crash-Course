# Headers

## The build process and multiple files

Everything we have learned so far has assumed that our entire C++ program exists within one file. However, this is not realisitic for real-world programs, because that file would soon get very large and difficult to organize. In addition, you need multiple files when you include dependencies, or code that other people write (including the C++ standard library). 

To accomplish this, you can write multiple ".cpp" files that can compile seperately and then be "linked" by a linker program. This allows you to write a function in one file but use it in another. 

During the build process, the compiler turns each ".cpp" file (aka a "translation unit") into a ".o" (object) file. Following that, the linker links the .o files, finding functions (and even variables, but we won't get into that now) that exist in one object file and allowing them to be used in other files. However, there is one major problem: compiling happens before linking. This means that the compiler has no clue which functions exist in other files. To fix this, you must include the **"declaration"** of each function in each ".cpp" file. As a reminder, the declaration of a function is the return type, the name, and the parameters, but not the logic inside brackets.


### File Drive.cpp:

```
Robot robot{2};

void driveRobot(int speed_percentage)
{
    robot.move(speed_percentage);
}
```

### File Main.cpp:

```

void driveRobot(int speed_percentage); // By including the function declaration, 
                   // you are letting the compiler know that this function exists

int main()
{
    driveRobot(1); // This now works because the linker connects this function call to Drive.cpp
}
```

## Header Files 

Now let's say you have 10 files in your program, and each of those files needs to access 2 or 3 common functions found in a single .cpp file. It would start to get very annoying if you had to copy those functions' declarations over and over again. To solve this problem, **header files** were introduced (this can either be ".h" or ".hpp" files).

We have already mentioned the `#include` statement when discussing the standard library. `#include` is just a fancy way of saying "copy-and-paste" the contents of this header file into here." `#include` is actually a preprocessor statement, which means it is run before the compiler. This allows you to store function declarations inside a header file, and then include the header file wherever you need to use that function.

### Following the example from earlier, it can be re-written as:

#### Drive.cpp:
```
Robot robot{2}; // Assuming Robot is a class somewhere in this file

void driveRobot(int speed_percentage)
{
    robot.move(speed_percentage);
}
```

#### Drive.hpp:
```
void driveRobot(int speed_percentage);
```

#### Main.hpp:

```
#include "Drive.hpp" // Copy-pastes Drive.hpp into here

int main()
{
    driveRobot(1); // This now works because the linker connects this function call to Drive.cpp
}
```

### Here is an example of the Intake.hpp and Intake.cpp files from our codebase

#### Intake.hpp:
```
#pragma once
#include "Buttons.hpp"

namespace Intake
{
    /******************************************************************/
    /*                  Public Function Declarations                  */
    /******************************************************************/

    void init();
    
    void deploy(bool deploy);

    void goHalfwayDown();

    [[nodiscard]] double getTemp();

    void tune(JoystickButton const &go_between_setpoints);
}
```

#### Intake.cpp:
```
#include "Intake.hpp" // This must be included so that Intake.cpp is aware of the namespace Intake and its contents
#include "SparkMax1771.hpp" // This contains the SparkMax1771 class used later

/******************************************************************/
/*                       Private Constants                        */
/******************************************************************/

constexpr auto UP_POS = 3.1771;
constexpr auto DOWN_POS = 74.501771;
constexpr auto HALF_POS = 50;

/******************************************************************/
/*                        Private Variables                       */
/******************************************************************/

static SparkMax1771 intake{9, rev::CANSparkMax::IdleMode::kBrake, UP_POS, DOWN_POS};

/******************************************************************/
/*                   Public Function Definitions                  */
/******************************************************************/
void Intake::init()
{
    intake.setPID(0.1, 0, 0);

    intake.SetSmartCurrentLimit(20);

    intake.BurnFlash();
}

void Intake::deploy(bool deploy)
{
    if (deploy)
        intake.setTarget(DOWN_POS);
    else
        intake.setTarget(UP_POS);
}

double Intake::getTemp()
{
    return intake.GetMotorTemperature();
}

void Intake::tune(JoystickButton const &go_between_setpoints)
{
    if (go_between_setpoints)
        intake.setTarget(50);
    else
        intake.setTarget(20);
}

void Intake::goHalfwayDown()
{
    intake.setTarget(HALF_POS);
}
```

As you can see, public functions (to be accessed from outside the Intake.cpp file) are declared in "Intake.hpp", inside the "Intake" namespace, but their definitions remain inside "Intake.cpp" along with variables which don't need to be accessed from elsewhere

Inside "Intake.cpp", function declarations must be written as "Intake::" followed by the function name.

#### If we wanted to use any of the functions in the Intake namespace, we could simply use #include "Intake.hpp"

## #pragma once

You may have noticed the statement `#pragma once` in the Intake.hpp file. Remember that `#include` simply copy-and-pastes the contents of a header file? Imagine a scenario in which you incude a header file more than once in a .cpp file. This would mean that .cpp file contains multiple declarations of a function, which would prevent compilation as this is not allowed. This may sound unlikely, but it is actually very common. 

Let's say you have multiple header files that each have function(s) that have a parameter or return type that is a unit from the WPILIB library (units::degree_t, units::meters_per_second_t, etc.). Each of those header files would need to include that unit class from the WPILIB library by utilizing `#include`. This happens all the time in our code base. Now, if you were to include those header files in one .cpp file, compilation would fail, as the unit would be declared more than once. 

#### To solve this, the `#pragma once` statement checks to see if that particular .hpp file has been included already.

## Variables in header files

Variables can also be written in header files, so that they can be accessed in multiple files. This is useful if you have a constant that needs to be utilized by different functions (i.e. the max speed of the robot, the location of the robot's wheels, etc.). 

### Here's an example from Drivetrain.hpp

```
// Absolute max module speed
constexpr units::meters_per_second_t MODULE_MAX_SPEED = 13.9_fps;

/*
Max effective speed considering pi radians/second max angular speed
ROBOT SAT: 9.535f/s
*/

// Formula for determing ROBOT_MAX_SPEED is Wheel Max Speed = Robot Max Speed + Omega max speed * distance of module from center
// Or Robot Max Speed = Max wheel speed - Omega max speed * distance from center
// Distance of module from center is 1.294ft

// Max effective linear speed
constexpr units::meters_per_second_t ROBOT_MAX_SPEED = 9.533_fps;
constexpr units::radians_per_second_t ROBOT_MAX_ANGULAR_SPEED{2 * wpi::numbers::pi};

constexpr units::meters_per_second_t TELEOP_MAX_SPEED = ROBOT_MAX_SPEED;
constexpr units::radians_per_second_t TELEOP_MAX_ANGULAR_SPEED{1.25 * wpi::numbers::pi};

constexpr units::meters_per_second_t TELEOP_SHOOTING_MAX_SPEED = 5_fps;
constexpr units::radians_per_second_t TELEOP_SHOOTING_MAX_ANGULAR_SPEED{wpi::numbers::pi};

constexpr units::meters_per_second_squared_t TELEOP_MAX_X_ACCELERATION = TELEOP_MAX_SPEED / 0.3_s;
constexpr units::meters_per_second_squared_t TELEOP_MAX_Y_ACCELERATION = TELEOP_MAX_SPEED / 0.5_s;

constexpr units::radians_per_second_squared_t TELEOP_MAX_ANGULAR_ACCELERATION = ROBOT_MAX_ANGULAR_SPEED / .4_s;

constexpr units::meters_per_second_t TRAJ_DEFAULT_SPEED = 3_mps;
constexpr units::acceleration::meters_per_second_squared_t TRAJ_DEFAULT_ACCELERATION = 3.5_mps_sq;
constexpr units::radians_per_second_t TRAJ_MAX_ANGULAR_SPEED = ROBOT_MAX_ANGULAR_SPEED / 2;
constexpr units::radians_per_second_squared_t TRAJ_MAX_ANGULAR_ACCELERATION{wpi::numbers::pi};
constexpr auto TRAJ_DEFAULT_ROT_P = 4;

constexpr auto DEFAULT_ROT_P = 3; // Modifier for rotational speed -> (degree * DEFAULT_ROT_P) / 1sec
constexpr units::radians_per_second_t FACE_DIRECTION_MAX_SPEED{1.5 * wpi::numbers::pi};

inline frc::SwerveDriveKinematics<4> kinematics{frc::Translation2d{11.875_in, 11.875_in},
                                                frc::Translation2d{11.875_in, -11.875_in},
                                                frc::Translation2d{-11.875_in, 11.875_in},
                                                frc::Translation2d{-11.875_in, -11.875_in}};
```

Normally, variables included into multiple C++ files with the same name would cause an issue, but this can be resolved in 3 ways.

- `Static` variables are limited to the local scope of their respective file. If you were to use `static` before a variable in a header file, it would be `static` in all the files that `#include` that header file, thereby solving the issue. However, the variables are isolated, so changing variables in one file would not affect the value in another.

- `inline` is a keyword which (used to) hint to the compiler that it should replace where a variable is being used with its actual value, or a function call with the actual defintion of the function, but nowadays compilers are smart enough to determine this on their own. Instead, `inline` makes the compiler link all the instances of that variable, so that the variable maintains the same value between all the files that use it (changing the value in one file changes it in all the files).

- `constexpr` variables are by default "inlined" into where they are used (the compiler replaces where the variable is being used with the actual value of the variable)

### For more resources on headers, check out [this awesome video](https://www.youtube.com/watch?v=9RJTQmK0YPI&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=10), [Microsoft Docs](https://docs.microsoft.com/en-us/cpp/cpp/header-files-cpp?view=msvc-170), or [Geeks4Geeks](https://www.geeksforgeeks.org/header-files-in-c-cpp-and-its-uses/)