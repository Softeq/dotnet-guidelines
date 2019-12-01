# Documentation
## Use comments selectively (SDCS-1701) [2]
Document only:
1. Public API of the libraries that are used in several system components (e.g. microservices) or acts as a shared library for multiple applications.
2. Complex algorithms
3. Workarounds 

## Add header to each source file (SDCS-1708) [2]
In the beginning of each source file that was created by Softeq team on the project there should be the following comment header added:
```csharp
// Developed for $CUSTOMER_NAME$ by Softeq Development Corporation
// http://www.softeq.com
```
Where [customer name] - the name of a client company (or the name of a client if he doesn’t represent a company). This name can be taken from JIRA or you can ask PM of the project. The name of a company (or the name a client) must be grammatically correct, it’s a very important point for a client.
**Note:** If your IDE doesn’t support automatic filling of the [customer name] value during the file creation, you should fill this value manually after copy-pasting.
**Tip:** In R# you need to run Cleanup Code to add file header template