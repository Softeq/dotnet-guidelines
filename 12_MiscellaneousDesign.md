# Miscellaneous
## Throw exceptions rather than returning some kind of status value (SDCS-1201) [2]
A code base that uses return values to report success or failure tends to have nested if-statements sprinkled all over the code. Quite often, a caller forgets to check the return value anyway. Structured exception handling has been introduced to allow you to throw exceptions and catch or replace them at a higher layer. In most systems it is quite common to throw exceptions whenever an unexpected situation occurs.

**Exception:** Calling and declaring members that implement the [TryParse](https://docs.microsoft.com/en-us/dotnet/api/system.int32.tryparse) pattern is allowed. For example:
```csharp
bool success = int.TryParse(text, out int number);
```

## Provide a rich and meaningful exception message text (SDCS-1202) [1]
The message should explain the cause of the exception, and clearly describe what needs to be done to avoid the exception.

## Throw the most specific exception that is appropriate (SDCS-1203) [2]
For example, if a method receives a null argument, it should throw `ArgumentNullException` instead of its base type `ArgumentException`.

## Don't swallow errors by catching generic exceptions (SDCS-1204) [1]
Avoid swallowing errors by catching non-specific exceptions, such as `Exception`, `SystemException`, and so on, in application code. Only top-level code, such as a last-chance exception handler, should catch a non-specific exception for logging purposes and a graceful shutdown of the application.

## Properly handle exceptions in asynchronous code (SDCS-1205) [2]
When throwing or handling exceptions in code that uses `async`/`await` or a `Task` remember the following two rules:
* Exceptions that occur within an `async`/`await` block and inside a Task's action are propagated to the awaiter.
* Exceptions that occur in the code preceding the asynchronous block are propagated to the caller.

## Always check an event handler delegate for null (SDCS-1206) [1] <img src ="Images/fullCover.png" width="16" height="16">
An event that has no subscribers is null, so before invoking, always make sure that the delegate list represented by the event variable is not null. Furthermore, to prevent conflicting changes from concurrent threads, use a temporary variable to prevent concurrent changes to the delegate.

```csharp
event EventHandler Notify;
 
void RaiseNotifyEvent(NotifyEventArgs args) 
{
    EventHandler handlers = Notify; 
    if (handlers != null) 
    { 
        handlers(this, args);
    }
}
```
**Tip:** In C# 6 you can use thread-safe [null propagation operator](https://msdn.microsoft.com/en-us/library/dn986595.aspx?f=255&MSPPError=-2147217396) to invoke delegate.

## Use a method to raise each event (SDCS-1207) [2]
Each event should have corresponding invocator.

## Don't pass null as the sender argument when raising an event (SDCS-1208) [1] <img src ="Images/fullCover.png" width="16" height="16">
Often an event handler is used to handle similar events from multiple senders. The sender argument is then used to get to the source of the event. Always pass a reference to the source (typically this) when raising the event. Furthermore don't pass null as the event data parameter when raising an event. If there is no event data, pass `EventArgs.Empty` instead of null.

**Tip:** For static events provide class type as a sender.

## Use generic constraints if applicable (SDCS-1209) [1]
Instead of casting to and from the object type in generic types or methods, use where constraints or the as operator to specify the exact characteristics of the generic parameter. For example:
```csharp
// GOOD
class MyClass<T> where T : SomeClass 
{
    void SomeMethod(T t)
    { 
        SomeClass obj = t; 
    } 
}
```
```csharp
// BAD
class MyClass<T>
{
    void SomeMethod(T t) 
    { 
        object temp = t; 
        SomeClass obj = (SomeClass) temp; 
    } 
}
```

## Evaluate the result of a LINQ expression before returning it (SDCS-1210) [1] <img src ="Images/partlyCover.png" width="16" height="16">
Consider the following code snippet
```csharp
public IEnumerable GetGoldMemberCustomers()
{
    const decimal GoldMemberThresholdInEuro = 1000000;
 
    var query =
        from customer in db.Customers
        where customer.Balance > GoldMemberThresholdInEuro
        select new GoldMember(customer.Name, customer.Balance);
 
    return query; 
}
```
Since LINQ queries use deferred execution, returning query will actually return the expression tree representing the above query. Each time the caller evaluates this result using a foreach cycle or similar, the entire query is re-executed resulting in new instances of `GoldMember` every time. Consequently, you cannot use the == operator to compare multiple `GoldMember` instances. Instead, always explicitly evaluate the result of a LINQ query using `ToList()`, `ToArray()` or similar methods.

## Do not use this and base prefixes unless it is required (SDCS-1211) [1] <img src ="Images/fullCover.png" width="16" height="16">
In a class hierarchy, it is not necessary to know at which level a member is declared to use it. Refactoring derived classes is harder if that level is fixed in the code.