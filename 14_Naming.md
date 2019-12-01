# Naming
## Use US English (SDCS-1401) [1]
All type members, parameters and variables should be named using words from the American English language.
* Choose easily readable, preferably grammatically correct names. For example, HorizontalAlignment is more readable than AlignmentHorizontal.
* Favor readability over brevity. The property name CanScrollHorizontally is better than ScrollableX (an obscure reference to the X-axis).
* Avoid using names that conflict with keywords of widely used programming languages.

**Exception:** In most projects, you will use words and phrases from your domain and names specific to your company. Visual Studio's Static Code Analysis performs a spelling check on all code, so you may need to add those terms to a [Custom Code Analysis Dictionary](http://blogs.msdn.com/fxcop/archive/2007/08/20/new-for-visual-studio-2008-custom-dictionaries.aspx).

## Use proper casing for language elements (SDCS-1402) [1]
| Language element        | Casing                       | Example          |
| :---------------------- | :--------------------------- |----------------- |
| Class, Struct           | Pascal                       | `AppDomain`      |
| Interface               | Pascal                       | IBusinessService |
| Enumeration type        | Pascal                       | ErrorLevel       |
| Enumeration values      | Pascal                       | FatalError       |
| Event                   | Pascal                       | Click            |
| Private field           | Camel with leading underline | _employee        |
| Constant field          | Pascal                       | MaximumItems     |
| Constant Local variable | Camel                        | maximumItems     |
| Read-only static field  | Pascal                       | AccentBrush      |
| Local Variable          | Camel                        | connectionString |
| Method                  | Pascal                       | ToString         |
| Namespace               | Pascal                       | System.Drawing   |
| Parameter               | Camel                        | typeName         |
| Type Parameter          | Pascal                       | TView            |
| Property                | Pascal                       | BackColor        |

## Don’t use protected, internal and public fields (SDCS-1403) [1]
**Note:** They can be replaced with auto properties with appropriate access modifier.

## Don't include numbers in variables, parameters and type members (SDCS-1404) [2]
In most cases they are a lazy excuse for not defining a clear and intention-revealing name.

## Use only well-known abbreviations (SDCS-1406) [2]
For example, use OnButtonClick rather than OnBtnClick. Avoid single character variable names, such as i or q. Use index or query instead.

**Note:** Use camel casing for abbreviations.

```csharp
// Good
Http
Io
Db
```

```csharp
/// Bad
HTTP
IO
DB
```

## Name a member, parameter or variable according to its meaning and not its type (SDCS-1407) [2] 
* Use functional names. For example, GetLength is a better name than GetInt.
* Don't use terms like Enum, Class or Struct in a name.

## Name types using nouns, noun phrases or adjective phrases (SDCS-1408) [2]
Bad examples include `SearchExamination` (a page to search for examinations), Common (does not end with a noun, and does not explain its purpose) and `SiteSecurity` (although the name is technically okay, it does not say anything about its purpose). Good examples include `BusinessBinder`, `SmartTextBox`, or `EditableSingleCustomer`.

## Name generic type parameters with descriptive names (SDCS-1409) [2]
* Always prefix descriptive type parameter names with the letter T.
* Always use a descriptive names unless a single-letter name is completely self-explanatory and a longer name would not add value. Use the single letter T as the type parameter in that case.
* Consider indicating constraints placed on a type parameter in the name of parameter. For example, a parameter constrained to `ISession` may be called `TSession`.

## Don't repeat the name of a class or enumeration in its members (SDCS-1410) [1]
```csharp
// GOOD
public class Employee
{ 
    public Employee Get() 
    {
    } 
    
    public void Delete()
    {
    }
  
    public void AddNewJob()
    {
    }
    
    public void RegisterForMeeting()
    {
    }
}
```
```csharp
// BAD
public class Employee
{ 
    public void GetEmployee()
    {
    }

    public void DeleteEmployee()
    {
    }
}
```

## Name members similarly to members of related .NET Framework classes (SDCS-1411) [2]
.NET developers are already accustomed to the naming patterns the framework uses, so following this same pattern helps them find their way in your classes as well. For instance, if you define a class that behaves like a collection, provide members like `Add`, `Remove` and `Count` instead of `AddItem`, `Delete` or `NumberOfItems`.

