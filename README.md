# C# Coding Guidelines
## What is this?
This document attempts to provide guidelines (or coding standards if you like) for all versions of C# that are both valuable and pragmatic. The idea behind this document is make code style consistent across the projects and inside the projects as well and guarantee predictable code quality for Softeq customers.The Guidelines are based on [Dennis Doomen guidelines](https://github.com/dennisdoomen/CSharpGuidelines).

## Rule severity
Each rule in this document has its importance level:

| Rule importance   | Description                                                                      |
| :---------------- | :------------------------------------------------------------------------------- |
| 1                 | Guidelines that you should never skip and should be applicable to all situations |
| 2                 | Recommended guidelines                                                           |

## Guidelines rules naming convention
All rules must be named in the following format: **Meaningful title of the rule (SDSC-1234) \[1\]**, where 
* **SDCS** is Softeq Development CSharp abbreviation 
* **1234** is rule index; 12 is rule group index (e.g. class design guidelines), 34 is rule index.
* **\[1\]** is rule a rule severity

## Table of contents
* [Class design](10_ClassDesign.md)
* [Member design](11_MemberDesign.md)
* [Miscellaneous design](12_MiscellaneousDesign.md)
* [Maintainability](13_Maintainability.md)