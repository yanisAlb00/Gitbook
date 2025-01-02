# Recursion in C

Example of code without recursion :&#x20;

```c
#include <stdio.h>
#include <stdlib.h>

void draw(int size);

int main(void)
{
    int size = 0;
    printf("What size for the pyramide ?\n");
    scanf("%i",&size);
    draw(size);
}

void draw(int size)
{
    for(int i=0; i<size;i++)
    {
        for(int j=0; j<i+1; j++)
        {
            printf("#");
        }
        printf("\n");
    }
}
```

The same code should be passed in recursive mode because there are 2 for loops.

{% hint style="info" %}
It is important to add an exit condition when we do recursive to not enter in infinite loop
{% endhint %}

Recursive version :&#x20;

```c
#include <stdio.h>
#include <stdlib.h>

int draw(int size);

int main(void)
{
    int size = 0;
    printf("What size for the pyramide ?\n");
    scanf("%i",&size);
    draw(size);
}

int draw(int size)
{
    if(size>0)
    {
        draw(size-1);
        for(int i=0; i<size;i++)
        {
            printf("#");
            
        }
        printf("\n");
    }
    else
    {
        return 0;
    }
}
```

When the user enter 4 for the size, main first call draw(4) and draw(4) calls draw(3), draw(3) calls draw(2) , so draw(1) is the first iteration to print #
