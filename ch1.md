# 1.1
Some important escape sequences:
- \n: newline
- \t: tab
- \b: backspace
- \": double quote
- \\: backslash

# 1.2
Can declare multiple variables on the same line. Ex:
`int fahr, celsius;`

Bulit-in data types:
- char: character (single byte)
- short: short integer
- long: long integer
- double: double-precision floating point
- float: regular-precision (?) floating point

Integer division truncates; any fractional part is discarded. That's why you don't do  `celsius = (5/9) * (fahr - 32)` because `(5/9)` would be truncated to 0, and so celsius would always be multiplying by 0.

To right-justify, you can augment the `%` with a width. For instance, you might say:
`printf(""3d "6d\n", fahr, celsius);`

The above code prints the first number right-justified on a line 3 digits wide, and the second field on a line 6 digits wide.

If you operate on an int and a float, the integer is converted to a float and then floating point arithmetic is done.

List of percent things:
- "d print as decimal integer
- "6d print as decimal integer, at least 6 characters wide print as floating point
- "6f print as floating point, at least 6 characters wide
- ".2f print as floating point, 2characters after decimal point
- "6.2f print as floating point, at least 6wide and 2after decimal point

# 1.3: Skipped because for loop
# 1.4
Can define constants with `#define NAME VALUE`, which replaces all instances of NAME in code with VALUE at compile time. 
# 1.5
Regardless of source, text I/O is dealt with as streams of characters. A **text stream** is a sequence of characters divided into lines. Each line has >= 0 chars plus a newline.

You can use `getchar()` and `putchar(c)` for stdin and stdout.

You should generally use int for getchar() because EOF is -1, and char isn't big enough for that. EOF is defined in <stdio.h>

You can keep an assignment inside a larger expression, like so:

`int c; while ((c = getchar()) != EOF) putchar(c);`

Precedence of != is greater than =.

You can set multiple variables to the same value:
`n1 = nw = nc = 0;` which is equal to `n1 = (nw = (nc = 0));`.

&& and || are the same precedence. They are evaluated left to right. Once an expression is true, the rest of the statement is not evaluated.

# 1.6: Arrays
if, else if, and else: stop checking the rest of the conditionals once one is true. You can also use a switch statmenet.

# 1.7: Functions
Parameter is the variable used in the parenthesis, and argument is the value used in the call of the function (within).

In the function prototype, you don't have to use the same parameter names. Instead of writing `int power(int m, int n);` you can just declare `int power(int, int);`

# 1.8: Arguments
In C, **all function arguments are passed by value**. This means that the function gets temporary copies of the parameters, meaning that you can't directly alter the variables provided as parameters. You can treat parameters as local variables without worrying about the outside.

For example:

```c
/*power: raise base to n-th power; n>=O; version2*/ 

int power(int base, int n)
{
    int p;
    for(p=1;n> 0;--n) p = p * base;
    return p;
}
```

Notice how the parameter n is being modified above; we dont have to worry about the value being passed in to n being modified because we have a copy.

If you want to modify the original stuff, you pass it the address and then dereference it.

# 1.9: Character arrays
If you don't want to return anything use `void` as the return type.

All strings have the null character (whose ASCII value is 0) at the end. This lets the OS know you're done. So, "hello" is really h-e-l-l-o-\0.

# 1.10: External variables and scope
Local variables that exist within functions come into existance when the function is called and then discarded afterward. If you try to read their values after, they will contain garbage. These variables are known as **automatic** variables. You can change this behavior by using the `static` storage class (see ch4).

You can define variables external to all functions (globally accessible). This is bad practice, but you can do it. 

You declare an external variable once outside of any function (please do it at the top) and then declare it again in each function that wants to access it.

```c
/* SAMPLE PROGRAM WITH EXTERN */

#include <stdio.h>

#define MAXLINE 100

int max;
char line[MAXLINE];
char longest[MAXLINE];

int getline(void);
void copy(void);

main() {

    int len; /* local to main */
    extern int max; /* So that compiler knows you're using a global var */
    extern char longest[]; /* Same as above */

    /* The rest of your code...do above in every other function that uses the global vars. */
}
```

If the definition of an external variable occurs before use in a particular function, you don't need the keyword `extern`. So the above function main did not technically need the keyword since the variables were declared above. Had they been declared below, you'd have needed them.

If you want to use variables across files, you definitely need `extern`, and you probably want to put that in the header file.

