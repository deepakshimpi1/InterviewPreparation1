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

## Action vs Func

In C#, `Action` and `Func` are both delegate types, but they are used for different purposes.

### Action:

- **Purpose:** Represents a method that performs an action but does not return a value.
- **Declaration:** `Action<T1, T2, ..., T16>`
- **Example:**
  ```csharp
  Action<string, int> printDetails = (name, age) =>
  {
      Console.WriteLine($"Name: {name}, Age: {age}");
  };
  ```
- **Use Case:** Suitable for scenarios where you want to perform an action, like event handlers, callbacks, or methods that don't return a value.

### Func:

- **Purpose:** Represents a method that takes parameters and returns a value.
- **Declaration:** `Func<T1, T2, ..., TResult>`
- **Example:**
  ```csharp
  Func<int, int, int> add = (a, b) => a + b;
  ```
- **Use Case:** Suitable for scenarios where you want to perform a computation and return a value, like LINQ projections, calculations, or any scenario where you need to produce a result.

### Example Combining Action and Func:

```csharp
using System;

class Program
{
    static void Main()
    {
        // Action: Perform an action (print)
        Action<string> printMessage = message =>
        {
            Console.WriteLine($"Message: {message}");
        };

        // Func: Calculate the length of a string and return the result
        Func<string, int> stringLength = s => s.Length;

        // Using Action and Func together
        PrintAndReturnLength("Hello, World!", printMessage, stringLength);
    }

    static void PrintAndReturnLength(string input, Action<string> printAction, Func<string, int> lengthFunction)
    {
        // Perform the action
        printAction(input);

        // Calculate the length using the function and print the result
        int length = lengthFunction(input);
        Console.WriteLine($"Length: {length}");
    }
}
```

In this example, `Action` is used to print a message, and `Func` is used to calculate the length of a string. They are then combined in the `PrintAndReturnLength` method to showcase how both can be used together in a scenario.

---

## Delegate vs Multicast Delegate

In C#, a delegate is a type that represents references to methods(pointer to a function). Delegates in C# provide a way to encapsulate a method, and they can be used to create type-safe function pointers. A multicast delegate is a type of delegate that can hold references to multiple methods, allowing them to be invoked sequentially.

### Delegate:

- **Definition:** A delegate is a reference type that can represent a method signature. It allows you to pass methods as parameters or store them as fields or properties.

- **Declaration:**
  ```csharp
  delegate void MyDelegate(int x);
  ```

- **Example:**
  ```csharp
  class Program
  {
      static void Main()
      {
          MyDelegate myDelegate = MyMethod;
          myDelegate(42);
      }

      static void MyMethod(int value)
      {
          Console.WriteLine($"MyMethod: {value}");
      }
  }
  ```

### Multicast Delegate:

- **Definition:** A multicast delegate is a delegate that holds references to multiple methods. When a multicast delegate is invoked, all the methods it references are called sequentially.

- **Declaration:**
  ```csharp
  delegate void MyMulticastDelegate(int x);
  ```

- **Example:**
  ```csharp
  class Program
  {
      static void Main()
      {
          MyMulticastDelegate multicastDelegate = Method1;
          multicastDelegate += Method2;

          // Invoking the multicast delegate calls both Method1 and Method2
          multicastDelegate(42);
      }

      static void Method1(int value)
      {
          Console.WriteLine($"Method1: {value}");
      }

      static void Method2(int value)
      {
          Console.WriteLine($"Method2: {value}");
      }
  }
  ```

In the example above, `multicastDelegate` is a multicast delegate that holds references to both `Method1` and `Method2`. When the delegate is invoked (`multicastDelegate(42)`), both `Method1` and `Method2` are called in the order they were added to the delegate.

Multicast delegates are particularly useful in scenarios where you want to combine and execute multiple methods sequentially. They are often used in event handling and callback scenarios.

---

## Array vs ArrayList

