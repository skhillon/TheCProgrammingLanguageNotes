# CHAPTER 5: POINTERS AND ARRAYS
A **pointer** is a variable that contains the address of another variable.
The type `void*` replaces `char*` as the proper type for a generic pointer.

# 5.1: Pointers and Addresses
Hopefully you learned a little about this in your Computer Architecture class. I'm not drawing a picture.

The unary operator `&` gives the address of an object:
```c
char c
char *p = &c; /* p contains the address of c */
```

You can't use the `&` operator on expressions, constants, or `register` variables.

The unary operator `*` is the **dereferencing** operator. When applied to a pointer, *and only a pointer*, it accesses the object the pointer points to. In other words, p contains the address of c. `*p` would go to the address in c and give you the value at that address.

Another example:
```c
int x = 1, y = 2, z[10];
int *ip;

ip = &x; /* ip contains the address where the value in x is stored */
y = *ip; /* y is now 1 */
*ip = 0; /* sets value where x exists to 0, so now x is 0. */
ip = &z[0]; /* ip now points to z[0] */
```

Every pointer points to a specific data type, with 1 exception: the generic pointer, `void*`, can point to any type but cannot be dereferenced itself.

The values that pointers are pointing to can be incremented in any of the following ways:
```c
y = *ip + 1;

*ip += 1;

++*ip; /* dereference is higher priority, but you should probably do ++(*ip); for clarity */

(*ip)++ /* unary operators go right to left, so you need the parenthesis */
```

# 5.2: Pointers and Function Arguments
Since C is pass by value, a function is always going to receive a copy of a variable, so there's no way to modify what you passed in originally. But, the workaround is to pass in a pointer. If a function gets a copy of the pointer, and you dereference it, you can actually modify something external.

Take swap, for example, which attempts to modify the values in the input variables. You need to do `swap(&a, &b);` for it to work correctly.

# 5.3: Pointers and Arrays
Arrays and pointers are very tightly linked, and any operation that can be achieved by array subscripting can also be achieved with pointers. Pointers will be faster, but are somewhat harder to read/understand.

An array is a pointer to the first element. In fact, C immediately converts array indexing syntax `arr[i]` to pointer syntax `*(a + i)`.

There is one difference between an array name and a pointer. A pointer is a variable, so expressions like `pa = a` and `pa++` are legal. However, an array is not a variable; it cannot be reassigned or incremented, so expressions like `arr = pa` and `arr++` are NOT legal.

In function declarations, `foo(int arr[]) {}` and `foo(int *arr) {}` are equivalent. It is possible to pass part of an array to a function by passing a pointer to the beginning of the subarray.

For example, if you wanted to pass all elements except 0 and 1 in `arr` to `foo()`, you could do either of the following:
```c
foo(&a[2]);
foo(a + 2);
```
Both will pass elements 2 -> last.

*If you are sure that elements exist*, you can also index backwards in an array. For example, if you were handed an array starting at element 2, as in the last code snippet, your local array copy would start at index 0. However, you know for a fact that there are two more that a function did not pass. You may access arr[-1] or arr[-2], which immediately precede index 0.

# 5.4: Address Arithmetic
Beyond the stuff you saw in the last section, you can actually use relational operators on pointers. If pointers p and q point to members of the same array, then you can use `<, >, !-, ==` based on position in the array.

`size_t` is the unsigned integer type returned by the `sizeof` operator.

# 5.5: Character Pointers and Functions
A string constant "I am a string" is an array of characters with '\0' appended at the end. The length in storage is therefore 1 more than the number of characters.

There is an important distinction between the following two lines of code:
```c
char arrmessage[] = "now is the time";
char *pmessage = "now is the time";
```

`arrmessage` is an array which is big enough to hold all the characters and a terminating null character. You may change elements in this array, but for all intents and purposes, you should treat this line as if it were
```c
const char arrmessage[] = "now is the time";
```
because their functionality is one and the same.

On the other hand, pointers can always be changed to point elsewhere, but you can't modify the string contents directly.

# 5.6: Pointer Arrays; Pointers to Pointers
Yeah you can have them.

# 5.7: Multi-dimensional Arrays
You can have these too:
```c
static char daytab[2][4] = {
    { 0, 1, 2, 3 },
    { 4, 5, 6, 7 }
}
```

If a two-dimensional array is to be passed to a function, the parameter declaration must specify the number of columns (4 above). We don't care about number of rows because we can just keep iterating rows until we hit a NULL pointer. We do, however, need a way to get back up to the row-level.

```c
foo(int daytab[2][4]) {

}

or

foo(int daytab[][13]) {

}

or 

foo (int (*daytab)[13]) {

}
```

# 5.8: Initialization of Pointer Arrays
It's the same as with the other declaration:

```c
static char *name[] = {
    "Illegal Month", "January", ... "December"
}; /* DON'T FORGET THE SEMICOLON */
```

# 5.9: Pointers vs Multi-dimensional Arrays
If you do this:
```c
int a[10][20];
```
then you set aside 200 integer-sized locations in memory, and you use a[row][col] to get to an index.

However, if you do this:
```c
int *b[10];
```
then the definition only allocates 10 pointers and does not initialzie them. Even if we end up needing all 200, we didn't need it right away, and that makes things more dynamic.

# 5.10: Command Line Arguments: Done that
# 5.11: Pointers to Functions
In C, a function itself is not a variable. However, functions can be assigned, placed in arrays, passed to other functions, returned by functions, etc.

# 5.12: I dont think we need to know this cuz it's whack
