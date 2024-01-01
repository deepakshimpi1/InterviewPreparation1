

Design patterns are evolved as resuable solutions to the problems that we encounter every day programming.

Gangs of 4 - `Elements of resulable Object-oriented software`

Types:
1. Creational
- This type deals with the object creation and initialization.
- This pattern gives the program more flexibility in deciding which objects needs to be created for a given case.
- Example, Singleton, Factory, Abstract Factory, Builder and Prototype

2. Structural
- This type deals with class and object composition
- This pattern focuses on decoupling interface and implementation of classes and it's objects
- Example: Adaptor, Bridge..

3. Behavioural
- This type deals with the communication between classes and objects
- Example. Chain of Responsibility, Command, Interpreter..

---

## Singleton Pattern:

- Creational design pattern
- Ensures a `class has only one instance` and provides a `global point of access` to that instance.
- It is commonly used in scenarios `where a single instance of a class is required` to coordinate actions across the system.

- Singleton Design Pattern Explanation:

- Step 1. Private constructor to prevent instantiation
- Step 2. Private static variable to hold single instance
- Step 3. `Public` static method for instance access
- Step 4. Mark class as sealed

```
public sealed class Singleton{
    //Step 2
    private static Singleton instance = null; 

    //Step 1
    private Singleton{
        counter++;
        Console.WriteLine("Counter: " + counter);
    }
    //Step 3
    public static Singleton GetInstance{
        if(instance == null)
        {
            instance  = new Singleton();
        }
        return instance;
    }

    public void GetDetails(){
        Console.Writeline("Invoked")
    }  
}
```

Usage Example:

```
public class Program{
    static void main(string args[])
    {
        // Get the Singleton Instance
        Singleton obj1 = Singleton.GetInstace();
        Singleton obj2 = Singleton.GetInstace();

        // Check if both instances refer to the same object
        System.out.println(obj1 == obj2);  // Output: true

        // Use the Singleton instance
        obj.GetDetails();
    }
}

OUTPUT: counter 1
```
- In this example, Singleton.GetInstance() always returns the same instance of the Singleton class. This ensures that there is only one instance of the class throughout the program.

### Why is Singleton class sealed?


```
public class Singleton1 {
            
    static int counter = 0;

    private static Singleton1 instance = null;    

    private Singleton1(){
        counter++;
        Console.WriteLine("Counter: " + counter);
    }

    public static Singleton1 GetInstance()
    {
        if(instance == null)
        {

            instance = new Singleton1();
        }
        return instance;

    //Nested class
    public class Singleton2: Singleton1 {

    }
} 

Main()
{
    // This will create multiple instace of class
    // This violates the Singleton design pattern
    Singleton1 obj1 = Singleton1.GetInstance();
    Singleton1.Singleton2 obj2 = new Singleton1.Singleton2();
}

OUTPUT: counter 1
        counter 2    
```
 
### Thread safty using Lazy loading:

- Egar loading: it is also thread safe

```
class Singleton{
private Singleton{

}
private static readonly Singleton instance = new Singleton()

public static Singleton GetInstance
{
    get {
        return instance;
    }
}
}
```

- Lazy is bydefault thread safe. 
- Implement using `Lazy` keyword
- When multiple thread access it it won't create new instance.

```
class Singleton{
private Singleton{

}
private static readonly Lazy<Singleton> instance = new Lazy<Singleton>(()=> new Singleton())

public static Singleton GetInstance
{
    get {
        return instance.Value;
    }
}
}
```

### Static class vs Singleton

No.|Static|Singleton
|---|---|---
|1. |It is a keyword |It is a design pattern 
|2. |It gives static members(method) | It gives an object
|3. |Static can not be passed as a reference| Singleton object can be passed as a reference 
|4. |Static can not implement interfaces, inherit from other classes| Singleton can implement interfaces and inherit from other classes 
|5. |It does not supports object disposal|It supports object disposal
|6. |Static objects can be stored on stack|Singleton objects can be stored on heap

### Project example

----

## Factory Design Pattern:

- Define an interface for creating an object, but let subclasses decide which class to instantiate.
- Factory pattern creates object without exposing the creation logic to client and refer newly created object using a common interface.

- Client -----(uses)----> Factory -----(creates)------> Product

- Client uses and factory to create instance of Product.
- Rather than creating Product instance directly, Client delegates the responsibilty to the Factory.
 
### Choose factory pattern when

- The object needs to be extended to subclasses
- The classes doesn't know what exact sub classes it has to create.
- The product implementation tend to change over time and the client remains unchanged.

**Summary:-**
- Simple factory `abstracts` the creation details of the product
- Simple factory refers to the newly created object through `an interface`.
- Any new type creation is handled with a change of code in the factory class and `not` at the client code.

### Project example

---

## Abstract Factory Pattern:

- Abstract factory is a super factory that creates other factories.
- This pattern provides a way an interface for creating families of related and dependent objects without specifying their concrete classes.

Exmaple: https://www.youtube.com/watch?v=z47aJGe7jR4&list=PL6n9fhu94yhUbctIoxoVTrklN3LMwTCmd&index=10

```
using System;

// Abstract Factory
public interface IShapeFactory
{
    ICircle CreateCircle();
    IRectangle CreateRectangle();
}

// Abstract Product
public interface ICircle
{
    void Draw();
}

// Concrete Product 1
public class Circle1 : ICircle
{
    public void Draw()
    {
        Console.WriteLine("Circle from Factory 1");
    }
}

// Concrete Product 2
public class Circle2 : ICircle
{
    public void Draw()
    {
        Console.WriteLine("Circle from Factory 2");
    }
}

// Abstract Product
public interface IRectangle
{
    void Draw();
}

// Concrete Product 1
public class Rectangle1 : IRectangle
{
    public void Draw()
    {
        Console.WriteLine("Rectangle from Factory 1");
    }
}

// Concrete Product 2
public class Rectangle2 : IRectangle
{
    public void Draw()
    {
        Console.WriteLine("Rectangle from Factory 2");
    }
}

// Concrete Factory 1
public class ConcreteShapeFactory1 : IShapeFactory
{
    public ICircle CreateCircle()
    {
        return new Circle1();
    }

    public IRectangle CreateRectangle()
    {
        return new Rectangle1();
    }
}

// Concrete Factory 2
public class ConcreteShapeFactory2 : IShapeFactory
{
    public ICircle CreateCircle()
    {
        return new Circle2();
    }

    public IRectangle CreateRectangle()
    {
        return new Rectangle2();
    }
}

// Client Code
public class Client
{
    public void CreateAndDrawShapes(IShapeFactory factory)
    {
        var circle = factory.CreateCircle();
        var rectangle = factory.CreateRectangle();

        circle.Draw();
        rectangle.Draw();
    }
}

// Example Usage
class Program
{
    static void Main()
    {
        var factory1 = new ConcreteShapeFactory1();
        var client1 = new Client();
        client1.CreateAndDrawShapes(factory1);

        var factory2 = new ConcreteShapeFactory2();
        var client2 = new Client();
        client2.CreateAndDrawShapes(factory2);
    }
}
```
- In this C# example, we have interfaces for the abstract factory (IShapeFactory), abstract products (ICircle and IRectangle), and their concrete implementations. The client code (Client) can create and draw shapes using any factory, and the product instances are independent of the concrete factory used to create them.