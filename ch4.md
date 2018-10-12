# CHAPTER 4: FUNCTIONS AND PROGRAM STRUCTURE
Use functions.
You don't need to declare the types of arguments when a function is defined, but you really should.

# 4.1: Basics of Functions
Bruh you should know this.

A minimal viable function is
```c
dummy() {}
```
which does nothing and returns nothing. These functions are good as placeholders. If you don't give it a return type (like above), **it is assumed to be int, not void**.

You don't need parenthesis around return expressions, but they're used often.

When you compile the 3 files `main`, `getline`, and `strindex` with `gcc main.c getline.c strindex.c`, you compile each of those files into object files, called `main.o`, `getline.o`, and `strindex.o` respectively.

These object files are linked into one executable image, which if you don't specify a name, will be called `a.out`.

If one file messed up, but the other object files were executed, you can fix the error and recompile like so (notice main):

`gcc main.c getline.o strindex.o`

# 4.2: Functions Returning Non-Integers
If there is no function prototype, a function is implicitly declared by its first appearance in an expression, such as:
```c
/* atof converts a string to its double-precision floating-point equivalent */
sum += atof(line);
``` 

If you don't provide arguments in the function declaration, as in
```c
double atof();
```
then the compiler does not check parameters. This is to allow older C code to compile with new standards, but it's a bad idea to do this now. If a function takes no arguments, explicitly say `void`. 

**Unlike other languages, lack of arguments and `void` are not the same thing.**

# 4.3: External Variables
C programs have external variables and functions, along with internal variables inside functions. There is no such thing as an internal function in C, because C does not allow functions to be declared inside other functions.

External variables are useful because of their scope and lifetime, but they reduce modularity and are bad for unit testing.

# 4.4: Scope Rules
A **declaration** announces the properties of a variable (primarily its type), but a **definition** actually causes storage to be set aside.

If the lines
```c
int sp;
double val[MAXVAL];
```
appear outside of any function, they *define* the external variables `sp` and `val`, and cause storage to be set aside. 

However, if you prepend with `extern`, then you simply *declare* the variables and storage is not set aside.

# 4.5: Header Files
Let's assume you have a calculator program split into multiple files: `main.c` as a driver program, stack operations like `push()` and `pop()` go in `stack.c`, and a character reading file called `getch.c`.

These files are all part of a larger program, but you keep them separate for their different functionality. Each of these files share declarations, and you want to centralize this to reduce maintainance.

The centralized file is called a **header file**, which we may call `calc.h`. Now, in each file, you can just add the following line at the top:
```c
#include "calc.h"
```
For bigger projects, you might want to have multiple header files (perhaps one for unit tests?), but for now, one is enough.

# 4.6: Static Variables
If you import another file, you have access to all of its functions and external variables. However, if you want to keep things only accessible within the sourcefile (like `private` in Java or `fileprivate` in Swift), you can prepend with `static`.

So, now if you're using global (external) variables within the source file, and you want the functions to be usable outside, but you don't want the variables to be seen, do this:

```c
static char buf[BUFF_SIZE]; /* Visible within source file, not outside. */
static int bufp = 0; /* Still only visible within source file */

int foo(void) {} /* External, visible to any other file */

void bar(void) {} /* Uses static vars, but again, the function is visible outside. */
```

If you've seen any Java programming, this might be the equialent:

```Java
private String buf;
private int bufp = 0;

public int foo() {}

public void bar() {}
```

# 4.7: Register Variables
Hopefully you've taken some sort of computer architecture class. If not, the Processing Unit keeps a few **registers** nearby so it doesn't have to go aaaallll the way to memory (RAM). This is all at the nano-level, but distance still matters, and accessing registers is hundreds or thousands of times faster than accessing memory.

Declaring a variable with `register` *suggests* that the variable should be placed in a register because it will be heavily used. The compiler is free to ignore you if it thinks that's stupid.

You can only apply this declaration to *automatic variables and to formal parameters of a function*. In other words, no register globals. Ex:

```c
void foo(register unsigned int a, register unsigned int b) {
    register int i;
    ...
}
```

While this is fast, it comes with restrictions. For example, you can't get the address of a register variable because it might not even be in memory. This is rarely used.

# 4.8: Block Structure
Variables exist within the scope of their nearest block. So, if you declare a variable in an if-statement, the variable will only be accessible during the execution of that if statemnet.

