# Framework
## Use C# type aliases instead of the types from the System namespace (SDCS-1601) [1]
For instance, use `object` instead of `Object`, `string` instead of `String`, and `int` instead of `Int32`. These aliases have been introduced to make the primitive types first class citizens of the C# language, so use them accordingly.

**Exception:** When referring to static members of those types, it is custom to use the full CLS name, e.g. `Int32.Parse()` instead of `int.Parse()`.

## Properly fill the attributes of the AssemblyInfo.cs file (SDCS-1605) [1]
Ensure that the attributes for the company name, description, copyright statement, version, etc. are filled. One way to ensure that version and other fields that are common to all assemblies have the same values, is to move the corresponding attributes out of the `AssemblyInfo.cs` into a `SolutionInfo.cs` file that is shared by all projects within a solution.

```csharp
[assembly: AssemblyProduct("Product name")]
[assembly: AssemblyDescription("Short product description")]
[assembly: AssemblyCompany("Company Name")]
[assembly: AssemblyCopyright("Copyright (c) 2020 Company Name")]
[assembly: AssemblyVersion("1.0.0.0")]
[assembly: AssemblyFileVersion("1.0.0.0")]
```

## Avoid LINQ for simple expressions (SDCS-1606) [2]
```csharp
// GOOD
var query = items.Where(i => i.Length > 0);
```
```csharp
// BAD
var query = from item in items where item.Length > 0; 
```

## Only use the dynamic keyword when talking to a dynamic object (SDCS-1608) [1] 
The dynamic keyword has been introduced for working with dynamic languages. Using it introduces a serious performance bottleneck because the compiler has to generate some complex Reflection code.
Use it only for calling methods or members of a dynamically created instance class (using the `Activator`) as an alternative to `Type.GetProperty()` and `Type.GetMethod()`, or for working with COM Interop types.

## Favor async/await over the Task (SDCS-1609) [1]
Using the new C# 5.0 keywords results in code that can still be read sequentially and also improves maintainability a lot, even if you need to chain multiple asynchronous operations.

```csharp
// GOOD
public async Task GetDataAsync()
{
    var result = await MyWebService.FetchDataAsync();
    return new Data (result);
}
```
```csharp
// BAD
public Task GetDataAsync()
{
    return MyWebService.FetchDataAsync().ContinueWith(t => new Data(t.Result));
}
```
**Tip:** Even if you need to target .NET Framework 4.0 you can use the `async` and `await` keywords. Simply install the [Async Targeting Pack](http://www.microsoft.com/en-us/download/details.aspx?id=29576).