## Avoid short names or names that can be mistaken for other names (SDCS-1412) [1]
Although technically correct, statements like the following can be confusing:
```csharp
var b001 = (lo == l0) ? (I1 == 11) : (lOl != 101);
```

## Properly name properties (SDCS-1413) [2]
* Name properties with nouns, noun phrases, or occasionally adjective phrases.
* Name boolean properties with an affirmative phrase. E.g. `CanSeek` instead of `CantSeek`. If for some reason it's more convenient to use negative phrase - use NOT in full form, not short. E.g. `CanNotSeek` instead of `CantSeek`
* Consider prefixing boolean properties with `Is`, `Has`, `Can`, `Allows`, or `Supports`.
* Consider giving a property the same name as its type. When you have a property that is strongly typed to an enumeration, the name of the property can be the same as the name of the enumeration. For example, if you have an enumeration named `CacheLevel`, a property that returns one of its values can also be named `CacheLevel`.

## Name methods using a verb or a verb-object pair (SDCS-1414) [2]
Name methods using a verb like Show or a verb-object pair such as ShowDialog. A good name should give a hint on the what of a member, and if possible, the why.

Also, don't include And in the name of a method. It implies that the method is doing more than one thing, which violates the single responsibility principle explained in [SDCS-1103](10_ClassDesign.md#use-an-interface-rather-than-a-base-class-to-support-multiple-implementations-stcs-1003-2).

## Name namespaces using names, layers, verbs and features (SDCS-1415) [2]
For instance, the following namespaces are good examples of that guideline.
```csharp
NHibernate.Extensibility
Microsoft.ServiceModel.WebApi
Microsoft.VisualStudio.Debugging
FluentAssertion.Primitives
CaliburnMicro.Extensions
```
**Note:** Never allow namespaces to contain the name of a type, but a noun in its plural form (e.g. Collections) is usually OK.

## Use a verb or verb phrase to name an event (SDCS-1416) [2]
Name events with a verb or a verb phrase (e.g. Click, Deleted, Closing, Minimizing, Arriving, etc.). Declaration of the Search event may look like this:
```charp
public event EventHandler<SearchArgs> Search;
```

## Use -ing and -ed to express pre-events and post-events (SDCS-1417) [2]
For example, a close event that is raised before a window is closed would be called `Closing`, and one that is raised after the window is closed would be called `Closed`. Don't use `Before` or `After` prefixes or suffixes to indicate pre and post events.

Suppose you want to define events related to the deletion of an object. Avoid defining the `Deleting` and `Deleted` events as BeginDelete and `EndDelete`. Define those events as follows:
* Deleting: Occurs just before the object is getting deleted
* Delete: Occurs when the object needs to be deleted by the event handler.
* Deleted: Occurs when the object is already deleted.

**Important:** To clarify moment of event execution summary for the event should be written describing exact moment of time when event would be raised
Example:
* Deleting (Summary: event is raised before deleting)
* Deleted (Summary: event is raised after deleting is finished)

**Note:** Avoid events that have undetermined moment of execution like

Deleting (Summary: Event is raised at some point while deleting executes)

## Prefix an event handler with On (SDCS-1418) [2]
It is good practice to prefix the method that handles an event with On. For example, a method that handles the Closing event can be named `OnClosing`.

## Group extension methods in a class suffixed with Extensions (SDCS-1419) [2]
If the name of an extension method conflicts with another member or extension method, you must prefix the call with the class name. Having them in a dedicated class with the Extensions suffix improves readability.

## Post-fix asynchronous methods with Async of TaskAsync (SDCS-1420) [2]
The general convention for methods that return `Task` or `Task<TResult>` is to post-fix them with Async, but if such a method already exists, use `TaskAsync` instead.

## Identifiers that refer to a collection type should have plural names [2]

## Use correct names for enums [1]
When you define a simple enum (e.g. `SeasonOfYear`) use singular form, in case flag enum use plural form (e.g. `FilePermissions`). 