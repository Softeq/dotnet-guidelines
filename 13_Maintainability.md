# Maintainability
## Make all members private and types internal sealed by default (SDCS-1302) [2]
To make a more conscious decision on which members to make available to other classes, first restrict the scope as much as possible. Then carefully decide what to expose as a public member or type.

## Avoid conditions with double negatives (SDCS-1303) [2] <img src ="Images/fullCover.png" width="16" height="16">
Although a property like customer.HasNoOrders makes sense, avoid using it in a negative condition like this:
```csharp
bool hasOrders = !customer.HasNoOrders;
```
Double negatives are more difficult to grasp than simple expressions, and people tend to read over the double negative easily.

## Name assemblies after their contained namespace (SDCS-1304) [2]
All DLLs should be named according to the pattern `Company.Component.dll` where Company refers to your company's name and Component contains one or more dot-separated clauses. For example `Softeq.Web.Controls.dll`.

As an example, consider a group of classes organized under the namespace `Softeq.Web.Binding` exposed by a certain assembly. According to this guideline, that assembly should be called `Softeq.Web.Binding.dll`.

## Name a source file to the type it contains (SDCS-1305) [1]
Use Pascal casing to name the file and don't use underscores.

## Limit the contents of a source code file to one type (SDCS-1306) [2]

**Exception:** Nested types should, for obvious reasons, be part of the same file.

**Exception:** Types that only differ by their number of generic type parameters should be part of the same file.

## Name a source file to the logical function of the partial type (SDCS-1307) [2]
When using partial types and allocating a part per file, name each file after the logical part that part plays. For example:
```csharp
// MyClass.cs
public partial class MyClass
{
    // some code
}
```
```csharp
// MyClass.Designer.cs
public partial class MyClass
{
    // some code
}
```

## Use using statements instead of fully qualified type names (SDCS-1308) [2] <img src ="Images/fullCover.png" width="16" height="16">
Limit usage of fully qualified type names to prevent name clashing. For example, don't do this:
```csharp
// GOOD
using System.Collections.Generic;
...
var list = new List();
```
```csharp
// BAD
var list = new System.Collections.Generic.List();
```
If you do need to prevent name clashing, use a using directive to assign an alias:
```csharp
using Label = System.Web.UI.WebControls.Label;
```

## Don't use "magic" numbers (SDCS-1309) [1]
Don't use literal values in your code, other than to define symbolic constants. For example:
```csharp
public class Whatever 
{
    public static readonly Color PapayaWhip = new Color(0xFFEFD5);
    public const int MaxNumberOfWheels = 18; 
}
```
Literals are allowed when their meaning is clear from the context, and not subject to future changes, For example:
```csharp
mean = (a + b) / 2; // okay 
WaitMilliseconds(waitTimeInSeconds / 1000); // clear enough
```
If the value of one constant depends on the value of another, attempt to make this explicit in the code.
```csharp
public class SomeSpecialContainer 
{ 
    public const int MaxItems = 32; 
    public const int HighWaterMark = 3 * MaxItems / 4; // at 75% 
}
```
**Note:** An enumeration can often be used for certain types of symbolic constants

## Consider using `var` for increasing readability (SDCS-1310) [2] <img src ="Images/fullCover.png" width="16" height="16">
Apply implicit typing only when it really increases readability of the code.
* Prefer using vars for long generic types
```csharp
// GOOD
var cache = _cacheService.GetCache();
```
```csharp
// BAD
Dictionary<DateTime, Dictionary<RequestType, RequestData>> cache = _cacheService.GetCache();
```
* Use implicit typing in case you do not care about actual instance type:
```csharp
var command = CommandFactory.Create(inputData);
messageBus.Publish(command);
```
* Use var when the type of the variable is obvious from the context:
```csharp
var employe = _employeRepository.GetById(employeId);
```
* Do not use var for numeric types declaration, as it is not obvious what is the type of the variable:
```csharp
var offset = 10.5; // is it double or float?
var temperature = 10; // does developer really wanted to use integer type or it was done by mistake?
```
## Declare and initialize variables as late as possible (SDCS-1311) [2]
Avoid the C and Visual Basic styles where all variables have to be defined at the beginning of a block, but rather define and initialize each variable at the point where it is needed.

## Assign each variable in a separate statement (SDCS-1312) [1] <img src ="Images/fullCover.png" width="16" height="16">
Don't use confusing constructs like the one below:
```csharp
var result = someField = GetSomeMethod();
```

