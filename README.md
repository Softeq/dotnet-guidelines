# C# Coding Guidelines
## What is this?
This document attempts to provide guidelines (or coding standards if you like) for all versions of C# that are both valuable and pragmatic. The idea behind this document is make code style consistent across the projects and inside the projects as well and guarantee predictable code quality for Softeq customers. The Guidelines are based on [Dennis Doomen guidelines](https://github.com/dennisdoomen/CSharpGuidelines).

## Rule severity
Each rule in this document has its importance level:

| Rule importance   | Description                                                                      |
| :---------------- | :------------------------------------------------------------------------------- |
| 1                 | Guidelines that you should never skip and should be applicable to all situations |
| 2                 | Recommended guidelines                                                           |

## Code Analysis

| Icon  			| Description                                                                      |
| :---------------- | :------------------------------------------------------------------------------- |
| <img src ="Images/fullCover.png" width="50" height="50">   | Fully covered by Re-sharper                                                      |
| <img src ="Images/partlyCover.png" width="50" height="50">  | Partly covered by Re-sharper                                                     |
|                   | If no image, rules aren't covered by Re-sharper								   |

## Guidelines rules naming convention
All rules must be named in the following format: **Meaningful title of the rule (SDSC-1234) \[1\]**, where 
* **SDCS** is Softeq Development CSharp abbreviation.
* **1234** is the rule index which includes group index (12) and the rule index (34).
* **\[1\]** is the rule a rule severity.

## Table of contents
* [10 Class design](10_ClassDesign.md)
* [11 Member design](11_MemberDesign.md)
* [12 Miscellaneous design](12_MiscellaneousDesign.md)
* [13 Maintainability](13_Maintainability.md)
* [14 Naming](14_Naming.md)
* [16 Framework](16_Framework.md)
* [17 Documentation](17_Documentation.md) 
* [18 Layout](18_Layout.md)
* [Important resources](99_ImportantResources.md)
* [DotSettings](SharedRules.DotSettings)
* [Editorconfig](SharedRules.editorconfig)
