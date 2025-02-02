# Design Patterns

First of all you need to understand what exactly is design patterns, those are the basic fundamentals and principles of object oriented programming through which we are actually able to design scalable and manageable and flexible designs for the software.

## Why Design Patterns?

- Design patterns are the basic fundamentals and principles of object oriented programming through which we are actually able to design scalable and manageable and flexible designs for the software.
- Design patterns are the best practices which are used by the developers to solve the common problems which we face while designing the software.

# Strategy Design Pattern

Suppose we have a we are implementing something related to drivers and vehicles. now imagine that you have a base class where we have the functions for drive display and so on and from that base class we have three or more other classes such as passenger off road vehicle and special vehicle.

Now what will happen is these three classes will be implementing their own things for example the passenger vehicle will simply use the base class so that's perfect

But on the other hand off road vehicle will be implementing their own special drive function or something like that same goes for the special vehicle. as a result all of these leads to some duplicates in the code which is not a very good thing to be honest. Plus LESS REUSABILITY.

## How to solve this issue ?

That can be made in a different manner suppose we have a interface or a driver strategy whatever you wanna call it, inside that we can have different concrete classes or something like that for different types of driving method like special driving race driving and so on.

And whenever any kind of vehicle comes into play they can simply we can simply pass them an object and they can simply be using those instead of having to write their own logic so we are not writing anything inside of that vehicle class.

![Alt text](https://i.ibb.co/G5Qt58m/Screenshot-1.png)

```java

public interface DriverStrategy {
    void drive();
}

public class NormalDriving implements DriverStrategy {
    @Override
    public void drive() {
        System.out.println("Normal Driving");
    }
}

public class OffRoadDriving implements DriverStrategy {
    @Override
    public void drive() {
        System.out.println("Off Road Driving");
    }
}

public class SportsDriving implements DriverStrategy {
    @Override
    public void drive() {
        System.out.println("Sports Driving");
    }
}

public class Vehicle
{
    DriverStrategy driverStrategy;

    // constructor injection
    Vehicle(DriverStrategy driverStrategy)
    {
        this.driverStrategy = driverStrategy;
    }

    public void drive()
    {
        driverStrategy.drive();
    }
}


public class OffRoadvehicle extends Vehicle
{
    OffRoadvehicle()
    {
        super(new OffRoadDriving() );
    }
}

```
