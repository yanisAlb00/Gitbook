# Pointers

A pointer contains the information of the address of the first byte of a memory storage.

It is defined by \*variable in C and associated to a data type (here char=1 byte)

```c
#include <stdio.h>
#include <string.h>
int main() {
 char str_a[20]; // A 20-element character array
 char *pointer; // A pointer, meant for a character array
 char *pointer2; // And yet another one
 strcpy(str_a, "Hello, world!\n");
 pointer = str_a; // Set the first pointer to the start of the array.
 printf(pointer);
 pointer2 = pointer + 2; // Set the second one 2 bytes further in.
 printf(pointer2); // Print it.
 strcpy(pointer2, "y you guys!\n"); // Copy into that spot.
 printf(pointer); // Print again.
}
```

`&` operand is used to manipulate the address of data :&#x20;

```c
#include <stdio.h>
int main() {
 int int_var = 5;
 int *int_ptr;
 int_ptr = &int_var; // put the address of int_var into int_ptr
}
```

`*`operand is used to get the value inside the memory located at the address stored by the pointer :

```c
#include <stdio.h>
int main() {
 int int_var = 5;
 int *int_ptr;
 int_ptr = &int_var; // Put the address of int_var into int_ptr.
 printf("int_ptr = 0x%08x\n", int_ptr);
 printf("&int_ptr = 0x%08x\n", &int_ptr);
 printf("*int_ptr = 0x%08x\n\n", *int_ptr);
 printf("int_var is located at 0x%08x and contains %d\n", &int_var, int_var);
 printf("int_ptr is located at 0x%08x, contains 0x%08x, and points to %d\n\n",
 &int_ptr, int_ptr, *int_ptr);
}
```

Example of code form CS50 :&#x20;

A string is a pointer to a char value (1byte = 8 bits).

The content of the string is all the chars following this value until null byte (/0 : all bits put at 0)

```c
#include <stdio.h>
#include <string.h>
int main(void)
{
    char *string="HI!";
    int length= strlen(string);
    printf("String length is : %i\n",length);
    printf("%s\n",string);
    printf("%p\n",&string);
    for(int i=0; i<length;i++)
    {
        printf("address is : %p\n",&string[i]);
        printf("value is : %c\n",string[i]);
    }
    printf("address is : %p\n",&string[length]);
    printf("value is : %b\n",string[length]);
}
```

result :&#x20;

```bash
yanis@work:~/gitbook-hacknotes$ make string
cc     string.c   -o string
yanis@work:~/gitbook-hacknotes$ ./string 
String length is : 3
HI!
0x7fff4de6e600
address is : 0x556e6e7bc004
value is : H
address is : 0x556e6e7bc005
value is : I
address is : 0x556e6e7bc006
value is : !
address is : 0x556e6e7bc007
value is : 0
```

