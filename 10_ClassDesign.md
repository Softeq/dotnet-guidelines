# Class Design
## A class or interface should have a single purpose (SDCS-1001) [1]
A class or interface should have a single purpose within the system it functions in. In general, a class either represents a primitive type like an email or ISBN number, an abstraction of some business concept, a plain data structure, or is responsible for orchestrating the interaction between other classes. It is never a combination of those. This rule is widely known as the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle), one of the [S.O.L.I.D](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)). principles.

**Tip:** A class with the word `And` in it is an obvious violation of this rule.

**Tip:** Use Design Patterns to communicate the intent of a class. If you can't assign a single design pattern to a class, chances are that it is doing more than one thing.

**Note:** If you create a class representing a primitive type you can greatly simplify its use by making it immutable.

## An interface should be small and focused (SDCS-1002) [2]
Interfaces should have a name that clearly explains their purpose or role in the system. Do not combine many vaguely related members on the same interface just because they were all on the same class. Separate the members based on the responsibility of those members, so that callers only need to call or implement the interface related to a particular task. This rule is more commonly known as the [Interface Segregation Principle](https://en.wikipedia.org/wiki/Interface_segregation_principle).

## Use an interface rather than a base class to support multiple implementations (STCS-1003) [2]
If you want to expose an extension point from your class, expose it as an interface rather than as a base class. You don't want to force users of that extension point to derive their implementations from a base class that might have an undesired behavior. However, for their convenience you may implement a(n abstract) default implementation that can serve as a starting point.

## Use an interface to decouple classes from each other (SDCS-1004) [2]
Interfaces are a very effective mechanism for decoupling classes from each other:
* They can prevent bidirectional associations;
* They simplify the replacement of one implementation with another;
* They allow the replacement of an expensive external service or resource with a temporary stub for use in a non-production environment.
* They allow the replacement of the actual implementation with a dummy implementation or a fake object in a unit test;
* Using a dependency injection framework you can centralize the choice of which class is used whenever a specific interface is requested.

## Avoid static classes (SDCS-1005) [2]
With the exception of extension method containers, static classes very often lead to badly designed code. They are also very difficult, if not impossible, to test in isolation, unless you're willing to use some very hacky tools.

## Don't hide inherited members with the new keyword (SDCS-1006) [1]
Not only does the new keyword break Polymorphism, one of the most essential object-orientation principles, it also makes sub-classes more difficult to understand. Consider the following two classes:

```csharp
// Base class
public class Book
{
    public virtual void Print()
    {
        Console.WriteLine("Printing Book");
    }
}
```
```csharp
// Inherited class
public class PocketBook : Book
{
    public new void Print()
    {
        Console.WriteLine("Printing PocketBook");
    }
}
```
This will cause behavior that you would not normally expect from class hierarchies:
```csharp
PocketBook pocketBook = new PocketBook();
pocketBook.Print(); // Outputs "Printing PocketBook "
((Book)pocketBook).Print(); // Outputs "Printing Book"
```
It should not make a difference whether you call `Print()` through a reference to the base class or through the derived class.

## It should be possible to treat a derived object as if it were a base class object (SDCS-1007) [1]
In other words, you should be able to use a reference to an object of a derived class wherever a reference to its base class object is used without knowing the specific derived class. A very notorious example of a violation of this rule is throwing a NotSupportedException when overriding some of the base-class methods. A less subtle example is not honoring the behavior expected by the base class.

**Note:** This rule is also known as the Liskov Substitution Principle, one of the [S.O.L.I.D.](http://www.lostechies.com/blogs/chad_myers/archive/2008/03/07/pablo-s-topic-of-the-month-march-solid-principles.aspx) principles.

## Don't refer to derived classes from the base class (SDCS-1008) [1]
Having dependencies from a base class to its sub-classes goes against proper object-oriented design and might prevent other developers from adding new derived classes.

## Avoid exposing the other objects an object depends on (SDCS-1009) [2]
If you find yourself writing code like this then you might be violating the [Law of Demeter](http://en.wikipedia.org/wiki/Law_of_Demeter).
```csharp
someObject.SomeProperty.GetChild().Foo()
```
An object should not expose any other classes it depends on because callers may misuse that exposed property or method to access the object behind it. By doing so, you allow calling code to become coupled to the class you are using, and thereby limiting the chance that you can easily replace it in a future stage.

**Note:** Using a class that is designed using the Fluent Interface pattern seems to violate this rule, but it is simply returning itself so that method chaining is allowed.

**Exception:** Inversion of Control or Dependency Injection frameworks often require you to expose a dependency as a public property. As long as this property is not used for anything other than dependency injection it would not be considered as a violation.

## Avoid bidirectional dependencies (SDCS-1010) [1]
This means that two classes know about each other's public members or rely on each other's internal behavior. Refactoring or replacing one of those classes requires changes on both parties and may involve a lot of unexpected work. The most obvious way of breaking that dependency is to introduce an interface for one of the classes and using Dependency Injection.

**Exception:** Domain models such as defined in [Domain-Driven Design](http://domaindrivendesign.org/) tend to occasionally involve bidirectional associations that model real-life associations. In those cases, make sure they are really necessary, and if they are, keep them in.

## Prefer using classes with both state and behavior (SDCS-1011) [2]
In general, if you find a lot of data-only classes in your code base, you probably also have a few (static) classes with a lot of behavior (see [SDCS-1005](#avoid-static-classes-sdcs-1005-2)). Use the principles of object-orientation explained in this section and move the logic close to the data it applies to.

**Exception:** The only exceptions to this rule are classes that are used to transfer data over a communication channel, also called [Data Transfer Objects](http://martinfowler.com/eaaCatalog/dataTransferObject.html), or a class that wraps several parameters of a method.

## Classes should protect the consistency of their internal state (SDCS-1012) [2]
Validate incoming arguments from public members. For example:
```csharp
public void SetAge(int years)
{
    AssertValueIsInRange(years, 0, 200, nameof(years));
    this.age = years;
}
```

## Avoid importing static members with "using static" (SDCS-1013) [1]
Directive "[using static](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/using-static)" imports accessible static members of a type, so those member could be accessed without specifying type name. Although it makes code a bit more compact - it also makes it much less readable. It makes static members of one type look like members of another type.
```csharp
// GOOD
using System;

public class MyClass
{
   public void PrintSqrt(double value)
   {
       Console.WriteLine(String.Format("Square root: {0}", Math.Sqrt(value)));
   }
}
```
```csharp
// BAD
using static System.Math;
using static System.String;
using static System.Console;

public class MyClass
{
   public void PrintSqrt(double value)
   {
       WriteLine(Format("Square root: {0}", Sqrt(value)));
   }
}
```
