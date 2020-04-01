# Performance
## Consider using Any() to determine whether an IEnumerable<T> is empty (SDCS-1501) [3]  
When a method or other member returns an IEnumerable<T> or other collection class that does not expose a Count property, use the Any() extension method rather than Count() to determine whether the collection contains items. If you do use Count(), you risk that iterating over the entire collection might have a significant impact (such as when it really is an IQueryable<T> to a persistent store).

**Note:** If you return an IEnumerable<T> to prevent editing from outside the owner as explained in SDCS-1105, and you're developing in .NET 4.5 or higher, consider the new read-only classes.
