# Layout
## Use a common layout (SDCS-1801) [1]
* Keep the length of each line under 150 characters.
* Use an indentation of 4 whitespaces, and don't use tabs
* Keep one whitespace between keywords like if and the expression, but don't add whitespaces after ( and before ) such as: `if (condition == null)`.
* Add a whitespace around operators, like +, -, ==, etc.
* Always follow the keywords if, else, do, while, for and foreach with opening and closing curly braces, even though the language does not require it.
* Always put opening and closing curly braces on a new line.
* Put the entire LINQ statement on one line, or start each keyword at the same indentation, like this:
```csharp
var query = from product in products where product.Price > 10 select product;
// or
var query = 
    from product in products 
    where product.Price > 10 
    select product;
```

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

## Use common style for breaking up long lines of code (SDCS-1805) [1]
If a line of code looks too big consider breaking it. For method definitions and calls put each argument on a new line with a correct indent.
```csharp
public AzureIotHubCommunicationChannel(
    CertificateStorage certificateStorage,
    string assignedHub,
    string deviceId)
```
In case of methods chain put each call on a new line:
```csharp
var converter = JsonSubtypesConverterBuilder
   .Of(typeof(IMeasure), "type")
   .RegisterSubtype(typeof(Temperature), MeasureType.Temperature)
   .RegisterSubtype(typeof(Humidity), MeasureType.Humidity)
   .SerializeDiscriminatorProperty()
   .Build();
```