Automatic variables are initialized and uninitialized upon entrance and exit of the block. Static variables are initialized only the first time and then stay available.

If you have matching variable names (which you shouldn't), then the compiler looks at the one with nearest scope. Ex:

```c
int x = 0;
int y = 1;

void foo(void) {
    double y = 5;
    printf("The value of y is %d, y); /* Prints 5, not 1. */
}
```

# 4.9: Initialization
Before initializing, external and static variables have a value of 0, and automatic and register variables hold garbage.

Because external and static variables are somewhat independent of context, their initializations must be constant expressions (e.g. they don't depend on other variables, only take literal values like numbers and characters).

Automatic and register variables can be initialized using other variables.

If you explicitly define an array, you don't have to declare its length:

```c
/* Length of days is inferred to be 12 */
int days[] = {31, 28. 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
```

If you give fewer initializer values than the size you specified, the missing elements will be 0. You're not allowed to have more initializers than specified size. You can't repeat elements and leave elements in the middle blank.

Character arrays are special, because instead of an array of single characters, you can just give it a string. The following two declarations are equivalent:

```c
char pattern[] = "ould";
char pattern[] = { 'o', 'u', 'l', 'd', '\0' };
```

# 4.10: Recursion
Functions may call themselves repeatedly. Each invocation gets a fresh set of all the automatic variables.

Recursion isn't any better in terms of memory than iterative solutions since you have to keep a new stack frame for every recursive call, and it's not faster. Recursion is all about convenience and readabiilty.

# 4.11: The C Preprocessor
Recall that the first step in a compiled language is, well, compilation. A preprocessor goes through and ..."understands" the code to put it together with other libraries and stuff, you get the gist of it. For more detail take a Computer Architecture class.

The two most frequently used preprocessor commands are:
    - `#include`: Includes the contents of another file during compilation (usually header files)
    - `#define`: Replaces a "variable" (not really a variable) with a predefined expression. Kind of like a hardcoded variable--the preprocessor basically finds wherever you used the name and replaces it with the hardcoded value.

## 4.11.1: File Inclusion
Any line in a source file that looks like `#include "filename"` or `#include <filename>` is replaced with the entire contents of the specified file.

If the filename is in quotes, then the system looks near the file you're already in. If there are no quotes or the filename is in `<>`, then the OS takes care of it (they're really vague here).

Most C source files have a bunch of `#include` directives from common libraries and header files, and a bunch of `#define` directives for common values within the code.

When an included file is changed, all files that depend on that must also be recompiled.

## 4.11.2: Macro Substituion
Looks like `#define name replacement`

All subsequent occurrances of `name` will be replaced with `replacement`. It's like hardcoding without actually hardcoding! The scope of these is file-wide.

You can redefine...anything:
```c
#define forever for (;;) /* Infinite loop */

#define max(A, B) ((A) > (B) ? (A) : (B))

#define if else /* please don't do this :( */
```

Macros come with pitfalls. For example, the replacement for
```c
x = max(p + q, r + s);
```
would be
```c
x = ((p + q) > (r + s) ? (p + q) : (r + s));
```

That is fine, but what if you had this:
```c
x = max(i++, j++);
```

The increment would happen twice, because you would *really* have this code:
```c
x = ((i = i + 1) < (j = j + 1) ? (i = i + 1) : (j = j + 1)) /* i and j are both 2 greater instead of 1 greater */
```

Still, for "safe" functions like `getchar()` (which I think is really a wrapper around `getc(stdin)`) and `putchar(c)`, you're ok using macros. In fact, most functions in `<ctype.h>` are implemented as macros.

You can undefine names with #undef. This is usually done to ensure that a routine is actually a function and not a macro:

```c
#undef getchar

int getchar(void) {}
```

There's a lot of extra stuff in here that I don't think we need. If you care, feel free to read.

## 4.11.3: Conditional Inclusion
You can control preprocessing itself with conditional statements. This is usually a way to ensure code doesn't compile twice (same header file everywhere).

You can do this:
```c
#if !defined(HDR)
#define HDR

/* Put the contents of hdr.h here */

#endif
```

The first line is usually shortened to `#ifndef HDR`.

You can also use these to figure out what system you're on to decide which version of a header to include:

```c
#if SYSTEM == SYSV
    #define HDR "sysv.h"
#elif SYSTEM == BSD
    #define HDR "bsd.h"
...
#else
    #define HDR "default.h"
#endif
#include HDR