## Favor Object and Collection Initializers over separate statements (SDCS-1313) [2] <img src ="Images/fullCover.png" width="16" height="16">
Use [Object Initializers](http://msdn.microsoft.com/en-us/library/bb384062.aspx):
```csharp
// GOOD
var startInfo = new ProcessStartInfo("myapp.exe") 
{
    StandardOutput = Console.Output,
    UseShellExecute = true 
};
```
```csharp
// BAD
var startInfo = new ProcessStartInfo("myapp.exe"); 
startInfo.StandardOutput = Console.Output;
startInfo.UseShellExecute = true;
```
Use collection or [dictionary initializers](http://msdn.microsoft.com/en-us/library/bb531208.aspx):
```csharp
// GOOD
var countries = new List { "Netherlands", "United States" };
```
```csharp
// BAD
var countries = new List();
countries.Add("Netherlands");
countries.Add("United States");
```

## Don't make explicit comparisons to true or false (SDCS-1314) [1] <img src ="Images/partlyCover.png" width="16" height="16">
It is usually bad style to compare a bool-type expression to true or false.
```csharp
// GOOD
while (condition)
{
    // Code
}
```
```csharp
// BAD
while (condition == false)
while (condition != true)
while (((condition == true) == true) == true)// where do you stop?
```

## Don't change a loop variable inside a for or foreach loop (SDCS-1315) [1] <img src ="Images/fullCover.png" width="16" height="16">
Updating the loop variable within the loop body is generally considered confusing, even more so if the loop variable is modified in more than one place. Although this rule also applies to foreach loops, an enumerator will typically detect changes to the collection the foreach loop is iteration over.
```csharp
for (int index = 0; index < 10; ++index) 
{ 
    if (_some condition_)
    {
        index = 11; // Wrong! Use 'break' or 'continue' instead. 
    }
}
```

## Always add a block after keywords such as if, else, while, for and foreach (SDCS-1317) [1] <img src ="Images/partlyCover.png" width="16" height="16">
This rules avoids possible confusion in statements of the form.
```csharp
// GOOD
if (b1) 
{ 
    if (b2) 
    { 
        Foo(); 
    } 
    else 
    { 
        Bar(); 
    } 
}
```
```csharp
// BAD
if (b1)
    if (b2) Foo();
else Bar(); // which 'if' goes with the 'else'?
```

## Always add a default block after the last case in a switch statement (SDCS-1318) [2]
Add a descriptive comment if the default block is supposed to be empty. Moreover, if that block is not supposed to be reached throw an `InvalidOperationException` to detect future changes that may fall through the existing cases. This ensures better code, because all paths the code can travel have been thought about.
```csharp
void Foo(string answer) 
{ 
    switch (answer) 
    { 
        case "no": 
        {
            Console.WriteLine("You answered with No"); 
            break;
        } 
        case "yes":
        { 
            Console.WriteLine("You answered with Yes"); 
            break;
        }
        default: 
        {
            // Not supposed to end up here. 
            throw new InvalidOperationException("Unexpected answer " + answer);
        } 
    } 
}
```

## Don't use if-else statements instead of a simple (conditional) assignment (SDCS-1321) [2] <img src ="Images/fullCover.png" width="16" height="16">
Express your intentions directly.
```csharp
// GOOD
return someString ?? "Unavailable";
```
```csharp
// BAD
string result;
 
if (someString != null)
{ 
    result = someString; 
}
else
{
    result = "Unavailable";
}
 
return result; 
```

## Encapsulate complex expressions in a method, property or  variable (SDCS-1322) [2]
Consider the following example:
```csharp
if (member.HidesBaseClassMember && (member.NodeType != NodeType.InstanceInitializer))
{
    // do something
}
```
In order to understand what this expression is about, you need to analyze its exact details and all of its possible outcomes. Obviously, you can add an explanatory comment on top of it, but it is much better to replace this complex expression with a clearly named method:
```csharp
if (NonConstructorMemberUsesNewKeyword(member)) 
{ 
    // do something
}
...
private bool NonConstructorMemberUsesNewKeyword(Member member) 
{ 
    return member.HidesBaseClassMember &&
        (member.NodeType != NodeType.InstanceInitializer);
} 
```
You still need to understand the expression if you are modifying it, but the calling code is now much easier to grasp.

## Call the more overloaded method from other overloads (SDCS-1323) [2] 
This guideline only applies to overloads that are intended to provide optional arguments. Consider, for example, the following code snippet:
```csharp
public class MyString 
{
    private string _someText;
 
    public int IndexOf(string phrase) 
    { 
        return IndexOf(phrase, 0);
    }
 
    public int IndexOf(string phrase, int startIndex) 
    { 
        return IndexOf(phrase, startIndex, someText.Length - startIndex);
    }
 
    public virtual int IndexOf(string phrase, int startIndex, int count) 
    { 
        return _someText.IndexOf(phrase, startIndex, count);
    } 
}
```
The class MyString provides three overloads for the IndexOf method, but two of them simply call the one with one more parameter. Notice that the same rule applies to class constructors; implement the most complete overload and call that one from the other overloads using the `this` operator. Also notice that the parameters with the same name should appear in the same position in all overloads.

**Important:** If you also want to allow derived classes to override these methods, define the most complete overload as a protected virtual method that is called by all overloads.

## Don't use ref or out parameters (SDCS-1325) [2]
They make code less understandable and might cause people to introduce bugs. Instead, return compound objects.

**Exception:** use out parameters only in TryXXX methods.

## Don't use parameters as temporary variables (SDCS-1326) [2]
Never use a parameter as a convenient variable for storing temporary state. Even though the type of your temporary variable may be the same, the name usually does not reflect the purpose of the temporary variable.

## Always check the result of an `as` operation (SDCS-1327) [1]
If you use `as` to obtain a certain interface reference from an object, always ensure that this operation does not return null. Failure to do so may cause a NullReferenceException at a much later stage if the object did not implement that interface. If you are sure the object is of a certain type, use the is operator instead.

## Don't comment out code (SDCS-1328) [2]
Never check in code that is commented out. Nobody knows what to do when they encounter a block of commented-out code. Was it temporarily disabled for testing purposes? Was it copied as an example? Should I delete it?

## Always add message to ObsoleteAttribute (SDCS-1329) [1] <img src ="Images/fullCover.png" width="16" height="16">
Never add ObsoleteAttribute without a message. Message must contain information on what interface you should use instead of the obsolete one.
```csharp
// GOOD
[Obsolete("This property is obsolete. Use NewProperty instead.")]
public string OldProperty
{
    get { return "The old property value."; }
}
  
public string NewProperty
{
    get { return "The new property value."; }
}
```
```csharp
// BAD
[Obsolete]
public string OldProperty
{
    get { return "The old property value."; }
}
  
public string NewProperty
{
    get { return "The new property value."; }
}
```
