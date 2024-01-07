## Task And Thread

| Difference | Task                                        | Thread                                                      |
|-------------|---------------------------------------------|-------------------------------------------------------------|
| `Purpose`     | Represents an asynchronous operation and is part of the Task Parallel Library. | Used for creating and manipulating a thread in Windows.      |
| `Result`      | Task can return a result.                   | No direct mechanism to return the result from a thread.      |
| `Cancellation` | Supports cancellation through cancellation tokens. | Thread doesn't support cancellation.                         |
| `Concurrent Processes` | Can have multiple processes happening at the same time. | Can only have one task running at a time.                   |
| `Asynchronous` | Easily implement Asynchronous using 'async' and 'await' keywords. | Asynchronous behavior is not as straightforward.            |
| Thread Pool  | Uses thread pool thread.                    | A new Thread() is not dealing with Thread pool thread.      |
| Level       | Higher level concept than Thread.            | Lower-level concept within a process.                       |

---

## Threading

-  an execution path of a program.
- We can use multiple threads to perform, different tasks of our program at the same time.
- using System.Threading;

```csharp
using System;
using System.Threading;

class Program
{
    static void Main()
    {
        // Creating and starting multiple threads
        Thread thread1 = new Thread(PerformOperation1);
        Thread thread2 = new Thread(PerformOperation2);
        Thread thread3 = new Thread(PerformOperation3);

        Console.WriteLine("Main: Starting multiple threads...");

        // Start the threads
        thread1.Start();
        thread2.Start();
        thread3.Start();

        // Wait for all threads to complete before continuing
        //thread1.Join();
        //thread2.Join();
        //thread3.Join();

        Console.WriteLine("Main: All threads are done!");
    }

    static void PerformOperation1()
    {
        Console.WriteLine($"Thread 1: Starting operation");
        Thread.Sleep(2000); // Simulate some work
        Console.WriteLine($"Thread 1: Operation completed");
    }
    static void PerformOperation2()
    {
        Console.WriteLine($"Thread 2: Starting operation");
        Thread.Sleep(2000); // Simulate some work
        Console.WriteLine($"Thread 2: Operation completed");
    }
    static void PerformOperation3()
    {
        Console.WriteLine($"Thread 3: Starting operation");
        Thread.Sleep(2000); // Simulate some work
        Console.WriteLine($"Thread 3: Operation completed");
    }
}
```
Output:
```
Main: Starting multiple threads...
Thread 1: Starting operation
Thread 2: Starting operation
Thread 3: Starting operation
Main: All threads are done!
Thread 1: Operation completed
Thread 2: Operation completed
Thread 3: Operation completed
```

```csharp
using System;
using System.Threading;

class Program
{
    static void Main()
    {
        // Creating and starting multiple threads
        Thread thread1 = new Thread(() => PerformOperation("Thread 1"));
        Thread thread2 = new Thread(() => PerformOperation("Thread 2"));
        Thread thread3 = new Thread(() => PerformOperation("Thread 3"));

        Console.WriteLine("Main: Starting multiple threads...");

        // Start the threads
        thread1.Start();
        thread2.Start();
        thread3.Start();

        // Wait for all threads to complete before continuing
        thread1.Join();
        thread2.Join();
        thread3.Join();

        Console.WriteLine("Main: All threads are done!");
    }

    static void PerformOperation(string threadName)
    {
        Console.WriteLine($"{threadName}: Starting operation");
        Thread.Sleep(2000); // Simulate some work
        Console.WriteLine($"{threadName}: Operation completed");
    }
}
```

Output:
```
Main: Starting multiple threads...
Thread 1: Starting operation
Thread 2: Starting operation
Thread 3: Starting operation
Thread 1: Operation completed
Thread 2: Operation completed
Thread 3: Operation completed
Main: All threads are done!
```

In this example, three threads (`thread1`, `thread2`, and `thread3`) are created and started concurrently. Each thread performs a simple operation (simulated by `Thread.Sleep(2000)`), and the main thread waits for all the threads to complete using the `Join` method. This demonstrates the concurrent execution of multiple threads.

### Thread.join()

The `thread1.Join()` method is used to make sure that the calling thread (in this case, the main thread) waits for `thread1` to complete before moving on to the next instruction. It's a way to synchronize the threads.

In the provided example:

```csharp
// Start the threads
thread1.Start();
thread2.Start();
thread3.Start();

// Wait for all threads to complete before continuing
thread1.Join();
thread2.Join();
thread3.Join();
```

After starting `thread1`, `thread2`, and `thread3`, the `Join()` method is called on each thread. This ensures that the main thread will pause its execution until each respective thread (`thread1`, `thread2`, and `thread3`) has completed its operation.

Without the `Join()` calls, the main thread might continue its execution even if the other threads are still running, potentially leading to unpredictable behavior. By using `Join()`, you ensure that the main thread waits for the completion of each individual thread before moving on, providing a controlled and synchronized execution flow.

---

## Task

- Derived from Task Parallel Library

