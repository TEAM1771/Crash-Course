# Objects

So far, we have explored the basic variable types (`int`, `double`, `char`, `bool`, etc.), but you have probably also noticed different variable types thrown in (`units::meters_per_second_t`, `units::degree_t`, `std::string`, etc.). These are called objects, and can be either **classes** or **structures**. 

### An object is essentially a way of creating a custom variable, including data and logic

A class or structure is a made in a similar way to a namespace. You begin by typing `class` or `struct`, followed by the name of the class, and enclose the contents in brackets. Unlike namespaces, classes must end in a semicolon.

```
class Robot
{
    
};
```

```
struct Robot
{
    
};
```

A class or struct can contain **variables** (aka **members**), as well as **functions** (sometimes called **methods** when used in a class/struct). 

## Constructor
One function which **all** classes and structures have (*whether or not you choose to declare it*) is called the **constructor**. This does exactly what it says: constructs the object. This function is run any time you create an **instance** of the class or struct (as a variable). It has no return type, and is called the same name as the object. It can also take parameters if needed.

```
class Robot
{
    int ID;

    public: 
    Robot(int robots_ID)
    {
        ID = robots_ID;
        std::cout << ID;
    }
};

Robot robot_1 = Robot(1); // ID = 1, prints 1
Robot robot_2 = Robot(2); // ID = 2, prints 2
Robot robot_3{3}; // This is an alternative way of intializing a variable of class.
                  // ID = 3, prints 3
```

#### More reading: [W3Schools](https://www.w3schools.com/cpp/cpp_constructors.asp), [Geeks4Geeks](https://www.geeksforgeeks.org/constructors-c/), [Microsoft Docs](https://docs.microsoft.com/en-us/cpp/cpp/constructors-cpp?view=msvc-170), and [The Cherno video](https://www.youtube.com/watch?v=FXhALMsHwEY&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=24)

## Visibility and Accessibility

By default, any variables or functions declared in a class are **private**. This means they can only be accessed by other functions in the class. However, anything following `public: ` will be able to seen, modified, and used in other code. 

For structures, it is the other way around: everything is **public** by default, and you must use `private: ` to hide a variable or function. 

Because of this dfference, structures are usually used as a way of holding many public variables, while classes are typically used more often when logic and hidden variables are involved. However, both can be used interchangably. For the purposes of this guide, I will only refer to classes from now on, but know that everything also applies to structures.

Class functions and variables can be accessed by using the name of an **instance** of a class, followed by a `.` and the name of the variable or function.

```
class Robot
{
    int ID;

    public: 
    
    std::string name;

    Robot(int robots_ID, std::string robots_name)
    {
        ID = robots_ID;
        std::cout << ID;

        name = robots_name;
    }

    void setNewName(std::string new_name)
    {
        name = new_name;
    }

    void setNewID(int new_ID)
    {
        ID = new_ID;
    }
};

Robot robot_1 = Robot(1, "Robosaurus"); // ID = 1, prints 1

robot_1.ID = 2; // This won't work because "ID" is a private variable.

robot_1.setNewID(2); // This is fine because "setNewID" is a public function.

robot_1.name = "O-Bob"; // This works because "name" is a public variable

robot_1.setNewName("MOMENTARY"); // This is also public.
```

