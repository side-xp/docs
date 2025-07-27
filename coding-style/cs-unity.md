# C# coding style for Unity

The *Unity Engine* uses C# for scripting. But even if [official code conventions for C#](https://learn.microsoft.com/dotnet/csharp/fundamentals/coding-style/coding-conventions) applies, the Unity API also has its own standards. And we have some custom rules on top of that.

In this guide, we try to give as much details as possible, and especially explain why we did that choice.

## Script example

This script example follows our code style guidelines for C# in Unity. See details below.

```cs
using System.Collections.Generic;

using UnityEngine;

namespace SideXP
{

    public class CodeStyleDemo<T> : MonoBehaviour
        where T : Object
    {

        // NEsted classes are allowed only if private or internal
        private class NestedClass { }

        // Enums are prefixed with "E-"
        private enum EObjectType
        {
            GameObject,
            ScriptableObject,
        }

        // Flag fields are prefixed with "F-"
        [System.Flags]
        private enum EPlayerStatus
        {
            None = 0,
            Paralyzed = 1 << 0,
            Confused = 1 << 1,
            Sleeping = 1 << 2,

            All = Paralyzed | Confused | Sleeping,
        }

        // Delegates must have the "-Delegate" suffix
        public delegate void PlayerHitDelegate();

        // Events must have the "On-" prefix (or "_on" if private)
        public event PlayerHitDelegate OnPlayerHit;

        // Public variable
        public bool followsGuidelines = true;

        // Private variable (serialized or not)
        private List<T> _objectsList = new List<T>();

        public void DisplayText()
        {
            if (followsGuidelines)
                Debug.Log("You can remove brackets for if/else statement if ALL the parts have only one instruction");
            else
                Debug.Log("If one of the parts contains more than one instruction, ALL the parts must have brackets");
        }

    }

}
```

## Detailed syntax rules

### General rules

If not specified below, elements must follow these general rules:

- **Use `PascalCase`** for naming elements
- Any rule that applies to `public` elements also applies to `protected` and `protected internal` elements
- Any rule that applies to `private` elements also applies to `internal` elements
- A single `*.cs` file should contain **only 1 class** (only exception for `private` nested classes)
- Always have **only 1 statement** maximum per line
- Always have **only 1 declaration** maximum per line
- Prefer **full names** to abbreviations, excepted for common ones used as name such as *Id*, *Url*, ...
- For loops and conditions, [**use inversion** to reduce nesting](https://www.youtube.com/watch?v=CFRhGnuXG-4) (prefer an early `return`, `break` or `continue` statement to check for *false* conditions instead of diong all operations in *true* condition)
- **Use `nameof()`** instead of plain strings to reference elements when possible
- **Never use `this`** unless absolutely necessary
- **Don't use `var`** unless the variable type is obvious or appears in the right side of a `=` operator
- Indent by **4 spaces**
- Always place **brackets on a new line**
- **Prefer custom delegates** over [`Func<>`](https://learn.microsoft.com/dotnet/api/system.func-2) or [`Action<>`](https://learn.microsoft.com/dotnet/api/system.action-1) if not obvious (since they can be documented as well as their parameters)
- It's allowed to remove brackets for condition or loop statements if they contain only one instruction. For conditions though, if at least one part of a `if/else if/else` combination contains instructions, all the parts must have brackets
- Feel free to use "useless" parenthesis or group operations in a ghost brackets block to improve reading and code layout when meaningful

### Imports (`using` statements)

- Always declare imports and aliases outside namespace declaration
- Take care to remove unused imports
- Group using statements by category in the following order:
    1. System
    2. Unity (or other high level engine or framework)
    3. Thrid-party libraries or plugins
    4. Custom

Example:

```cs
using System;
using System.Collections.Generic;
// You can separate categories with a blank line
using UnityEngine;
using UnityEditor;
using DG.Tweening;
using SideXP.Core;
using SideXP.Core.EditorOnly;
```

### Classes & structs

- **Always document** a class in-code with a summary comment (`/// <summary>`), startingg with "*Represents...*" if applicable
- **Use nested class only if `private` or `internal`**, otherwise you must create a new file for it
- **Add blank line** after the first bracket of a class declaration and before that last one
- **Avoid using `-Base` suffix** for a base class meant to be inherited, unless it's `abstract`
- **Always add suffix `-Attribute`** if the class inherits from `System.Attribute`
- **Always add suffix `-Exception`** if the class inherits from `System.Exception`

### Interfaces

- **Always add prefix `I-`** for interfaces
- **Always document** an interface in-code with a summary comment (`/// <summary>`), starting with "*Qualifies a class for...*" if applicable

### Enums

- **Always add prefix `E-`** for regular enumerations
- **Always add prefix `F-`** for enumerations marked with [`System.Flags`](https://learn.microsoft.com/dotnet/api/system.flagsattribute)
- **Always document** an enumeration in-code with a summary comment (`/// <summary>`)
- **Prefer use bit shift notation** for flag fields, starting with `1 << 0` (eg. `1 << 1`, `1 << 2`, ...)
- **Always add a comma** after the last item

> Note: The `F-` suffix is used to clarify if a field can have multiple values, without the need of checking for the enumeration declaration.

### Fields

- If `private` or `internal`, **use `camelCase`** prefixed with `_`
- If `private` and `static`, **use `camelCase`** prefixed with `s_`
- If `constant` (whatever public or private), **use `PascalCase`**
- Choose whether all the fields have an **initialization value** or none of them, but never both (except for *events* and *delegates*). It’s allowed to use `default` as initialization value
- If `public`, **always document** using a summary comment (`/// <summary>`), or with a `TooltipAttribute` in Unity
- If `private`, we encourage to document it using a summary comment (`/// <summary>`), but that's not mandatory
- Always use `is-` or `has-` for boolean fields

> Note: The `_` prefix is meant to check for scope by just reading the syntax. And the `s_` prefix is meant to clearly state that the owner is the class itself.

### Functions

- **Use `PascalCase`**
- **Use `camelCase`** for parameters
- **Always document** using a summary comment (`/// <summary>`), and include parameters documentation if not obvious
- Use of **local functions** is allowed
- Always use prefix `Is-`, `Has-` or `Any-` for functions that just return a boolean

### Generic types

- **Use only `T-`** as prefix for generic parameters
- When several type parameters are declared, use clear names all prefixed with `T-` (eg. `TState`, `TUser`, ...). If not related to a specific base type, use an id instead (eg. `T0`, `T1`, ...)
- Always place a `where` constraint on a new line

### Delegates

- **Always document** using a summary comment (`/// <summary>`)
- **Always add suffix `-Delegate`**
- Parameters are named using `camelCase`

### Events

- **Always add prefix `On-`** (or `_on` if `private`)

### `new` operator

- **Prefer using objct initializers** instead of setting all `public` fields one by one

### Comments

- Limit comment line width to 140 chars

### Regions

Use of regions is not mandatory, but strongly encouraged for long scripts (250+ lines), or scripts that implement functions used in several contexts (eg. utility classes).

Regions can be used to group content by access (*Public*, *Private*, ...), or by feature.

## Additional rules

### Custom markers

Feel free to use the following markers in comments:

- `@todo`: Something to check or do later, whether you plan to create an issue for it or just take a note for yourself
- `@note`: Additional information about something interesting in your code. Useful to provide information about something unusual or unexpected you had to do to solve a problem
- `@fix`: Identify a special case that had to be handled to address an issue