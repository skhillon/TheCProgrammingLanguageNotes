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

