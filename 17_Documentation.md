# Documentation
## Write comments and documentation in US English (SDCS-1701) [1] 

## Document all public types and members (SDCS-1702) [3]
Documenting your code allows Visual Studio to pop-up the documentation when your class is used somewhere else. Furthermore, by properly documenting your classes, tools can generate professionally looking class documentation.

## Write XML documentation with other developers in mind (SDCS-1703) [2]
Write the documentation of your type with other developers in mind. Assume they will not have access to the source code and try to explain how to get the most out of the functionality of your type.

## Write MSDN-style documentation (SDCS-1704) [3]
Following the MSDN online help style and word choice helps developers find their way through your documentation more easily.

**Tip:** The tool [GhostDoc](http://submain.com/products/ghostdoc.aspx) can generate a starting point for documenting code with a shortcut key.

## Avoid inline comments (SDCS-1705) [2]
If you feel the need to explain a block of code using a comment, consider replacing that block with a method with a clear name.

**Exception:** write inline comments for workarounds and complicated logic.

## Only write comments to explain complex algorithms or decisions (SDCS-1706)  [1]
Try to focus comments on the why and what of a code block and not the how. Avoid explaining the statements in words, but instead help the reader understand why you chose a certain solution or algorithm and what you are trying to achieve. If applicable, also mention that you chose an alternative solution because you ran into a problem with the obvious solution.

## Don't use comments for tracking work to be done later (SDCS-1707) [2]
Annotating a block of code or some work to be done using a TODO or similar comment may seem a reasonable way of tracking work-to-be-done. But in reality, nobody really searches for comments like that. Use a work item tracking system such as Team Foundation Server to keep track of leftovers.

## Add header to each source file (SDCS-1708) [2]
In the beginning of each source file that was created by Softeq team on the project there should be the following comment header added:
```csharp
// Developed for [customer name] by Softeq Development Corporation
// http://www.softeq.com
```
Where [customer name] - the name of a client company (or the name of a client if he doesn’t represent a company). This name can be taken from JIRA or you can ask PM of the project. The name of a company (or the name a client) must be grammatically correct, it’s a very important point for a client.

For example:
```csharp
// Developed for NVIDIA by Softeq Development Corporation
// http://www.softeq.com
```
Please note that if your IDE doesn’t support automatic filling of the [customer name] value during the file creation, you should fill this value manually after copy-pasting.
