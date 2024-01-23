## Enitity Framework

- An Entity Framework (EF) is an open-source ORM (Object-Relational Mapper) from Microsoft. 
- It allows developers to work with .NET applications and other domain-specific objects
- Entity framework does not focus on the original database columns and tables used to store data.
- It automatically creates codes for the data access layers, intermediate layers, and mapping codes. 
- This eventually helps developers cut down the development of work and time.

## Why should we use the Entity Framework?

- writing and managing ADO.NET codes is a problematic task. 
- Therefore, Microsoft introduced the `Entity Framework` to make this tedious task more manageable. 
- Entity Framework reduces a significant amount of code-based tasks by providing relational data in the form of domain-specific objects.

## What is ADO.NET EF?

- ADO.NET Entity Framework is an ORM framework 
- Allows us to work with different relational databases, such as Oracle, MYSQL, SQL Server, DB2, etc. 
- It enables us to work with the data either as objects or entities.

## Main components of the Entity Framework Architecture.

- `Entity Data Model (EDM):` It consists of the three parts, such as storage model, conceptual model, and mapping.
- `Entity SQL:` It is an alternate query language used in Entity Frameworks.
- `LINQ to Entities (L2E):` It is a query language that helps write queries against the object, which further helps to retrieve the entities based on the definitions specifies in the conceptual model.
- `Entity Client Data Provider:` It is defined as the layer that helps convert the L2E queries to SQL queries to be easily understood by the database. Additionally, it can interact with the ADO.NET data provider to transfer or retrieve data from different databases.
- `Net Data Provider:` It is another layer that helps interact with the database by using standard ADO.NET.
- `Object Service:` It is an entry point into the database used to access and send back the data when needed. It helps convert the data coming from an entity client data provider into an entity object structure.

## Primary functions of the Entity Framework?

- Map domain classes to the database schema.
- It helps execute LINQ queries to SQL.
- It keeps tracks of changes in the entities.

## What are the processes used to load related entities in the Entity Framework?

`Lazy Loading:` 
- This process delays the loading of related objects until there is a requirement of them. 
- Lazy loading only returns objects needed by the user, and all other related objects are only returned when required in the process.

`Eager Loading:` 
- This process mainly takes place when we query for an object. 
- Eager loading returns all the related objects. Additionally, all the related objects are automatically loaded with the parent object.

## What are the different types of inheritance supported in Entity Framework?

- Table per Hierarchy (TPH)
- Table per Type (TPT)
- Table per Concrete Class (TPC)

## Primary parts of the Entity Data Model?

There are mainly three parts of the Entity Data Model, such as:
- Storage Model
- Conceptual Model
- Mapping

## What is meant by a model in context to Entity Framework?

- A model is nothing but a class mainly used to represent the data.
- In context to EF, a model represents the data from a table inside the existing database.

Example: The following codes display the basic customer model:
```EF
public class Customer  
{  
  public int ID { get; set; }  
  public string Name { get; set; }  
  public DateTime JoinDate { get; set; }  
}  
```

## LINQ vs EF

Linq:
- It only operates with the help of the SQL Server Database.	
- It supports one to one mapping between entity classes and the relational tables.
- It maintains a relation by creating a .dbml file.
- It does not support complex types.
- It cannot generate a database by using the model.
- It enables users to query the data with DataContext.
- It contains a tightly coupled mechanism.	
- It is mainly used for faster application developments with SQL Server.

EF:
- It has various databases, such as SQL Server, MYSQL, Oracle, DB2, etc.
- It supports one to one, one to many, and many to many mapping types between the entity classes and the relational tables.
- It first creates the .edmx file. After that, it maintains a relation using three types of files: .ssdl, .msl and .csdl.
- It supports complex types.
- It can generate a database using the model.
- It allows users to query the data with DbContext, ObjectContext, and EntitySQL.
- It contains a loosely coupled mechanism.
- It is primarily used for faster application developments using SQL Server and other databases like MYSQL, Oracle, DB2, etc.

## Conceptual Model?
- The conceptual model is usually defined as the model class that consists of relationships. 
- This type of model remains independent of the database structure.

## Storage Model?
- The storage model is usually explained as the database design model that consists of database tables, stored procs, views, and keys with relationships.

---

## Entity Framework?
- Entity Framework is an ORM framework. 
- ORM stands for Object Relational Mapping.
  - Object Relational Mapping framework automatically creates classes based on database tables, and the vice versa is also true, thatis, it can also automatically generate necessary SQL to create database tables based on classes.
