# Arrays , Linked Lists and Trees in C

## Array

An Array is a chunk of contiguous memory :&#x20;

```
| 1 | 2 | 3 | 4 |
```

### Advantage

* Fast  : we can use binary search to find an element with a complexity of O(log n) so very efficient

### Inconvenient&#x20;

* Heavy to copy : Because the chunk of memory is contiguous we can not easily copy this array with a low complex algorithm

## Linked list

A linked list is a set of nodes.&#x20;

Each node is defined by a value and a pointer to the next value of the list.

The last node has his pointer to NULL value.

```
At address 0x123 : node 0 = {1 , 0x456}
At address 0x456 : node 1 = {2, Ox789}
At address 0x789 : node 2 = {3, NULL}
```

### Advantage

* Easy to copy or prepending/appending an element : We don't need to browse all the list we can just change the first pointer

### Inconvenient

* Binary search can't not be apply because we don't have a contiguous chunk of memory

## Trees = Concept of perfect mixture between array + Linked List

A tree is a set of nodes.

Each note is defined by defined by a value and 2 pointers : one to his left child and the other to his right child.

The "leaf" nodes contain NULL on left and/or right

```
           4
    2                6
1        3        5        7
```

For example, if we introduce a tree with the following rule : each node contains a smaller value on the left and a higher value on the right we would be back to the complexity of O(log n) for searching and element







