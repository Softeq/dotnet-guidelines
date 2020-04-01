# Layout
## Use a common layout (SDCS-1801) [1]
* Keep the length of each line under 130 characters.
* Use an indentation of 4 whitespaces, and don't use tabs
* Keep one whitespace between keywords like if and the expression, but don't add whitespaces after ( and before ) such as: `if (condition == null)`.
* Add a whitespace around operators, like +, -, ==, etc.
* Always follow the keywords if, else, do, while, for and foreach with opening and closing curly braces, even though the language does not require it.
* Always put opening and closing parentheses on a new line, except the case when the method has an empty implementation. In this case the operator brackets must be placed on the line as the method declaration
```csharp
var dto = new ConsumerDto() { }
```
* Don't indent object Initializers and initialize each property on a new line, so use a format like this:
```csharp
var dto = new ConsumerDto()
 {
     Id = 123,
     Name = "Microsoft",
     PartnerShip = PartnerShip.Gold
 }
```
* Don't indent lambda statements and use a format like this:
```csharp
methodThatTakesAnAction.Do(x =>
{
   // code
});
```

* Put the entire LINQ statement on one line, or start each keyword at the same indentation, like this:
```csharp
var query = from product in products where product.Price > 10 select product;
// or
var query =
    from product in products
    where product.Price > 10
    select product;
```
* Start the LINQ statement with all the from expressions and don't interweave them with restrictions.

* Add an empty line between multi-line statements, between members, after the closing parentheses, between unrelated code blocks, around the #region keyword, and between the using statements of different root namespaces.

## Order and group namespaces according the company (SDCS-1802) [2]
```csharp
// System namespaces are first
using System;
using System.Collections;
using System.XML;
// Then any other namespaces grouped by name
using Softeq.Toolkit.Platform;
using Softeq.MessageBus;
using Telerik.WebControls;
using Telerik.Ajax;
```
**Tip:** Visual Studio has organize usings feature that can automate this action. You can find more info here in the organize usings section.

## Place members in a well-defined order (SDCS-1803) [1]
Maintaining a common order allows other team members to find their way in your code more easily. In general, a source file should be readable from top to bottom, as if reading a book, to prevent readers from having to browse up and down through the code file.

Members must be ordered by type in the following order:
1. Constants
2. Fields
3. Constructors
4. Finalizer
5. Events
6. Properties
7. Methods
8. Nested types

Then by access modifier:
1. Public
2. Internal
3. Protected
4. Private

Then by type/instance modifiers:
1. Static
2. Instance

Then by assignment modifiers:
1. Const
2. Readonly

And then by inheritance modifiers:
1. Abstract
2. Virtual
3. Override
4. No modifier

**Note:** members with the same order priority must be placed in call order.

## Avoid using #regions (SDCS-1804) [1]
If the class looks too big consider refactoring it into set of classes according to SRP.