- EF supports various database providers, including SQL Server, MySQL, PostgreSQL, and SQLite, among others.
-  EF supports lazy loading, where related data is loaded from the database only when accessed, and eager loading, where related data is retrieved along with the main data.
- EF supports Language-Integrated Query (LINQ), which enables developers to write queries using C# syntax directly in their code. This provides a type-safe

## Database First Approach:

- Database-First approach is often chosen when working with existing databases, 
- especially in scenarios where the database schema is already well-established.
- It allows developers to leverage existing databases without having to manually create data models or write extensive code for database interactions.
 

### Example Scenario:
Imagine you have an existing database with a table called `Students`. The table has columns such as `StudentID`, `FirstName`, `LastName`, and `Age`. We'll use Entity Framework to generate a data model and interact with this database.

### Steps:

1. **Create an Existing Database:**
   - Assume you have a database named `SchoolDB` with a table named `Students`.

2. **Generate Entity Data Model:**
   - Use Entity Framework tools, such as the Entity Data Model Wizard in Visual Studio, to generate the data model based on the existing database.

3. **Generated Entity Classes:**
   - Entity Framework generates entity classes based on the `Students` table. These classes might look like the following:

   ```csharp
   public class Student
   {
       public int StudentID { get; set; }
       public string FirstName { get; set; }
       public string LastName { get; set; }
       public int Age { get; set; }
   }
   ```

4. **Generated Context Class:**
   - Entity Framework also generates a context class responsible for managing the interactions with the database. It might look like this:

   ```csharp
   public class SchoolDbContext : DbContext
   {
       public DbSet<Student> Students { get; set; }
   }
   ```

5. **Use Generated Code in Application:**
   - Incorporate the generated entity classes and context class into your application code:

   ```csharp
   using (var context = new SchoolDbContext())
   {
       // Querying data
       var students = context.Students.ToList();

       // Adding a new student
       var newStudent = new Student
       {
           FirstName = "John",
           LastName = "Doe",
           Age = 20
       };
       context.Students.Add(newStudent);
       context.SaveChanges();

       // Updating a student
       var existingStudent = context.Students.FirstOrDefault(s => s.FirstName == "John");
       if (existingStudent != null)
       {
           existingStudent.Age = 21;
           context.SaveChanges();
       }

       // Deleting a student
       var studentToDelete = context.Students.FirstOrDefault(s => s.LastName == "Doe");
       if (studentToDelete != null)
       {
           context.Students.Remove(studentToDelete);
           context.SaveChanges();
       }
   }
   ```

6. **Update Model as Needed:**
   - If changes are made to the database schema, you can use Entity Framework tools to update the Entity Data Model, ensuring that your data model remains in sync with the database.

This example demonstrates how the Database-First approach allows you to work with an existing database by generating the necessary code for data access based on the database schema.

---

## DbContext vs DbSet

`DbContext` and `DbSet` are both key components of Entity Framework, but they serve different purposes in the context of database interaction.

### DbContext:

- **Definition:**
  - `DbContext` is a class in Entity Framework that represents a session with the database. It is responsible for managing database connections, transaction management, and providing a set of DbSet properties.

- **Functionality:**
  - Manages the interaction between the application and the database.
  - Acts as a bridge between the application's domain entities and the database.
  - Maintains information about the database connection, the model, and provides methods for querying, updating, and persisting data.

- **Example:**
  ```csharp
  public class SchoolDbContext : DbContext
  {
      public DbSet<Student> Students { get; set; }
      public DbSet<Course> Courses { get; set; }

      // Other configuration and methods...
  }
  ```

- **Usage:**
  - Create an instance of the `DbContext` to interact with the database.
  - Provides access to multiple DbSet properties, each representing a collection of entities corresponding to a database table.

### DbSet:

- **Definition:**
  - `DbSet` is a class within Entity Framework that represents a collection of entities for a specific database table. It is a property of the `DbContext` class.

- **Functionality:**
  - Represents a set of entities (rows) for a specific table in the database.
  - Provides methods for querying, adding, updating, and deleting entities within that table.

- **Example:**
  ```csharp
  public class SchoolDbContext : DbContext
  {
      public DbSet<Student> Students { get; set; }
      public DbSet<Course> Courses { get; set; }

      // Other configuration and methods...
  }
  ```

- **Usage:**
  - Used within the `DbContext` to perform CRUD operations on entities within a specific database table.
  - Enables LINQ queries and other operations on a specific entity type.

