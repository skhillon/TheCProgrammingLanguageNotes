# CHAPTER 4: FUNCTIONS AND PROGRAM STRUCTURE
Use functions cuz they're gr9 and you can trust that it does what it does without worrying about how hashtag modularity.

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

