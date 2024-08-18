# Variables in C

## Different types of variables

### String

```c
char str[20];
```

Array of char (1 Byte) value

### Signed (4 Bytes)

Can be both positive or negative using the two's complement method

```c
unsigned variable;

```

### Unsigned (4 bytes)

Can only be positive

### Long (8 Bytes)

### Short (2 Bytes)

### Float (4 Bytes)

## Variables scoping

* Global variables (value persists across all functions but if a local variable of a function has the same name, local variable will win)
* Local variables (value that does not persist across different function calls, only use in one specific function call)
* Static variables (if it is defined inside a function, it will persist across different function calls)
