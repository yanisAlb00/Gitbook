# Memory Segmentation

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
