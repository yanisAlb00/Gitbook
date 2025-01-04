# Hash tables

An Hash table is an array of linked lists

For example, the storage of contacts :&#x20;

On a each value of that array, it is stored a pointer to the next value of the linked list

In the worst case : to find an element it is O(n)

```
A
B -> Bowser -> BowserJr
C
D -> Daisy -> Diddy Kong -> Donkey Kong
E
F
G
H
I
J
K
L -> Luigi -> Link
M -> Mario
N
O
P-> Peach
Q
R
S
T
U
V
W
X
Y
Z
```

If I make bigeer the size of that array :&#x20;

```
...
...
dai -> daisy
daj
dak
...
did -> diddy kong
die
...
don -> donkey kong
dom
```

The size of that array is 26³ in memory but we now reach constant time to find a contact if don't have a lot of collisions O(1)

Hash function role is to output a number regarding the first letter of the name

```
Mario -> HASH FUNCTION -> 12
```

```c
# include <ctype.h>

int hash (char* contact)
{
    return toupper(contact[0]) -'A';
}
```

