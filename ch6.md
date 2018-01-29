# CHAPTER 6: STRUCTURES
A **structure** is a collection of one or more variables, possibly of different types, grouped together under a single name for convenient handling. They permit a group of related variables to be treated as a unit instead of as separate entities, and therefore organize the handling of complicated data.

You may pass structures to functions, return structures, and assign structures.

# 6.1: Basics of Structures
Let's create a few structures suitable for graphics. The basic object is a point with an x coordinate and y coordinate:
```c
struct point {
    int x;
    int y;
};
```
The keyword `struct` starts the declaration, which is a list of declarations enclosed in braces. You may add an optional **structure tag** after the keyword (the tag name here is `point`, and idk why you would ever not give it the tag).

The variables named within a structure are called **members**. A structure member and an ordinary, non-member variable in the same code can have the same name, as they can always be distinguished by context.

A `struct` declaration defines a type, and the right brace that terminates the list of mbmers may be followed by a list of variables. So, the following two blocks of code are analagous:

```c
struct point {
    int x;
    int y;
} origin, top_left, bottom_right;

int x, y, z;
```
Each declares variables and allocates memory for them on the stack.

If you do not follow up with a list of variables, you do not allocate storage. However, if you give it a tag, you can use it in later definitions of the structure.

```c
struct point pt;
...
struct point maxpt = { 320, 200 };
```
You may also initialize an automatic structure by assignment or by calling a function that returns a structure of the right type.

A member of a structure is accessed using dot-syntax: `structure-name.member`.

You may also nest structures within each other:

```c
struct rect {
    struct point pt1;
    struct point pt2;
} screen;

screen.pt1.x = 5;
```

# 6.2