Arrays and ArrayLists are both used for storing collections of elements in C#, but they have key differences in terms of flexibility, type safety, and performance. Here's a comparison between arrays and ArrayLists in C#:

1. **Type Safety:**
   - **Array:** Elements must be of the same data type. Arrays are strongly typed.
   - **ArrayList:** Elements can be of different data types. ArrayList is not strongly typed.

2. **Size:**
   - **Array:** Fixed-size. The size is specified at the time of declaration and cannot be changed.
   - **ArrayList:** Dynamically resizable. It can grow or shrink as elements are added or removed.

3. **Performance:**
   - **Array:** Generally faster for direct element access because elements are stored in contiguous memory locations.
   - **ArrayList:** May have slightly slower performance due to the need for boxing and unboxing (converting between value types and reference types) and dynamic resizing.

4. **Initialization:**
   - **Array:** Initialized with a fixed size.
     ```csharp
     int[] integerArray = new int[5];
     ```
   - **ArrayList:** Can be initialized empty or with an existing collection.
     ```csharp
     ArrayList arrayList = new ArrayList();
     ```
  
6. **Methods and Features:**
   - **Array:** Limited set of methods. Primarily supports direct access by index.
   - **ArrayList:** Provides a richer set of methods such as `Add`, `Remove`, `Contains`, `IndexOf`, etc.

7. **Compile-Time vs. Runtime Checks:**
   - **Array:** Type checks are performed at compile time.
   - **ArrayList:** Type checks are performed at runtime, which can lead to runtime errors if type mismatches occur.

Here's a simple example illustrating the differences:

```csharp
// Array
int[] integerArray = new int[5];

// ArrayList
ArrayList arrayList = new ArrayList();
arrayList.Add(1);
arrayList.Add("string"); // Allowed, but not type-safe
```

---

## Array vs List

Arrays and lists are both used to store collections of elements in C#, but they have differences in terms of flexibility, performance, and functionality. Here's a comparison between arrays and lists in C#:

1. **Size:**
   - **Array:** Fixed-size. The size is specified at the time of declaration, and it cannot be changed.
   - **List\<T\>:** Dynamically resizable. Lists can grow or shrink as elements are added or removed.

2. **Type:**
   - **Array:** Elements must be of the same data type.
   - **List\<T\>:** Strongly typed. Elements must be of the specified type (`T`).

3. **Performance:**
   - **Array:** Generally faster for direct element access due to contiguous memory locations.
   - **List\<T\>:** May have slightly slower performance due to dynamic resizing. However, the difference is often negligible in many scenarios.

4. **Initialization:**
   - **Array:** Initialized with a fixed size.
     ```csharp
     int[] integerArray = new int[5];
     ```
   - **List\<T\>:** Can be initialized empty or with an existing collection.
     ```csharp
     List<int> integerList = new List<int>();
     ```

5. **Flexibility:**
   - **Array:** Fixed size; cannot be resized.
   - **List\<T\>:** Dynamic size; can grow or shrink as needed.

6. **Methods and Features:**
   - **Array:** Limited set of methods and features. Primarily supports direct access by index.
   - **List\<T\>:** Offers a rich set of methods and features such as `Add`, `Remove`, `Contains`, `IndexOf`, etc.

7. **Use Cases:**
   - **Array:** Suitable for situations where a fixed-size collection is sufficient and direct element access is crucial.
   - **List\<T\>:** Preferred for dynamic collections that need to grow or shrink. Provides more functionality and is often used in modern C# development.

Here's a simple example illustrating the differences:

```csharp
// Array
int[] integerArray = new int[5];

// List<T>
List<int> integerList = new List<int>();
integerList.Add(1);
integerList.Add(2);
integerList.Add(3);
```

In summary, arrays are suitable for fixed-size collections with homogeneous elements and direct access needs, while lists provide dynamic resizing, type safety, and additional functionality, making them more versatile for various scenarios. In modern C# development, lists (or other generic collections) are often preferred for their flexibility and ease of use.

