# Member Design
## Allow properties to be set in any order (SDCS-1101) [1]
Properties should be stateless with respect to other properties, i.e. there should not be a difference between first setting property DataSource and then DataMember or vice-versa.

## Use a method instead of a property (SDCS-1102) [2]
* If the work is more expensive than setting a field value.
* If it represents a conversion such as the Object.ToString method.
* If it returns a different result each time it is called, even if the arguments didn't change. For example, the NewGuid method returns a different value each time it is called.
* If the operation causes a side effect such as changing some internal state not directly related to the property (which violates the [Command Query Separation principle](http://martinfowler.com/bliki/CommandQuerySeparation.html)).

**Exception:** Populating an internal cache or implementing [lazy-loading](http://www.martinfowler.com/eaaCatalog/lazyLoad.html) is a good exception.

## A method or property should do only one thing (SDCS-1103) [1]
Similarly to rule [SDCS-1001](10_ClassDesign.md#a-class-or-interface-should-have-a-single-purpose-sdcs-1001-1), a method should have a single responsibility.

## Don't expose stateful objects through static members (SDCS-1104) [2]
A stateful object is an object that contains many properties and lots of behavior behind it. If you expose such an object through a static property or method of some other object, it will be very difficult to refactor or cover a class with unit tests that relies on such a stateful object. In general, introducing a construction like that is a great example of violating many of the guidelines of this chapter.

A classic example of this is the HttpContext.Current property, part of ASP.NET. Many see the `HttpContext` class as a source of a lot of ugly code. In fact, the testing guideline [Isolate the Ugly Stuff](http://codebetter.com/jeremymiller/2005/10/21/haacked-on-tdd-and-jeremys-first-rule-of-tdd/) often refers to this class.

## Return an `IEnumerable<T>` or `IReadOnly*<T>` instead of a concrete collection class (SDCS-1105) [2]
You generally don't want callers to be able to change an internal collection, so don't return arrays, lists or other collection classes directly. Instead, return an IEnumerable<T>, `IReadOnlyCollection<T>`, `IReadOnlyList<T>` or `IReadOnlyDictionary<TKey, TValue>`.

**Exception:** Immutable collections such as `ImmutableArray<T>`, `ImmutableList<T>` and `ImmutableDictionary<TKey, TValue>` prevent modifications from the outside and are thus allowed.

## Define parameters as specific as possible (SDCS-1106) [2]
If your member needs a specific piece of data, define parameters as specific as that and don't take a container object instead. For instance, consider a method that needs a connection string that is exposed through a central IConfiguration interface. Rather than taking a dependency on the entire configuration, just define a parameter for the connection string. This not only prevents unnecessary coupling, it also improved maintainability in the long run.

**Note:** An easy trick to remember this guideline is the Don't ship the truck if you only need a package.

## Consider using domain-specific value types rather than primitives (SDCS-1107) [2]
Instead of using strings, integers and decimals for representing domain-specific types such as an ISBN number, an email address or amount of money, consider created dedicated value objects that wrap both the data and the validation rules that apply to it. By doing this, you prevent ending up having multiple implementations of the same business rules, which both improves maintainability and prevents bugs.

## Don't use mutually exclusive properties (SDCS-1108) [1]
Having properties that cannot be used at the same time typically signals a type that represents two conflicting concepts. Even though those concepts may share some of their behavior and states, they obviously have different rules that do not cooperate.

This violation is often seen in domain models and introduces all kinds of conditional logic related to those conflicting rules, causing a ripple effect that significantly increases the maintenance burden.

## Properties, arguments and return values representing collections or tasks should never be null (SDCS-1109) [1]
Returning null can be unexpected by the caller. Always return an empty collection instead of a null reference. When your member returns `Task` or `Task<T>`, return `Task.CompletedTask` or `Task.FromResult()`. This also prevents cluttering your code base with additional checks for null.