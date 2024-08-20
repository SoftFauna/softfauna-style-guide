
# Index

- [Index](#index)
- [Documentation](#documentation)
  - [Basic Naming Conventions:](#basic-naming-conventions)
  - [Files](#files)
  - [Includes](#includes)
  - [Functions](#functions)
  - [Macros](#macros)
  - [Variables](#variables)
  - [Whitespace](#whitespace)
  - [Braces](#braces)
  - [Comments](#comments)
  - [Conditionals](#conditionals)
    - [if](#if)
    - [switch](#switch)
    - [ternary](#ternary)
  - [Loops](#loops)
    - [for](#for)
    - [while](#while)
  - [Objects](#objects)
  - [Pointers](#pointers)
  - [Saftey](#saftey)
  - [Miscelaneous](#miscelaneous)
- [Licensing](#licensing)


# Documentation

## Basic Naming Conventions:

| | |
|:------------------- |:------------------------- |
| all extern          | `[namespace]_` prefix     |
| variable (static)   | `g_` prefix               |
| variable (local)    | mut or const?             |
| variable (mutable)  | `snake_case`              |
| variable (constant) | `SCREAMING_SNAKE_CASE`    |
| preproc define      | `SCREAMING_SNAKE_CASE`    |
| function            | `snake_case ()`           |
| method              | `[object]_` prefix        |
| macro               | `SCREAMING_SNAKE_CASE()`  |
| macro (unsafe)      | `_UNSAFE` suffix          |
| typedef             | `snake_case`; `_t` suffix |
| struct              | `snake_case`              |
| enum                | `snake_case`              |
| enum constant       | `SCREAMING_SNAKE_CASE`    |

[top](#index)

## Files

- the first line of any given file must contain a breif summary of what use 
  the file serves
- all header files must contain a preproccessor `#ifndef` header guard (no
  using `#pragma`)
- all files should end with an "end-of-file" comment 
- segregate the C source file's personal header file from the rest of the 
  includes.

``` C 
/* optional project name + url */
/* example.c: this is an example source code file for documenting my style
 * guidlines */

/* Licensing/Copyright Notice */

/* optionally change log */

#include "example.h"

/* code */
/* ... */

/* end-of-file */
```

``` C 
/* optionally project name + url */
/* example.h: this is an examle header file for documenting my style 
 * guidlines */

/* Licensing/Copyright Notice */

/* optionally change log */

#ifndef EXAMPLE_HEADER
#define EXAMPLE_HEADER

/* code */
/* ... */

#endif /* header guard */
/* end-of-file */
```

[top](#index)

## Includes

- the include list should always be alphabetized
- includes should be primarily made within C files; header files should 
  include only the bare necessities for interacting with the module.
- include the linked (personal) header file(s) seperate from the dep includes

``` C 
#include "example.h"

#include <assert.h>
#include <limits.h>
#include <malloc.h>
#include "my_header_file.h"
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
```

[top](#index)

## Functions

- use void for funtions who require no params
- provide comment documenting a brief summary of what each funtion does on 
  the line directly preceeding the function definition
- funtion definitions should have the return type and name on seperate lines
- never use K&R style parameters in Function definitions
- make active use of the 'static' keyword.
- Function Prototypes should have the return type, name, and parameters 
  inlined.
- Funtions must always have a space between the name and parameter list
- Function return values must ALWAYS either be checked, or explicitly be cast 
  to void. with the exception of functions who output data to the terminal, 
  for example printf
- all global/extern functions should be prefixed by a shared module name.

``` C 
/* goods */
void example_functin_a (void);
int example_function_b (void);
static int function_c (int a, int b, long c);
static int function_d (int a, int b, long c);

/* does function_c kind of stuffs */
static int
function_c (int a, int b, long c)
{
    /* ... */
}

/* ... */


/* bads */
void example_function_a();
function_b (void);
static int function_c (int, int, long);
static int function_d (int a, int b, long c);


/* functin a summary */
void example_function_a ()
{
    /* missing void in param list */
    /* ... */
}


/* functin b summary */
function_b (void)
{
    /* implicit int and missing module prefix or static */
    /* ... */
}


/* function c summary */
static int
function_c (a, b, c)
int a, b;
long c;
{
    /* k&r style parameters gross */
    /* ... */
}


static int
function_d (int a, int b, long c)
{
    /* missing the funtion summary */
    /* ... */
}
```

[top](#index)

## Macros

- if a macro access a _variable_ more than once times it must be marked with 
  the suffix `_UNSAFE` as per SEI Cert C.
- generally macros should be avoided; use inline functions instead where ever 
  possible
- utilize `_Generic` when applicable

``` C 
/* good */
#define MACRO_DOUBLE_UNSAFE(a) ((a) + (a))

/* better */
static inline int
inline_double (int a)
{
    return a + a;
}

/* note: that if we call the macro with MACRO_DOUBLE_UNSAFE(variable++), 
 * it will be incremented twice and return the wrong result. the inline
 * example does not suffer this fate and subsequently is prefered.
 */
```

[top](#index)

## Variables

- mutable variables should be listed as per C89 standard, at the top of any 
  given scope
- constants may be defined whenever is most convienient (C99 style)
- global and static variables must be initialized within an init funtion. 
  They must NOT be defined by default.
- all extern variables must be prefixed by a common module name.

``` C 
void
example_function (int input_a, int input_b)
{
    int var_c, var_d;

    var_c = input_a + input_b;
    var_d = input_a - input_b;
    const int VAR_E = input_a * input_b;
    const int VAR_F = input_a / input_b;

    for (int i = VAR_E + 1; i > VAR_F; i--)
    {
        const int RESULT = var_c + var_d;
        printf ("%d + %d = %d\n", var_c--, var_d--, RESULT);
    }

    return;
}
```

[top](#index)

## Whitespace

- use 4 spaces, never use tab characters
- there must never be more then 2 empty lines next to one another
- all arithmatic, logic, and comparison characters must be padded either side 
  by a singular space
- if a line must wrap, for any reason, neatly align it with the context of the 
  previous line
- all function calls, definitins, and protoypes must have a 1 space of 
  badding between the name and parenthesis
- all keywords must carry 1 space of padding directly after

``` C 
/* good */
if ((a > 2) ||
    ((a + 1) == 4) &&
    (b++ < 100) &&
    (a << b) != 0)
{
    function_call ();
    /* ... */
}

/* bad */
if((a>2)||
  ((a+1)==4)&&
  (b++<100)&&
  (a<<b)!=0)
{
  function_call();
  /* ... */
}
```

[top](#index)

## Braces

- braces should always be on a seperate line (`allman`/`bsd` style)
- comments are permitted to be on the same line as a brace

``` C 
/* good */
void func (void)
{
    if (true)
    {
        /* ... */
    }

    /* ... */
}
```

[top](#index)

## Comments 

- only use C style comments, `/* ... */`; C++ style comments, `// ...` are 
  never permitted. 
- comments may be provided on seperate line, after the `;`, or following a 
  `{`, where ever is most convient at the time.
- recomended to add a comment following closing braces for clarity, optional

``` C 
/* good */

/* single line comment */
int a = 5;  /* comment after semi-colon */
if (true)
{   /* comment on brace */
    /* ... */

}   /* end if (true) */
            

/* bad */
// comment
```
[top](#index)

## Conditionals

### if 

- may be single lined (curly braces optional, recomended) 
- when not single lined, curly braces must always be present
- must never be on the same line as a curly brace

``` C 
/* good */
if (/* conditional */) printf ("Hello!\n");
if (/* conditional */) { printf ("Hello!\n"); }

if (/* contitional */)
{
    /* ... */
}
else if (/* contitional */)
{
    /* ... */
}
else
{
    /* ... */
}

/* BAD */
if (/* conditional */)
    printf ("Hello!\n");
```

[top](#index)

### switch 

- case and default statments must begin on the same line as the switch 
  keyword.
- each case should end with a break statement, unreachable or not.

``` C 
typedef enum 
{
  STATE_ERROR,
  STATE_RUNNING,
  STATE_HALTED,
  STATE_CLOSING,
} my_state_t;

switch (current_state)
{
case STATE_ERROR:
    /* ... */
    return NULL;
    break;

case STATE_RUNNING:
case STATE_HALTED:
case STATE_CLOSING:
    /* ... */
    break;

default:
    /* ... */
    exit (EXIT_FAILURE);
    break;
}
```

[top](#index)

### ternary

- should be avoided when ever possible
- only permittable as variable definitions and in function parameters
- NEVER for use in control flow, only applicable for simple assignments;

``` C 
/* fine/good */
char iseven_string = ((variable % 2 == 0) ? "true" : "false");
printf ("is even: %s", ((variable % 2 == 0) ? "true" : "false"));

/* bad */ /* dificult to read and maintain, just use an if statement */
(variable % 2 == 0) ? print_even () : print_odd ();
```
[top](#index)

## Loops

### for
- are permitted to use `i`, `j`, and `k` as temporary incrementors. defined in c99 
  style.

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

[top](#index)

### while

- the while statement cannot be on the same line as the ending curly brace
- curly braces are mandatory, even if no code is provided within

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

[top](#index)

## Objects

- enums do not require a name
- structures only require a name if it points to a itself
- typedefs always require a name
- structures should be organized to minimize wasted memory
- state enums should always contain an error state value, this value should 
  be the first item of the enum
- should be prefixed by a common module name

``` C 
typedef enum 
{
  STATE_ERROR,
  STATE_RUNNING,
  STATE_HALTED,
  STATE_CLOSING,
} my_state_t;

typedef struct my_node
{
    struct my_node *next_node;
    my_bcd_t m;
} my_node_t;

typedef struct
{
    size_t alloc, count;
    char *d;
} my_bcd_t;
```

[top](#index)

## Pointers

- place the star, `*`, next to the name, NOT by the type
- always take care to free memory, with the exception of if we are exitting 
  the application right away.
- pay attention to weather a ptr is `restrict` or not

``` C 
char *str1;           /* good */
char* str1;           /* bad */
char * str1;          /* bad */
```

[top](#index)

## Saftey

SEI Cert C should be loosely followed, and undefined/unspecified behavior 
should not be used. This is optional and varies project to project.
- do some basic checks like for mathematical over+underflows ect.

``` C 
/* example */
typedef struct 
{
    /* ... */
} my_object;

/* a tainted value is any data that is variable between runtimes. Any data 
 * the user can interact with is considered tainted. */

size_t objarr_length = /* tainted value */;
my_object *object_array = NULL;

if ((SIZE_MAX - 1 <) ||
    (SIZE_MAX / sizeof (my_object) < objarr_length))
{
    /* handle ERANGE error */
}

object_array = (my_object *)malloc (sizeof (my_object) * objarr_length);
if (NULL == object_array)
{
    /* handle ENOMEM error */
}

/* ... */

free (object_array);
object_array = NULL;
```

[top](#index)

## Miscelaneous

- never use implicit int (K&R style)
- "implicit int" is only permitted when a more specific typing 
  `long`/`long long`/`unsigned` is available

``` C
/* good */
static int function_a (int a, long b);

/* bad */
static function_a (a, long b);
```

[top](#index)

# Licensing