---

## ArrayList vs List

`ArrayList` and `List<T>` are both used for storing collections of elements in C#, but they have significant differences, especially in terms of type safety and performance. Here's a comparison between `ArrayList` and `List<T>`:

1. **Type Safety:**
   - **`ArrayList`:** Not strongly typed. It can store elements of any data type, leading to potential runtime errors due to type mismatches.
   - **`List<T>`:** Strongly typed. The type is specified during declaration, and it provides compile-time type safety. It only accepts elements of the specified type `T`.

2. **Performance:**
   - **`ArrayList`:** May have performance overhead due to boxing and unboxing (converting between value types and reference types) and type checks at runtime.
   - **`List<T>`:** Generally more performant than `ArrayList` because it avoids boxing and unboxing, and type checks are performed at compile time.

3. **Initialization:**
   - **`ArrayList`:** Can be initialized empty or with an existing collection.
     ```csharp
     ArrayList arrayList = new ArrayList();
     ```
   - **`List<T>`:** Can be initialized empty or with an existing collection.
     ```csharp
     List<int> integerList = new List<int>();
     ```

4. **Type of Elements:**
   - **`ArrayList`:** Can store elements of any type.
   - **`List<T>`:** Only accepts elements of the specified type `T`. Provides type safety.

5. **Methods and Features:**
   - **`ArrayList`:** Limited set of methods. Requires casting when retrieving elements.
   - **`List<T>`:** Provides a richer set of methods and features. Elements can be accessed without casting.

6. **Compile-Time vs. Runtime Checks:**
   - **`ArrayList`:** Type checks are performed at runtime, which can lead to runtime errors if type mismatches occur.
   - **`List<T>`:** Type checks are performed at compile time, providing better safety.

7. **Performance Considerations:**
   - If working with value types or a specific type, `List<T>` is generally preferred for better performance and type safety.
   - `ArrayList` might be used in scenarios where dealing with different types is necessary, but its usage is less common in modern C# development.

Here's a simple example illustrating the differences:

```csharp
// ArrayList
ArrayList arrayList = new ArrayList();
arrayList.Add(1);
arrayList.Add("string"); // Allowed, but not type-safe

// List<T>
List<int> integerList = new List<int>();
integerList.Add(1);
// integerList.Add("string"); // Not allowed due to type safety
```

In modern C# development, `List<T>` is commonly preferred over `ArrayList` due to its type safety and better performance.

---

## `List` and `Dictionary`

| Feature                   | `List`                          | `Dictionary<TKey, TValue>`            |
|---------------------------|---------------------------------|--------------------------------------|
| **Purpose**               | Ordered collection of elements   | Key-value pairs                      |
| **Data Structure**        | Dynamic array                   | Hash table                           |
| **Accessing Elements**    | By index (integer values)        | By key (unique identifier)           |
| **Ordering**              | Maintains order of elements      | No specific order                    |
| **Duplicate Keys/Values** | Duplicates allowed               | Keys must be unique                  |
| **Performance**           | Good for element access by index | Efficient for lookups based on keys  |
| **Initialization**        | Can be initialized with or without initial elements | Can be initialized with or without initial key-value pairs |
| **Type Safety**           | Strongly typed                   | Strongly typed (for keys and values) |
| **Example**               | ```csharp List<int> integerList = new List<int>(); integerList.Add(1); ``` | ```csharp Dictionary<string, int> keyValuePairs = new Dictionary<string, int>(); keyValuePairs.Add("one", 1); ``` |

 
---

## `HashSet` and `List` in tabular form:

