# CHAPTER 2: TYPES, OPERATORS, AND EXPRESSIONS
There are now `signed` and `unsigned` forms of all integer types. Floating-point operations can be done in single precision, and if you need extended precision there is `long double`.

You can concatenate string constants at compile-time.

There is now the `const` keyword, which prevents variables from being changed (like `final` in Java).

# 2.1: Variable Names
There are restrictions on naming variables and symbolic constants:
- First character must be a letter (the underscore counts). However, library routines often begin with underscore, so avoid doing that.
- Use lowercase for vars and uppercase for symbolic constants.
- Can't use keywords like `if, else, int, float`, etc.

# 2.2: Data Types and Sizes
Basic data types are `char, int, float,` and `double`.

You can also add the qualifiers `short` and `long` for integer sizes. `short` is at least 16 bits, `long` is at least 32 bits, and `int` is either 16 or 32 bit. `short` < `int` < `long`.

`signed`/`unsigned` may be applied to any character or integer.

# 2.3: Constants
An integer constant like `1234` is an int. An long constant is terminated with `l` or `L`, as in `long int mynum = 123456789L`. An int too big to fit into int type will turn into a long.

Floating point constants are double unless terminated with f/F. l/L terminators indicate a long double.

You can specify raw values in different number systems. A leading 0 on a number (ex: `01234`) indicates octal, and a leading 0x or 0X means hexadecimal. You can append these with the terminal characters listed above.

Characters are also integer constants corresponding to their ASCII value. You can write '0' or '\0'; they're the same.

Strings are internally represented as an array of characters with a null character appended at the end.

**The library function `strlen(s) returns the length of the string without the terminal `\0`, so make sure to add 1 when allocating space!**

An *enumeration constant* is a list of constant integer values, as in

`enum boolean { NO, YES };`, where the first value is given `0`, the second `1`, and so on unless explicitly specified. If not all values are specified, implcit assignment continues from the last specified value.

Ex:
`enum months { JAN = 1, FEB, MAR, APR, MAY, JUN, JUL, AUG, SEP, OCT, NOV, DEC }; /* FEB is 2, MAR is 3, etc. */`

# 2.4: Declarations
If assigning a value in the declaration of a non-automatic variable, the initialization is done only once.

External and static variables are initialized to 0. Automatic variables without initializers have undefined (garbage) values. Basically, those spots will have whatever the OS put in there last time the spot was used.

You can apply the modifier `const` in a declaration to specify that the variable's value will not be changed. Ex:

```c
const double e = 2.71828182845905;
const char message[] = "warning";
```

You can also use `const` with array arguments to indicate that the function does not change the array's contents.

`int strlen(const char[]);`

Changing a `const` variable is undefined behavior, and the results will depend on your system.

# 2.5 Arithmetic Operators
Binary arithmetic operators are `+, -, *, /, %`. Integer division truncates any fractional part, and modulus can only be applied to integers. Truncation for `/` on negative numbers and the sign of the result for `%` on negative numbers varies by machine. Overflow and underflow cases are also machine-specific.

ORDER OF PRECEDENCE (highest to lowest):
    - Unary `+` and `-` (ex: `-4` is negative 4)
    - `*, /, %`
    - `+, -`
    - In case of tie, go left to right.

Relational operators (`>, <, <=, >=`) all have the same precedence. Lower priority are the equality operators (`==` and `!=`). So, `i < lim - 1` is expressed as `i < (lim - 1)`.

# 2.7: Type Conversions
When a binary operator has operands of different types, they are converted to a common type. Usually, the "narrower" (less information) operand is converted into a type that is "wider" (can store more information). For example, an int contains less information than a float, so an operation with int and float will turn both into floats. You may force a conversion the other way, and you will get a warning, but it is not illegal.

In the conditional expressions like `if, while, for,` etc, `False` is `0` and `True` is any non-zero number.

Floats are used over doubles in automatic type casting to save storage in large arrays, or, less often, to save time on machines where double-precision arithmetic is expensive (not optimized, your compiler handles this for you).

Explicit type conversions can be forced (**"coerced"**) with a unary operator called a **cast**: `(target-type) expression`.

For example, the square root function takes a double. If you pass an integer and want to change it to a double, you would do: `sqrt((double) n);`. Remember that `n` itself is not altered because C is pass by value, so the parameter gets a copy of `n` in double type.

If arguments are declared by a function prototype (which they usually are), then declaration will cause automatic coercion, and you can just do `sqrt(n)`, and n will automatically be converted.

# 2.8: Increment and Decrement Operators
You can use `++` to add 1 to the operand, or `--` to subtract 1. If you put these before, they are **prefix operators**, and if you put them after, they are **postfix operators**.

`++n` Increments `n` before using the resulting value, whereas `n++` will use the value of n and then update it after.

For example:
```c
int n = 5;
int x = n++; /* x is 5, n is now 6 */

~~~~~~~~~~
int n = 5;
int x = ++n; /* x and n are both x */
```

You can only apply increment/decrement operators to variables and not on expressions: `(i + j)++` is illegal.

This functionality can be useful. Lets examine a before/after:

```c
/* BEFORE */
if (c == '\n') {
    s[i] = c;
    i++;
}

/* AFTER */
if (c == '\n') s[i++] = c; /* i is incremented after assignment */
```

# 2.9: Bitwise operators
C provides 6 operators for bit manipulation, which can only be applied to **integral operands** (`char, short, int,` and `long`). Signed/unsigned does not matter.

- `&` - bitwise AND
- `|` - bitwise OR
- `^` - bitwise XOR
- `<<` - left shift
- `>>` - right shift
- `~` - one's complement (unary, or bitwise NOT)

# 2.10: Assignment Operators and Expressions
You can shorten `x = x + 1` to `x += 1`. Woohoo.

# 2.11: Conditoinal Expressions
Ternary operator is a thing. The following two snippets of code are equivalent:

```c
if (a > b)
    z = a;
else
    z = b;
```
and
`z = (a > b) ? a : b`

You don't usually need parenthesis since the priority of `?` is very low, but you should do it anyway for readability.

# 2.12
I'm not listing the table here.

Be careful when doing stuff like `arr[i] = i++;` since this may cause unintended side effects depending on your compiler. It's much safer to do this in two separate statements.