```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        // Creating and starting multiple tasks
        Task task1 = Task.Run(() => PerformOperation("Task 1"));
        Task task2 = Task.Run(() => PerformOperation("Task 2"));
        Task task3 = Task.Run(() => PerformOperation("Task 3"));

        Console.WriteLine("Main: Starting multiple tasks...");

        // Wait for all tasks to complete before continuing
        await Task.WhenAll(task1, task2, task3);

        Console.WriteLine("Main: All tasks are done!");
    }

    static void PerformOperation(string taskName)
    {
        Console.WriteLine($"{taskName}: Starting operation");
        Task.Delay(2000).Wait(); // Simulate some work asynchronously
        Console.WriteLine($"{taskName}: Operation completed");
    }
}
```

Output:
```
Main: Starting multiple tasks...
Task 1: Starting operation
Task 2: Starting operation
Task 3: Starting operation
Task 1: Operation completed
Task 2: Operation completed
Task 3: Operation completed
Main: All tasks are done!
```

In this example:
- `Task.Run` is used to create and start tasks asynchronously.
- `Task.WhenAll` is used to wait for all tasks to complete before continuing the execution of the main method.
- The `PerformOperation` method simulates an asynchronous operation using `Task.Delay(2000)`.

This example demonstrates the concurrent execution of multiple tasks using the `Task` class. Note that the `await Task.WhenAll` statement replaces the need for explicit `Join` calls in the `Thread` example.
### Equivalent of `Join` using task
In the context of `Task`, the equivalent of `Join` is to use the `await` keyword along with `Task.WhenAll` or `Task.WhenAny` depending on the specific synchronization requirement.

Here's an example using `Task.WhenAll`:

```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        // Creating and starting multiple tasks
        Task task1 = Task.Run(() => PerformOperation("Task 1"));
        Task task2 = Task.Run(() => PerformOperation("Task 2"));
        Task task3 = Task.Run(() => PerformOperation("Task 3"));

        Console.WriteLine("Main: Starting multiple tasks...");

        // Use Task.WhenAll to wait for all tasks to complete
        await Task.WhenAll(task1, task2, task3);

        Console.WriteLine("Main: All tasks are done!");
    }

    static void PerformOperation(string taskName)
    {
        Console.WriteLine($"{taskName}: Starting operation");
        Task.Delay(2000).Wait(); // Simulate some work asynchronously
        Console.WriteLine($"{taskName}: Operation completed");
    }
}
```

In this example, the `await Task.WhenAll(task1, task2, task3)` statement ensures that the main method will wait for all three tasks to complete before moving on. This is similar to the `Join` method used with threads.

If you want to await any of the tasks, you can use `Task.WhenAny`:

```csharp
// Use Task.WhenAny to wait for any of the tasks to complete
Task completedTask = await Task.WhenAny(task1, task2, task3);
Console.WriteLine($"Main: One task completed: {completedTask.Id}");
```

This will allow the main method to continue as soon as any of the tasks is completed.

---
## Generics

- Allow you to write code that can work with `different data types` while maintaining `type safety`
- allows for code reusability for different data types
- add <T> to: classes, methods, fields, etc.
    - you can add <T> or <anything>.


### Generic Example:

```csharp
using System;

namespace MyFirstProgram
{
    class Program
    {
        static void Main(string[] args)
        {
            
            int[] intArray = { 1, 2, 3 };
            double[] doubleArray = { 1.0, 2.0, 3.0 };
            String[] stringArray = { "1", "2", "3" };

            displayElements(intArray);
            displayElements(doubleArray);
            displayElements(stringArray);

            Console.ReadKey();
        }     
        public static void displayElements<Thing>(Thing[] array)
        {
            foreach (Thing item in array)
            {
                Console.Write(item + " ");
            }
            Console.WriteLine();
        }
    }
}
```


### Generic Methods:

```csharp
public class Utility
{
    public static void Swap<T>(ref T a, ref T b)
    {
        T temp = a;
        a = b;
        b = temp;
    }
}

class Program
{
    static void Main()
    {
        int x = 5, y = 10;
        Console.WriteLine($"Before Swap: x = {x}, y = {y}");

        Utility.Swap(ref x, ref y);

        Console.WriteLine($"After Swap: x = {x}, y = {y}");
    }
}
```

In this example, the `Swap` method is generic, allowing it to swap values of different types.

### Calculator example using Generics