### Relationship:

- **Connection:**
  - `DbContext` contains one or more `DbSet` properties, each corresponding to a database table.
  - `DbSet` is part of the `DbContext` and represents a specific collection of entities.

- **Hierarchy:**
  - `DbContext` is higher in the hierarchy and encapsulates the entire database context, managing all aspects of database interaction.
  - `DbSet` is a property within the `DbContext` and represents a specific set of entities within the context.

In summary, `DbContext` is the main class responsible for managing the interaction with the database, while `DbSet` is a property of `DbContext` representing a collection of entities for a specific database table. They work together to provide a high-level abstraction for database operations in Entity Framework.


### Transaction with DbContext
- Be cautious about sharing a DbContext instance across multiple threads. It's generally recommended to use a `separate` DbContext instance for each thread or scope.

```c#
public class YourService
{
    private readonly YourDbContext _context;

    public YourService(YourDbContext context)
    {
        _context = context;
    }

    public void PerformTransaction()
    {
        using (var transaction = _context.Database.BeginTransaction())
        {
            try
            {
                // Perform operations within the transaction using '_context'
                _context.SaveChanges(); // Commit changes if successful

                transaction.Commit();
            }
            catch (Exception)
            {
                transaction.Rollback();
                // Handle exception or rethrow if needed
            }
        }
    }
}

```
---

## Model First Approach

- We first create an entity model, the data model is designed visually using a `graphical designer`.
- And the corresponding database schema is generated from that model.
- Ca use `edmx` Entity Data Model XML to design model. 
- It provides a high-level abstraction that allows developers to focus on the structure of the data model without dealing with the database schema creation manually.

---
## Code First Approach

- Code first approach allows us to create our custom classes first 
- and based on those EF can generate database automatically for us.
- `data model` is defined using `code`, and the database schema is generated or updated based on the code.
- Here we add model classes then DbContext class and also operations using repository pattern.

### Pros:
- Code-First provides more control over the database schema through code. Developers can define entities, relationships, and constraints directly in code.
- Code-First integrates well with version control systems. Developers can manage changes to the data model alongside application code changes.
- Entity classes can be reused in different projects, making it easier to share data models across applications.
---

## Repository Pattern
  
### Introduction:

- The Repository Pattern is a design pattern commonly used in software development, particularly in the context of data access. 
- Its main purpose is to provide an `abstraction layer between the application's business logic and the data access logic`, offering a clean and modular way to interact with a data storage system, such as a database."

### Key Components:

1. **Entity:**
   - Explain that an entity represents a domain object or a data entity within the application. It's a representation of a real-world concept in the system.

2. **Repository Interface:**
   - Describe the repository interface as the contract that defines the methods for data access related to a specific entity. It outlines the operations that the repository will provide, such as GetById, GetAll, Add, Update, and Delete.

3. **Repository Implementation:**
   - Explain that the repository implementation is responsible for implementing the methods defined in the repository interface. It contains the actual data access logic, interacting with the underlying data storage (e.g., a database).

### Example:

Provide a simple example to illustrate how the Repository Pattern works. Use a common scenario like managing products:

"As an example, let's consider a scenario where we have a `Product` entity. The `IProductRepository` interface outlines methods like `GetById`, `GetAll`, `Add`, `Update`, and `Delete`. The `ProductRepository` class then implements these methods, handling the actual data access logic, such as querying the database or adding a new product."

### Benefits:

Highlight the advantages of using the Repository Pattern:

1. **Abstraction:**
   - Emphasize how the pattern abstracts away the complexities of data access, providing a clean and well-defined interface for the rest of the application.

2. **Testability:**
   - Mention that the pattern enhances testability by allowing easy mocking of the repository interface during unit testing.

3. **Separation of Concerns:**
   - Explain how it promotes a separation of concerns, isolating the data access logic from the rest of the application's business logic.

4. **Flexibility:**
   - Discuss how the pattern provides flexibility by allowing the underlying data storage system to be switched without affecting the application's business logic.


### Real-World Use Cases:

Provide real-world scenarios where the Repository Pattern is commonly used:

- "In larger applications where managing data access becomes more complex."
- "When working with different data storage systems (e.g., relational databases, NoSQL databases) and needing a consistent interface."
- "In scenarios where version control and tracking changes to the data model are crucial."
---

## Dapper

"Dapper is a micro ORM (Object-Relational Mapping) library for .NET that simplifies the process of interacting with databases, specifically when querying data. Here's why developers often choose to use Dapper for SQL queries:

