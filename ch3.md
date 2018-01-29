# CHAPTER 3: CONTROL FLOW
Use braces if you have nested conditionals to show which `else` corresponds to which `if`.

I skipped 3.1-3.4 because that's basic programming.

# 3.4: Switch
Quick way to write a bunch of `else-if` statements, but the constraint is that each case must be a *constant integer value*. Ex:

```c
switch (expression) {
    case const_expr_int:
        my_code();
        break;
    case other_const_expr_int:
        my_code_other();
        break;
    default:
        unexpected();
        break;
}
```

If you do not provide the `break` or `return` statements, the switch statement will **fall through** and continue checking for cases below that might match. This can be a tool but also go wrong if you're not careful. Ex:

```c
/* this is useful */
switch (c) {
    case '0': case '1': .... case '9':
        some_code_with_nums();
        break;
    case '\n': case '\t':
        some_code_with_whitespace();
        break;
    default:
        other_code();
        break;
}
```

You don't need `default`, but better safe than sorry.

You don't need the expressions in a for loop.

```c
/* These loops are both infinite */

while (1) {}

for (;;) {}
```

# Skipped sections 3.5-3.7...

# 3.8: Goto and Labels
You should use this sparingly as it's a code smell, but it's there. `goto` is not used anywhere in the book except here, so don't be dumb.

```c
for (...)
    for (...)
        for (...)
            if (disaster) goto disaster_handler;

disaster_handler:
    fix_the_disaster();
```

As you can see, the code jumps to disaster_handler. This can be fixed with a simple flag:

```c
found = 1;

for (...)
    for (...)
        for (...)
            if (disaster) found = 0;

if (found)
    good_things();
else
    bad_things();
```

