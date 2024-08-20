## SoftFauna C Style Guidelines

## Index

- [SoftFauna C Style Guidelines](#softfauna-c-style-guidelines)
- [Index](#index)
- [Documentation](#documentation)
  - [Naming Conventions](#naming-conventions)
  - [Formatting](#formatting)
  - [File Boilerplate](#file-boilerplate)
    - [Header Files](#header-files)
    - [C Source Files](#c-source-files)
  - [Include Statements](#include-statements)
  - [Preproccessor](#preproccessor)
    - [Macros](#macros)
    - [Constants](#constants)
  - [Comments](#comments)
  - [Functions](#functions)
  - [Methods](#methods)
  - [Variables](#variables)
  - [Pointers](#pointers)
  - [Conditionals](#conditionals)
    - [if elseif else](#if-elseif-else)
    - [switch base default](#switch-base-default)
  - [Loops](#loops)
    - [while dowhile](#while-dowhile)
    - [for](#for)
  - [Objects](#objects)
    - [typedefs](#typedefs)
    - [structs](#structs)
    - [enums](#enums)
- [License](#license)

## Documentation

### Naming Conventions

`snake_case` may be used for functions, methods, mutable variables, typedefs, structs, and enums.
While, `SCREAMING_SNAKE_CASE` is for macros, defines, constant variables, and enum constants.

| | |
|:------------------- |:------------------------- |
| extern/header item  | `[namespace]_` prefix     |
| static variable     | `g_` prefix               |
| method              | `[object]_` prefix        |
| typedef             | `_t` suffix               |
| unsafe macro        | `_UNSAFE` suffix          |

### Formatting

Limit lines to always be less than 100 columns, anything more and some editors require scorlling.

Use 4 space indentation, no tab characters.

Curly braces are (*almost*) always on a seperate line, `allman`/`BSD` style.

All keywords which accept parenthesis, are required to have 1 space of padding between itself and the parenthesis. This non-exclusive list contains: `if`, `else if`, `for`, and `while` expressions. Further, `#include` follow identically with their aligators and double quotes.

There should be 1 space of padding between function name and parameter list for all function definitions, prototypes, and calls. However, when dealing with macros this padding spaces is optional.

### File Boilerplate

Within the first page (24 lines) of a file, the project's name and project url, the file's purpose, and the project's basic licensing information must be visible. each file should fit the following format

``` C
/* SoftFauna Style Guide - C Lang
 * <https://github.com/SoftFauna/softfauna-style-guide.git>
 *
 * This file exists as an example of how all SoftFauna file boilerplate should
 * look */

/* SoftFauna C Style Guide by is marked with CC0 1.0 Universal
 * <https://creativecommons.org/publicdomain/zero/1.0/?ref=chooser-v1> */

/* [optional GNU style changelog would go here] */

/* [start code here] */
```

All files should also end with a comment documenting that the end-of-file has been reached, such as a simple

``` C
/* end of file */
```

#### Header Files

Directly following the universal file boilerplate, header files, `*.h`, must begin with a header guard as follows

``` C
#ifndef FILENAME_HEADER
#define FILENAME_HEADER

/* [code goes here] */

#endif /* header guard */
```

note: do not use `#pragma once`

#### C Source Files

With few exceptions, every C source file should have an acompanying header file (by the same name). This header file should be included before any other includes or code takes place.

### Include Statements

Includes should be alphabetized to make for easy scanning. The bulk of includes should be done within the C source files, NOT within header files. Header files should contiain only the necesary includes inorder to compile and utilize the interface. Header file's includes often only serve to provide propper typedefs, defines, and enums to use the interface. 

An example include list may look as follows

``` C
#include <assert.h>
#include "internel_header.h"
#include <limits.h>
#include <malloc.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "string_utils.h"
#include "utils.h"
```

### Preproccessor

#### Macros

#### Constants

### Comments

### Functions

### Methods

### Variables

### Pointers

### Conditionals

#### if elseif else

Single line `if` statments are allowed, in such cases, the curly braces are optional and the resulting code must be inline with the keyword and conditional as in the following 2 cases

``` C
if (variable_a % 2 == 0) printf ("variable_a is even!\n");
if (variable_a % 2 == 0) { printf ("variable_a is even!\n"); }
``` 

All other if statements required to follow the [standard curly brace](#formatting) syntaxing, as documented previously, and as subsequently shown in the following
``` C
if (variable_a % 2 == 0)
{
    printf ("variable_a is even!\n");
}
else if (variable_a % 2 == 1)
{
    printf ("variable_a is odd!\n");
}
else
{
    fprintf (stderr, "we have bigger issues at hand, oh no\n");
    abort ();
}
```


#### switch base default

### Loops

#### while dowhile

#### for

### Objects

#### typedefs

#### structs

#### enums

## License

[SoftFauna C Style Guide](https://github.com/SoftFauna/softfauna-style-guide.git) by [The SoftFauna Team](https://github.com/SoftFauna) is marked with [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/?ref=chooser-v1)  
<https://creativecommons.org/publicdomain/zero/1.0/?ref=chooser-v1>
