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
    - [structs](#structs)
    - [enums](#enums)
- [License](#license)

## Documentation

---

### Naming Conventions


`snake_case` may be used for functions, methods, mutable variables, typedefs,
structs, and enums.  
While `SCREAMING_SNAKE_CASE` is for macros, defines, constant variables, and
enum constants.

| | |
|:------------------- |:------------------------- |
| extern/header item  | `[namespace]_` prefix     |
| static variable     | `g_` prefix               |
| method              | `[object]_` prefix        |
| typedef             | `_t` suffix               |
| unsafe macro        | `_UNSAFE` suffix          |

[back to index](#index)

---

### Formatting

Limit lines to always be less than 100 columns, anything more and some editors
require scrolling.

Use 4-space indentation, no tab characters.

Curly braces are (*almost*) always on a separate line, `Allman`/`BSD` style.

All keywords which accept parenthesis, are required to have 1 space of padding
between itself and the parenthesis. This non-exclusive list contains: `if`, 
`else if`, `for`, and `while` expressions. Further, `#include` follows 
identically with their alligators and double quotes.

There should be 1 padding space between the function name and the parameter 
list for all function definitions, prototypes, and calls. However, when 
dealing with macros this padding space is optional.

[back to index](#index)

---

### File Boilerplate

Within the first page (24 lines) of a file, the project's name, URL, the
file's purpose, and the project's basic licensing information must 
be visible. each file should fit the following format

``` C
/* SoftFauna Style Guide - C Lang
 * <https://github.com/SoftFauna/softfauna-style-guide.git>
 *
 * This file exists as an example of how all SoftFauna file boilerplate 
 * should look */

/* SoftFauna C Style Guide is marked with CC0 1.0 Universal
 * <https://creativecommons.org/publicdomain/zero/1.0/?ref=chooser-v1> */

/* [optional GNU style changelog would go here] */

/* [start code here] */
```

All files should also end with a comment documenting that the end-of-file has 
been reached, such as a simple

``` C
/* end of file */
```

#### Header Files

Directly following the universal file boilerplate, header files, `*.h`, must 
begin with a header guard as follows

``` C
#ifndef FILENAME_HEADER
#define FILENAME_HEADER

/* [code goes here] */

#endif /* header guard */
```

note: do not use `#pragma once`

#### C Source Files

With few exceptions, every C source file should have an accompanying header 
file (by the same name). This header file should be included before any other 
inclusions or code takes place.

[back to index](#index)

---

### Include Statements

The includes should be alphabetized to make it easy to scan. The bulk of 
includes should be done within the C source files, NOT within header files. 
Header files should contain only the necessary includes to compile and utilize 
the interface. Header files' includes often only serve to provide proper 
typedefs, defines, and enums to use the interface. 

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

[back to index](#index)

---

### Preproccessor

#### Macros

Any macro that makes multiple uncontrolled accesses to the same _parameter_ 
must be marked as `_UNSAFE` following SEI Cert C. 

``` C
#define DOUBLE_UNSAFE(a) ((a) + (a))
```

in the declaration, macros must NOT have any padding space between the name 
and param list, however, in calling and use an optional padding space is 
permitted.

When possible macros should be avoided in favor of inline functions. for 
example instead of using 

``` C
#define MACRO_ADD(a, b) ((a) + (b))
```

opt for an inline approach as follows

``` C
static inline int
add_int (int a, int b)
{
    return a + b;
}
```

To extend the ease of use, when applicable the C11 selector `_Generic` may be 
used for preproc abstractions. such that the following is possible

``` C
static inline int add_int (int a, int b);
static inline long add_long (long a, long b);

#define BETTER_MACRO_ADD(a, b) _Generic ((a), \
    int:  add_int, \
    long: add_long \
)((a), (b))
```

#### Constants

Preprocessor constants are great for limiting _magic numbers_, preferably they 
should be defined near the top of the given file (c or header) for easy access.

Though for many cases Enums may be preferable instead.

note: there are some occasions where contents may use snake_case, such as 
pseudo-typedefs/pseudo-qualifiers and the like, generally any scenario where 
the definition does nothing to affect the code and which is only used to 
improve general readability may this exception be used.

``` C
/* expands to nothing, does nothing to affect the code, but adds additional 
 * clarity (or clutter depending on the individual) */
#define mutable

static mutable char *g_example_str = NULL;
static const char *G_CONSTANT_STR = "Hello";
```

[back to index](#index)

---

### Comments

Comments are to strictly use C style comments, never should C++ style comments 
be used. Multi-line C-style comments should be prefixed by a ` * ` for each 
subsequent new line.

the closing brace, ` */`, should be inline with the comment text whenever 
columns permit.

Comments may be made practically anywhere within the code, on the preceding 
line, following the `;`, or even following curly braces, `{` and `}`. 

``` C
/* this is a single-line comment */

/* this is
 * a multi
 * line
 * comment */

/* explain what MAGIC_NUMBER_A does */
const int MAGIC_NUMBER_A;

const int MAGIC_NUMBER_B;     /* explain what MAGIC_NUMBER_B does */

if (true)
{   /* this block of code will always occur and does [...] stuffs 
     * using MAGIC_NUMBER_A and MAGIC_NUMBER_B */
    /* [code] */

}   /* end if [human readable conditinal] */
```

comments should be relatively short; general code comments are to be all 
lowercase. Variables and other code definitions should retain their casing, 
for example, MAGIC_NUMBER_A in the above code block.

[back to index](#index)

---

### Functions

functions that take no parameters must be listed as taking a `void` parameter, 
to avoid K&R-style function prototypes.

function prototypes should have all elements inlined; the parameter list may 
be split across several lines if the max line length is surpassed.

function definitions are required to list the return type on a separate line 
from the function name.

functions should aim to produce as few side effects as is reasonable.

the static keyword is wonderful and should be used liberally whenever it is 
applicable!

function returns must always either be checked, stored, or explicitly cast to 
`void`. This applies to all function calls except cases where the function's 
main goal is to interface with `stdout` or `stderr`.

preferably, either at the definition or prototyping (depending on the linter) 
it may be beneficial to provide a brief comment documenting details of the 
function and its purpose.

Additionally, it is preferable to make avid use of guard statements within 
functions

functions should also use errno.

[back to index](#index)

---

### Methods

methods follow all the same rules as functions, except that they are required 
to be prefixed with the object on whom they operate, and if they require said 
object as a parameter it should be passed by reference as the first 
parameter, either name `self`, `s`, or `p` given the scenario.

``` C
/* defined in header */
typedef struct node_int
{
    int m;
    struct node_int *next;
} node_int_t;

/* returns a reference to the next node in the linked list.
 * if end-of-list is reached, NULL is returned.
 * on an error, returns NULL and sets errno. */
node_int_t *nodei_get_next (node_int_t *self);

/* defined in c source */
node_int_t *
nodei_get_next (node_int_t *self)
{
    if (NULL == self)
    {
        errno = EINVAL;
        return NULL;
    }

    return self->next;
}
```

[back to index](#index)

---

### Variables

static and extern mutable variables must not contain default values at 
declaration, instead, they should be initialized within an `init ()` 
function. static/extern pointers may be set to NULL by default for safety.

constant variables on the other hand must be initialized with default values, 
and as such do not require any such initialization function.

local mutable variables must be defined similarly to C89 style variables at 
the beginning of the active scope, be it: function, while, for, closure, or 
otherwise.

local constant variables on the other hand may be defined as needed, using 
C99 style.

also, please never use implicit `int` typing for variables or functions alike.

``` C
/* note: this whole example is riddled with bad practices and security 
 * vulnerabilities, it is only to demonstrate variables */
static const size_t G_BUF_LEN = 100;
static char *g_buf = NULL;
static int buf_index;

void
example_init (void)
{
    g_buf = (char *)malloc (G_BUF_LEN);
    g_buf_index = 0;
    return;
}

void
example_quit (void)
{
    free (g_buf); g_buf = NULL;
    g_buf_index = 0;
    return;
}

static void
example_function (void)
{
    int a = 1;
    int b = 2;

    for (int i = 0; i < g_buf_index; i++)
    {
        char c = g_buf[i];
        char d = g_buf[i + 1];
        printf ("%c %c\n", c, d);

        const char CD = c + d;
        printf ("%d\n", (int)(unsigned char)CD);
    }

    const int AB = a + b;
    printf ("%d", AB);

    return;
}
```

[back to index](#index)

---

### Pointers

with very few exceptions, pointers should default to `NULL`, only updated once
 valid data is provided to them. before dereferencing a pointer it should be 
 checked that it isn't `NULL`, often, this is done at the beginning of a 
 function using a guard clause.

the `*` should be aligned with the name, NOT with the type.

``` C
static char *g_pointer_a = NULL;

static void
function_example (void)
{
    char character;

    if (NULL == g_pointer_a)
    {
        errno = EINVAL;
        return;
    }

    character = g_pointer_a[0];

    /* [more code] */
}
```

After freeing any pointers, the variable should be explicitly set to `NULL` to
avoid a potential use-after-free violation (assuming that NULL guard clauses 
are in widespread use).

[back to index](#index)

---

### Conditionals

#### if elseif else

Single line `if` statements are allowed, in such cases, the curly braces are 
optional and the resulting code must be inline with the keyword and 
conditional as in the following 2 cases

``` C
if (variable_a % 2 == 0) printf ("variable_a is even!\n");
if (variable_a % 2 == 0) { printf ("variable_a is even!\n"); }
``` 

All other if statements are required to follow the 
[standard curly brace](#formatting) syntax, as documented previously, and as 
subsequently shown in the following

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

Additionally, when comparing the equality of a variable to an implicit value, 
place the implicit value first, as to avoid accidental single equal 
assignments.

``` C
if (NULL == variable_a)
{
    /* ... */
}
```

#### switch base default

switch statements must align the `switch`, `case`, and `default` keywords 
along the same column as the braces.

no code may be inserted outside the bounds of execution, such as, but not 
limited to, after a break or before the first condition.

all conditions must end with either a `break`, a `return`, or a 
`_Noreturn function`. however, fall-through cases are allowed. the end of a 
given conditional block must be followed by a newline for padding.

``` C
typedef enum {
    STATE_ERROR,
    STATE_ACTIVE,
    STATE_DISABLED,
    STATE_BUSY,
    STATE_HALT,
} state_t;

switch ((state_t)value)
{
case STATE_ACTIVE:
    break;             /* break */

case STATE_DISABLED:
case STATE_HALT:
case STATE_BUSY:
    return NULL;       /* return */

case STATE_ERROR:
default:
    abort ();          /* _Noreturn */
}
```

[back to index](#index)

---

### Loops

loops should favor iteration and incrementation over recursion. recursion is 
permitted, however, it should not be the default behavior.

#### while dowhile

while loops that rely on side effects in the conditional are required to still
 provide curly braces, however, they should be inlined.

more commonly, the while statement should be on a separate line from the curly
 braces, this is always the case for dowhile loops.

``` C 
while (/* side effect */) {}

while (true)
{
    /* ... */
}

do
{
    /* ... */
}
while (true);
```

#### for

loops are permitted to go a maximum of 3 layers deep (anything more and you 
may need to reevaluate some choices). the base increment/counter for loop 
variables are `i`, `j`, and `k`.

the incrementor may be declared within the scope of the statement or at the 
beginning of the scope, depending on the intended behavior

``` C 
for (int i = 0; i < 10; i++)
{
    for (int j = 0; j < 10; j++)
    {
        for (int k = 0; k < 10; k++)
        {
            /* ... */
        }
    }
}
```

[back to index](#index)

---

### Objects

objects should (_almost_) always be part of a typedef.

#### structs

structures must list the internal fields such that as little space is wasted
on padding as is reasonable, additionally it is recommended that they begin
with the largest datatype. alternatively, less ideal but easier, structures 
may be ordered by size, with the largest fields ordered first and the 
smallest last. (assuming modern values, x86/x86_64, or specifics for the 
intended system)

an exception to the above is variable length arrays, which are required to 
be the last element in the struct by definition.

structures may use _headers_ to implement pseudo-private/public fields. (note 
this is not shown in the example)

``` C 
typedef struct dynamic_array
{
    struct dynamic_array *self;    /* 8/4 byte */
    size_t elem_size;              /* 8/4 byte */
    size_t alloc_count;            /* 8/4 byte */
    size_t count;                  /* 8/4 byte */
    uint32_t uid;                  /* 4 byte */
    unsigned char m[];             /* variable length array */
} dynamic_array_t;

typedef struct
{
    float x;
    float y;
    float z;
} vec3;
```

#### enums

enums should be used as a collection of related constant values

if an enum needs to be named (outside of a typedef) it must use snakecase. all
 internal constants must follow the same casing as constants and be prefixed 
 with a descriptor of the enum.

``` C 
typedef enum
{
    STATE_ERROR,
    STATE_RUNNING,
    STATE_HALTED,
    STATE_CLOSING,
} my_state_t;

typedef enum named_enum
{
    /* ... */
} named_enum_t
```

[back to index](#index)

---

## License

[SoftFauna C Style Guide](https://github.com/SoftFauna/softfauna-style-guide.git) by [The SoftFauna Team](https://github.com/SoftFauna) is marked with [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/?ref=chooser-v1)  
<https://creativecommons.org/publicdomain/zero/1.0/?ref=chooser-v1>
