<!-- TOC -->

- [Method Overloading](#method-overloading)
  - [Why is method overloading not possible by changing the return type in java?](#why-is-method-overloading-not-possible-by-changing-the-return-type-in-java)
  - [Can we overload the methods by making them static?](#can-we-overload-the-methods-by-making-them-static)
  - [Can we overload the main() method?](#can-we-overload-the-main-method)
- [Method Overriding](#method-overriding)
  - [Can we override the static methods?](#can-we-override-the-static-methods)
  - [Can we override the overloaded method?](#can-we-override-the-overloaded-method)
  - [Can we override the private methods?](#can-we-override-the-private-methods)
  - [Can we change the scope of the overridden method in the subclass?](#can-we-change-the-scope-of-the-overridden-method-in-the-subclass)
  - [Can we modify the throws clause of the superclass method while overriding it in the subclass?](#can-we-modify-the-throws-clause-of-the-superclass-method-while-overriding-it-in-the-subclass)

<!-- /TOC -->

# Method Overloading

In Java, method overloading is a feature that allows a class to have multiple methods with the same name but different parameters. This means that you can create multiple methods with the same name but different parameter lists.

```java
public class MyClass {
    public void print(String s) {
        System.out.println(s);
    }

    public void print(int i) {
        System.out.println(i);
    }

    public void print(double d) {
        System.out.println(d);
    }
}
```

In this example, the `MyClass` class has three methods with the same name `print()` but different parameter lists. This means that you can call the `print()` method with a String, int, or double argument, and the compiler will know which method to call based on the type of the argument.

```java
public class Main {
    public static void main(String[] args) {
        MyClass myObject = new MyClass();
        myObject.print("Hello"); // Prints "Hello"
        myObject.print(10); // Prints "10"
        myObject.print(3.14); // Prints "3.14"
    }
}
```

In this example, we create an object of the `MyClass` class and call the `print()` method with different arguments. The compiler knows which method to call based on the type of the argument.


## Why is method overloading not possible by changing the return type in java?

In Java, method overloading is not possible by changing the return type of the program due to avoid the ambiguity.

```java

class Adder{  
static int add(int a,int b){return a+b;}  
static double add(int a,int b){return a+b;}  
}  
class TestOverloading3{  
public static void main(String[] args){  
System.out.println(Adder.add(11,11));//ambiguity  
}}  

```

```bash
Compile Time Error: method add(int, int) is already defined in class Adder
```

## Can we overload the methods by making them static?

No, We cannot overload the methods by just applying the static keyword to them(number of parameters and types are the same). Consider the following example.

```java

public class Animal  
{  
    void consume(int a)  
    {  
        System.out.println(a+" consumed!!");  
    }  
    static void consume(int a)  
    {  
        System.out.println("consumed static "+a);  
    }  
    public static void main (String args[])  
    {  
        Animal a = new Animal();  
        a.consume(10);  
        Animal.consume(20);  
    }  
}    

```

```bash

Animal.java:7: error: method consume(int) is already defined in class Animal
    static void consume(int a)
                ^
Animal.java:15: error: non-static method consume(int) cannot be referenced from a static context
        Animal.consume(20);
              ^
2 errors

```

## Can we overload the main() method?

Yes, we can have any number of main methods in a Java program by using method overloading.



# Method Overriding

In Java, method overriding is a feature that allows a subclass to provide a different implementation of a method that is already defined in its superclass. This means that you can create a method in a subclass that has the same name, return type, and parameters as a method in its superclass, but a different implementation.

```java
public class Animal {
    public void makeSound() {
        System.out.println("Generic animal sound");
    }
}

public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Woof!");
    }
}

public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Meow!");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Dog();
        animal.makeSound(); // Prints "Woof!"

        animal = new Cat();
        animal.makeSound(); // Prints "Meow!"
    }
}
```

In this example, the `Animal` class has a method called `makeSound()`. The `Dog` and `Cat` classes override this method to provide a different implementation. When the `makeSound()` method is called, the actual method that is executed is determined at runtime, based on the type of the object that is being used.

## Can we override the static methods?

No we cannot override the static methods because the static methods are the part of class not object. If we declare the same method in the subclass then it will hide the superclass method instead of overriding it.

## Can we override the overloaded method?

Yes, we can override the overloaded method in the subclass.

## Can we override the private methods?

No, we cannot override the private methods because the scope of private methods is limited to the class and we cannot access them outside of the class.

## Can we change the scope of the overridden method in the subclass?

Yes, we can change the scope of the overridden method in the subclass. However, we must notice that we cannot decrease the accessibility of the method. The following point must be taken care of while changing the accessibility of the method.

 - The private can be changed to protected, public, or default.
 - The protected can be changed to public or default.
 - The default can be changed to public.
 - The public will always remain public


## Can we modify the throws clause of the superclass method while overriding it in the subclass?

Yes, we can modify the throws clause of the superclass method while overriding it in the subclass. However, there are some rules which are to be followed while overriding in case of exception handling.

 - If the superclass method does not declare an exception, subclass overridden method cannot declare the checked exception, but it can declare the unchecked exception.
 - If the superclass method declares an exception, subclass overridden method can declare same, subclass exception or no exception but cannot declare parent exception.