```csharp
using System;

public class Calculator<T>
{
    public T Add(T a, T b)
    {
        dynamic operand1 = a;
        dynamic operand2 = b;
        return operand1 + operand2;
    }

    public T Subtract(T a, T b)
    {
        dynamic operand1 = a;
        dynamic operand2 = b;
        return operand1 - operand2;
    }

    public T Multiply(T a, T b)
    {
        dynamic operand1 = a;
        dynamic operand2 = b;
        return operand1 * operand2;
    }

    public T Divide(T a, T b)
    {
        dynamic operand1 = a;
        dynamic operand2 = b;

        if (operand2 == 0)
        {
            throw new ArgumentException("Cannot divide by zero.");
        }

        return operand1 / operand2;
    }
}

class Program
{
    static void Main()
    {
        // For integer values
        Calculator<int> intCalculator = new Calculator<int>();

        Console.WriteLine($"Addition: {intCalculator.Add(5, 3)}");
        Console.WriteLine($"Subtraction: {intCalculator.Subtract(8, 4)}");
        Console.WriteLine($"Multiplication: {intCalculator.Multiply(6, 2)}");
        Console.WriteLine($"Division: {intCalculator.Divide(10, 2)}");
        
        // For double values
        Calculator<double> doubleCalculator = new Calculator<double>();

        Console.WriteLine($"Addition: {doubleCalculator.Add(5.5, 3.2)}");
        Console.WriteLine($"Subtraction: {doubleCalculator.Subtract(8.6, 4.2)}");
        Console.WriteLine($"Multiplication: {doubleCalculator.Multiply(6.0, 2.5)}");
        Console.WriteLine($"Division: {doubleCalculator.Divide(10.0, 2.0)}");
    }
}
```

In this example, the `Calculator` class is generic, and it can perform basic arithmetic operations (addition, subtraction, multiplication, and division) on different numeric types (`int` and `double` in this case). The `dynamic` keyword is used to handle different numeric types, and the calculator is instantiated for both `int` and `double`. You can easily extend it to support other numeric types as needed.

---
## string and String

In C#, `string` and `String` are essentially the same. `string` is an alias for `System.String`, and both can be used interchangeably. It's simply a matter of coding style and preference.

```csharp
string str1 = "Hello";   // using 'string' keyword
String str2 = "World";   // using 'String' class
```

Both `string` and `String` refer to the same class in the .NET Framework, which represents a sequence of characters. In practice, most C# developers prefer using `string` for consistency and brevity.

```csharp
string fullName = "John Doe";
```

It's important to note that while `string` is the C# keyword, `String` is the class in the .NET Framework. The C# compiler automatically maps `string` to `String` during compilation.

---

## Auto implemented properties

- shortcut when no additional logic is required in the property
- you only have to write get; and/or set; inside the property
  
```csharp
public class Person
{
    // Auto-implemented property
    public string FirstName { get; set; }

    // Auto-implemented property with private set
    public string LastName { get; private set; }

    // Auto-implemented property with initial value
    public int Age { get; set; } = 25;
}

class Program
{
    static void Main()
    {
        Person person = new Person();

        // Setting values using auto-implemented properties
        person.FirstName = "John";
        person.LastName = "Doe";  // Note: private set is allowed within the class
        // Age has an initial value of 25

        // Accessing values using auto-implemented properties
        Console.WriteLine($"Full Name: {person.FirstName} {person.LastName}");
        Console.WriteLine($"Age: {person.Age}");
    }
}
```

In this example:
- `FirstName` is an auto-implemented property with both a public get and a public set.
- `LastName` is an auto-implemented property with a public get and a private set. This allows setting the value within the class but not from external code.
- `Age` is an auto-implemented property with an initial value of 25.

Auto-implemented properties are convenient for scenarios where the `property doesn't require additional logic in the getter or setter`. If additional logic is needed, you can switch to a standard property with an explicit backing field.

---

## ToString()

-  converts an object to its string representation so that it is suitable for display

```c#
using System;

namespace MyFirstProgram
{
    class Program 
    {
        static void Main(string[] args)
        {

           
            Car car = new Car("Chevy", "Corvette", 2022, "blue");

            Console.WriteLine(car.ToString());

            Console.ReadKey();
        }
    }
    class Car
    {
        String make;
        String model;
        int year;
        String color;

        public Car(String make, String model, int year, String color)
        {
            this.make = make;
            this.model = model;
            this.year = year;
            this.color = color;
        }
        // Override base class method and add our own implementation.
        public override string ToString()
        {       
            return "This is a " + make + " " + model;
        }
    }
}
```
---

## Extension methods

Extension methods in C# `allow you to add new methods to existing types without modifying them`. They are a powerful feature that enhances the capabilities of types from external libraries or even the core framework. Extension methods are defined in static classes and must be in the same namespace as they are used or imported through a `using` directive.

Here's a basic example of an extension method:

```csharp
using System;

public static class StringExtensions
{
    // Extension method for strings: Capitalize the first letter
    public static string CapitalizeFirstLetter(this string input)
    {
        if (string.IsNullOrEmpty(input))
        {
            return input;
        }

        return char.ToUpper(input[0]) + input.Substring(1);
    }
}

class Program
{
    static void Main()
    {
        string text = "hello world";

        // Using the extension method
        string result = text.CapitalizeFirstLetter();

        Console.WriteLine(result);  // Outputs: Hello world
    }
}
```

In this example:

- `StringExtensions` is a static class containing an extension method `CapitalizeFirstLetter`.
- The extension method operates on a `string` (`this string input`), and it capitalizes the first letter of the string.
- The `this` keyword in the parameter indicates that this method is an extension method for the `string` type.

---

## 