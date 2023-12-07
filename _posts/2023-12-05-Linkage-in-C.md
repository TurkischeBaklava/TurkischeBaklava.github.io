---
title: Linkage
date: 2023-12-05 12:26:29 +/-TTTT
---


Every defition in C and C++ has a property known as _linkage_. A definition with _external linkage_ is visible to and can be referenced by translation units other the one in which it appears. A definition with _internal linkage_ can only be "seen" inside the translation unit in which it appears and thus cannot be referenced by other translation units.

This property is called __linkage__ because it dictates whether or not the linker is permitted to cross-reference the entity.
In a sense, linkage is the translation unit's equivalent of the __public__ and __private__ keywords in C++ class definitions.
The concept of linkage applies only to global names. The concept of linkage doesn't apply to names that are declared within a scope. A scope is specified by a set of enclosing braces such as in function or class definitions.

<br/>

The compiler defaults the linkage of symbols such that:
- Non-const global variables have external linkage by default
- Const global variables have internal linkage by default
- Functions have external linkage by default

<br/>


### static
The `static` keyword, when used in the global namespace, forces a symbol to have internal linkeage. <br>
Prior to the introduction of namespaces in C++, programs declared global names as static to make them local to a file, like in C. The use of *file statics* is inherited from C. 
This is also valid in C++. (C++ 98/03 deprecated this usage in favor of anonymous namespaces, but is no longer deprecated in > C++ 11.)

<br/>

### extern
The `extern` keyword results in a symbol having external linkage. It allows us to make _pure_ declarations. Only apply `extern` to the non-const declarations that don't provide the definition. But if you want a `const` variable to have external linkage, apply the `extern` keyword to the definition, and to all other declarations in other files.

<br/>

> The term symbol to refer to any kind of “code entity” that a linker works with. (for example a variable or function name, can be declared any number of times within its scope, however it can only defined once)

<br/>

## Code examples

```c
//header.h

/* Declarations */
/* Specified as static. Can be only accessed and referenced internally */
static int static_var;
/* non-const variable specified as external. Can be accessed and referenced across other translation units */
extern int extern_var;
/* const variable has internal linkage by default. Applying external keyword will make it external */
extern const int extern_const;
```

```c
//main2.c

#include header.h
/* Definitions */
static int static_var = 99;

void printFunc(){

    printf("ext_int: %d (main2.c) \n", extern_var);
    printf("s_int:   %d (main2.c) \n", static_var);  
}
```

```c
//main.c

...
#include "main2.h"

void printFunc();

int main(){

    static_var = 66;

    printf("extern_int:  %d (main.c) \n", extern_var);
    printf("ext_const:   %d (main.c) \n", ext_const);
    printf("static_int:  %d (main.c) \n", static_var);
    extern_var = 100;
    printFunc();

    ...
}
```

### Output
```bash
ext_int: 99 (main.c) 
ext_const:  1000 (main.c) 
s_int:  66 (main.c) 
ext_int: 100 (main2.c) 
s_int: 55 (main2.c) 

```
<br/>

> `inline` functions
>
> The real reason why `inline` function definitons are permitted in header files: it is because `inline` functions have _internal linkage_ by default, just as if they had been declared `static`.

<br/>

### Don't get confused with `static` keyword. It can refer to a lot of things.
- When used at file scope, `static` means "restrict the visibility of this variable or function so it can only be seen inside this .cpp file".
- When used at function scope, `static` means "this variable is a global, not an automatic, but it can only be seen inside this function".
- When used inside a `struct` or `class` declaration, static means "this variable is not a regular member variable, but instead acts just like a global".




<br/>
<br/>

[Internal and External Linkage in C++](https://www.goldsborough.me/c/c++/linker/2016/03/30/19-34-25-internal_and_external_linkage_in_c++/)

[Translation units and linkage](https://learn.microsoft.com/en-us/cpp/cpp/program-and-linkage-cpp?view=msvc-170)

[C++ Primer, 792](https://www.oreilly.com/library/view/c-primer-fifth/9780133053043/)

[Game Engine Architecture, 149-151](https://www.gameenginebook.com/)