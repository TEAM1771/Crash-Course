# Why do we use C++?

Let's start with the basics. A robot is a collection of hardware and electronics which interact in order to perform tasks. Programming is required to coordinate all of parts. The "brain" of the robot is the RoboRio, a small computer which connects up to all the electronics and runs the software which you see in this GitHub. It can connect over WiFi or USB to another computer in order to control the robot. 

Now, it would be very difficult to write software entirely from scratch to run on the RoboRio. As is the case in most software projects, common "libraries" are used which contain methods to connect with the operating system and the various electronics connected to the RoboRio. This collection is called WPILib. WPILib is available in both Java and C++ versions.

The RoboRio is not a very powerful computer, and has limted resources. As such, it is important to keep the software we write "light" and "efficent" so that the robot is always responsive (you want the robot to stop moving when you let go of the joystick).

Java is a commonly used programming language among FRC teams. It is considered easy to learn as its syntax is easily understood and memory management is automatically handled. However, Java is not the best solution for relatively low-memory situations like the RoboRio.

C++, on the other hand, was built on top of C, a language designed to run efficently while providing an easier time for programmers. It was created in 1985 and is a tried and true low-level langauge (runs directly on hardware). Its syntax is similar to Java, but include a lot more features to give the programmer more flexibility. In addition, while compiling, the C++ optimizer indentifies inefficent or unused code and optimizes it to the maximum. While it does have a higher skill ceiling compared to Java, it is not difficult to get started with.

We use C++ to maximimze our efficency and get the most out of the RoboRio. In addition, the added flexibility of its syntax and libraries provide us with a lot of options. Finally, knowing C++ is great skill and it will serve you well into the future. It makes up the base of operating systems and software for devices around the world.