1. **Performance:**
   - Dapper is known for its `exceptional performance`. It's designed to be lightweight and efficient, resulting in faster data access compared to some larger ORM frameworks. This is particularly crucial in applications where performance is a priority.

2. **Simplicity:**
   - Dapper is remarkably simple to use. It doesn't come with an overwhelming set of features or complex configurations. You write your SQL queries, and Dapper seamlessly maps the results to your object models. The simplicity makes it easy for developers to understand and adopt.

3. **Raw SQL Queries:**
   - Dapper allows developers to `write raw SQL queries`. This gives a level of flexibility and control over the queries.

4. **Automatic Object Mapping:**
   - Dapper `provides automatic object mapping`, which means you can execute a SQL query, and Dapper will intelligently map the result set directly to your object models.

5. **Lightweight Library:**
   - Being a micro ORM, Dapper has a small footprint. It doesn't introduce unnecessary overhead to your application, making it a suitable choice for projects where a lightweight data access solution is preferred.

6. **Compatibility:**
   - Dapper is compatible with various database providers, including SQL Server, MySQL, PostgreSQL, and more. This flexibility allows developers to use Dapper in projects with different database systems.

In summary, Dapper is chosen for its performance, simplicity, and flexibility. It excels in scenarios where raw SQL queries are preferred, and there's a need for a lightweight and efficient data access solution."


```c#
using System;
using System.Collections.Generic;
using System.Data.SqlClient;
using Dapper;

class Program
{
    static void Main()
    {
        // Retrieve the connection string from configuration
        string connectionString = "YourConnectionStringHere";

        // Query the database using Dapper
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            connection.Open();

            // Define the SQL query
            string sqlQuery = "SELECT ProductId, ProductName, Price FROM Products";

            // Execute the query and map results to the Product class
            IEnumerable<Product> products = connection.Query<Product>(sqlQuery);

            // Display the results
            Console.WriteLine("Product List:");
            foreach (var product in products)
            {
                Console.WriteLine($"ProductId: {product.ProductId}, ProductName: {product.ProductName}, Price: {product.Price}");
            }
        }
    }
}
```

---

## EF vs EF Core

- EF core is cross platform and lightweight
- Supports various platforms, including Windows, Linux, and macOS.
- Suitable for applications targeting .NET Core, .NET 5+, and beyond.

### Example using Entity Framework (EF):

```csharp
using System;
using System.Data.Entity;

// Entity class
public class Product
{
    public int ProductId { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}

// DbContext class
public class AppDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }
}

class Program
{
    static void Main()
    {
        using (var context = new AppDbContext())
        {
            // Querying products
            var products = context.Products.ToList();

            // Displaying results
            Console.WriteLine("Products:");
            foreach (var product in products)
            {
                Console.WriteLine($"ProductId: {product.ProductId}, Name: {product.Name}, Price: {product.Price}");
            }
        }
    }
}
```

### Example using Entity Framework Core (EF Core):

```csharp
using System;
using Microsoft.EntityFrameworkCore;

// Entity class
public class Product
{
    public int ProductId { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}

// DbContext class
public class AppDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("YourConnectionStringHere");
    }
}

class Program
{
    static void Main()
    {
        using (var context = new AppDbContext())
        {
            // Querying products
            var products = context.Products.ToList();

            // Displaying results
            Console.WriteLine("Products:");
            foreach (var product in products)
            {
                Console.WriteLine($"ProductId: {product.ProductId}, Name: {product.Name}, Price: {product.Price}");
            }
        }
    }
}
```

### Key Differences:

1. **Namespace and Package:**
   - EF: `using System.Data.Entity;`
   - EF Core: `using Microsoft.EntityFrameworkCore;`

2. **DbContext Configuration:**
   - EF: Configuration often involves settings in the `App.config` or `Web.config` file.
   - EF Core: Uses the `OnConfiguring` method to configure the database connection, and typically relies on dependency injection for additional configurations.

3. **ConnectionString:**
   - EF: Connection string management can be different based on the specific configuration.
   - EF Core: Connection string is often specified in the `OnConfiguring` method.

4. **Platform Support:**
   - EF: Primarily targeted at the full .NET Framework.
   - EF Core: Designed for cross-platform support and is compatible with various .NET implementations, including .NET Core and .NET 5+.

5. **Database Providers:**
   - EF: Supports various database providers.
   - EF Core: Initially had a more limited set of providers, but support has been expanding.

---