| Feature                   | `HashSet<T>`                                 | `List<T>`                                    |
|---------------------------|-----------------------------------------------|----------------------------------------------|
| **Purpose**               | Stores a collection of unique elements        | Stores an ordered collection of elements     |
| **Data Structure**        | Uses a hash table internally                  | Implements a dynamic array                  |
| **Type of Elements**      | Unique elements (No duplicates allowed)      | Duplicates allowed                          |
| **Accessing Elements**    | No direct index-based access                  | Direct index-based access                   |
| **Ordering**              | No specific order                             | Maintains order of elements                  |
| **Performance**           | Efficient for checking uniqueness             | Good for element access by index            |
| **Initialization**        | Can be initialized with or without initial elements | Can be initialized with or without initial elements |
| **Type Safety**           | Strongly typed                                | Strongly typed                               |
| **Example**               | ```csharp HashSet<int> integerSet = new HashSet<int>(); integerSet.Add(1); ``` | ```csharp List<int> integerList = new List<int>(); integerList.Add(1); ``` |

---

## `HashSet` and `Dictionary<TKey, TValue>` 

| Feature                   | `HashSet<T>`                                 | `Dictionary<TKey, TValue>`                    |
|---------------------------|-----------------------------------------------|----------------------------------------------|
| **Purpose**               | Stores a collection of unique elements        | Stores key-value pairs                       |
| **Data Structure**        | Uses a hash table internally                  | Uses a hash table internally                 |
| **Type of Elements**      | Unique elements (No duplicates allowed)      | Keys must be unique; values can have duplicates |
| **Accessing Elements**    | No direct access by key                       | Access by key (Each key is associated with a value) |
| **Ordering**              | No specific order                             | No specific order (Keys are not guaranteed to be in any order) |
| **Performance**           | Efficient for checking uniqueness             | Efficient for lookups based on keys         |
| **Initialization**        | Can be initialized with or without initial elements | Can be initialized with or without initial key-value pairs |
| **Type Safety**           | Strongly typed                                | Strongly typed (for keys and values)        |
| **Example**               | ```csharp HashSet<int> integerSet = new HashSet<int>(); integerSet.Add(1); ``` | ```csharp Dictionary<string, int> keyValuePairs = new Dictionary<string, int>(); keyValuePairs.Add("one", 1); ``` |

Choose between `HashSet` and `Dictionary<TKey, TValue>` based on whether you need to store a collection of unique elements (use `HashSet`) or key-value pairs (use `Dictionary`). Use `HashSet` when uniqueness is the primary concern, and use `Dictionary` when you need to associate unique keys with corresponding values.

---
## `Hashtable` and `Dictionary<TKey, TValue>` 

| Feature                   | `Hashtable`                                   | `Dictionary<TKey, TValue>`                    |
|---------------------------|-----------------------------------------------|----------------------------------------------|
| **Purpose**               | Stores key-value pairs                       | Stores key-value pairs                       |
| **Data Structure**        | Uses a hash table internally                  | Uses a hash table internally                 |
| **Type of Keys/Values**    | Keys and values can be of any data type       | Keys and values must be of specified types (`TKey`, `TValue`) |
| **Accessing Elements**    | Access by key (Each key is associated with a value) | Access by key (Each key is associated with a value) |
| **Ordering**              | No specific order (Keys are not guaranteed to be in any order) | No specific order (Keys are not guaranteed to be in any order) |
| **Performance**           | May have performance overhead due to boxing/unboxing | Efficient for lookups based on keys         |
| **Initialization**        | Can be initialized with or without initial key-value pairs | Can be initialized with or without initial key-value pairs |
| **Type Safety**           | Not strongly typed (keys and values can be of any type) | Strongly typed (for keys and values)        |
| **Example**               | ```csharp Hashtable keyValuePairs = new Hashtable(); keyValuePairs.Add("one", 1); ``` | ```csharp Dictionary<string, int> keyValuePairs = new Dictionary<string, int>(); keyValuePairs.Add("one", 1); ``` |

It's worth noting that `Dictionary<TKey, TValue>` is part of the generic collection classes introduced in .NET Framework 2.0, offering type safety and better performance compared to `Hashtable`. In modern C# development, it is generally recommended to use `Dictionary<TKey, TValue>` over `Hashtable` for key-value pair scenarios due to these advantages.