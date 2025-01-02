# Memory Segmentation

## Segments

A compiled program's memory is divided into 5 Segments :&#x20;

1. **text segment**
   1. content = instructions in assembly language
   2. access = read-only since there is no need to change code after compilation (it also allows multiple programs to run the same program in the same time)
   3. fixed size
2. **data segment**
   1. content = global and static variables which are initialized
   2. access = writable
   3. fixed size
3. **bss segment**
   1. content = global and static variables which are not initialized
   2. access = writable
   3. fixed size
4. **heap segment**
   1. content = blocks of memory that can be manipulated by allocator and deallocator algorithms by the programmer on the fly
   2. access = writable
   3. not fixed size
   4. the growth of the heap moves downward toward higher memory addresses
5. **stack segment**
   1. content = local variables inside functions and information of functions context that are stored in FILO (First-In Last-Out). Stack segment contains a stack of different stack frames
   2. access = writable
   3. not fixed size
   4. it grows upward in a visual listing of memory toward lower memory addresses

## Examples of memory addresses of different variables

* global or static variable which is initialized : 0x080497f8 (data segment)

```c
int initialized = 5;
```

* global or static variable which is not initialized : 0x080497ec (bss segment)

```c
int not_initialized;
```

* heap variable : 0x080497f0 (heap segment)

```c
int main()
{
    int *heap_var;
    heap_var = (int)malloc(4);
}
```

* local variables : very high addresses (stack segment)
  * local\_var of main : higher address than local\_var of function 0xbffff834
  * local\_var of function : smaller address than local\_var of main 0xbffff814

```c
void function
{
    int local_var;
}
int main()
{
    int local_var;
    function();
}
```



Example : swap 2 values by using "Passing by value"

\--> Does not work because values inside swap function are local variables and not global ones

```c
#include<stdio.h>

void swap(int a, int b);

int main(void)
{
    int x=1;
    int y=2;
    printf("x is %i and y is %i\n",x,y);
    swap(x,y);
    printf("x is %i and y is %i\n",x,y);

}

void swap(int a, int b)
{
    int tmp = a;
    a = b;
    b = tmp;
}
```



```bash
yanis@work:~/gitbook-hacknotes$ make swap
cc     swap.c   -o swap
yanis@work:~/gitbook-hacknotes$ ./swap
x is 1 and y is 2
x is 1 and y is 2
```

Need to use method "Passing by reference"&#x20;

```c
#include<stdio.h>

void swap(int *a, int *b);

int main(void)
{
    int x=1;
    int y=2;
    printf("x is %i and y is %i\n",x,y);
    swap(&x,&y);
    printf("x is %i and y is %i\n",x,y);

}

void swap(int *a, int *b)
{
    int tmp = *a;
    *a = *b;
    *b = tmp;
}
```