For more on visiblity, watch [this video from The Cherno](https://www.youtube.com/watch?v=6OVQ8nh3KP0&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=29), or 

## Static in Objects

The keyword `static` in C++ has 4 uses. This will talk about the use of static specifically in classes/structs, but the other 2 uses will be covered in the next guide.

`Static` can be used in classes to create static variables or functions, and it is used by typing `static` before the variable or function declaration.

- Static variables in a class: Shared between instances of a class and can be accessed outside an instance (if public)
- Static class functions: Can be accessed/called without creating an instance (if public)

Static variables and functions can be accessed outside of an instance by using the name of the class, followed by `::` (2 colons) (the same process as accessing something in a namespace).

```
class Robot
{
    int ID;

    public: 
    
    std::string name;

    static int num_of_robots = 0;

    Robot(int robots_ID, std::string robots_name)
    {
        ID = robots_ID;
        std::cout << ID;

        name = robots_name;

        num_of_robots++; // Same as num_of_robots = num_of_robots + 1;
    }
};

Robot robosaurus{0, "Robosaurus"};
Robot OBob{1, "O-Bob"};

std::cout << Robot::num_of_robots; // Outputs 2
std::cout << robosaurus.num_of_robots; // Outputs 2
std::cout << OBOB.num_of_robots; // Outputs 2

OBOB.num_of_robots = 3;

std::cout << robosaurus.num_of_robots; // Outputs 3
```

Static variables are created when the program first runs and are then shared between every instance of that class. This is very useful for keeping a shared number or counting the number of instances of a class. Static variables can also be private:

```
Class Demo
{
    static int private_var = 0;

    public:

    void setPrivateVar(int new_val)
    {
        private_var = new_val;
    }

    int getPrivateVar()
    {
        return private_var;
    }
};

Demo demo_1 = Demo();
Demo demo_2{}; // equivalent to the line above

demo_1.setPrivateVar(3);
std::cout << demo_2.getPrivateVar(); // Outputs 3
```

The code above is an example of a common OOP format: [encapsulation](https://www.w3schools.com/cpp/cpp_encapsulation.asp) (object-oriented programming is a style of programming implemented in many programming languages including Java and C++). This involves making a private variable which can be accessed only through functions (we do not use this format, but it is useful to know if you plan on taking AP CSA).

Static functions are useful if you have some logic that can be recycled throughout all the instances of your class. They can also be used to access a private static variable outside of an instance. Static functions can only access static variables (because you wouldn't be able to access a variable that is specific to an instance of a class when calling the function from outside an instance).

```
Class Demo
{
    static int private_var = 0;

    public:

    static void setPrivateVar(int new_val)
    {
        private_var = new_val;
    }

    static int getPrivateVar()
    {
        return private_var;
    }

    static convert(int a)
    {
        // Does some sort of conversion which is needed in all instances of your class
        return a;
    }
};

Demo demo_1 = Demo();
Demo demo_2{};

Demo::setPrivateVar(3);

std::cout << demo_2.getPrivateVar(); // Outputs 3

Demo::getPrivateVar(); // Also outputs 3

Demo::convert(3);

demo_1.convert(3); // Same result as above
```

#### More reading from [Microsoft Documentation](https://docs.microsoft.com/en-us/cpp/cpp/static-members-cpp?view=msvc-170) and a video from [The Cherno](https://www.youtube.com/watch?v=V-BFlMrBtqQ&list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb&index=22).

### If you have extra time, I suggest learning about the concepts of inheritence, polymorphism, and abstract classes, but these are not required for writing robot code.

## If you are curious about learning more of anything, some great resources include: 

- [GeeksForGeeks](https://www.geeksforgeeks.org/c-plus-plus/?ref=shm) (usually fairly understandable, but doesn't provide as much information)

- [Cpp Reference](cppreference.com) (the real deal; can be a little overwhelming, but full of information)

- [w3schools](https://www.w3schools.com/cpp/) (teaching-orientated site with lots of examples, but it doesn't have a lot of topics or information)

- [Microsoft Documentation](docs.microsoft.com/en-us/cpp/cpp) (a good blend of information, but isn't too difficult to understand)

- You can also look for videos if you prefer to learn from that kind of resource. We highly recommend the [C++ playlist](https://www.youtube.com/playlist?list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb) from "The Cherno" on YouTube.

- We also have some additional resources on #programming in the team's slack

- A quick google search usually returns multiple of the above, but if you search with a specific question or issues, you'll probably find plenty of solutions on Stack Overflow or Reddit

#### For example, for the concepts I mentioned earlier:

- Inheritence: [W3Schools](https://www.w3schools.com/cpp/cpp_inheritance.asp/polymorphism) and [Geeks4Geeks](https://www.geeksforgeeks.org/inheritance-in-c/)

- Polymorphism: [W3Schools](https://www.w3schools.com/cpp/cpp_polymorphism.asp) and [Geeks4Geeks](https://www.geeksforgeeks.org/polymorphism-in-c/)

- Abstract Classes: [Cpp Reference](https://en.cppreference.com/w/cpp/language/abstract_class) and [Geeks4Geeks](https://www.geeksforgeeks.org/pure-virtual-functions-and-abstract-classes/)