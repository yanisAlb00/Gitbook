# Assembleur

## Utilisation de objdump pour lire le progamme en assembleur

2 principaux types de langages pour la décompilation de binaire :&#x20;

* AT\&T : Syntaxe avec de nombreux $ et %
* INTEL : Syntaxe + simple

```
objdump -M intel -d a.out | grep -A20 main.:   
0000000000001139 <main>:
    1139:       55                      push   rbp
    113a:       48 89 e5                mov    rbp,rsp
    113d:       48 83 ec 10             sub    rsp,0x10
    1141:       c7 45 fc 00 00 00 00    mov    DWORD PTR [rbp-0x4],0x0
    1148:       eb 13                   jmp    115d <main+0x24>
    114a:       48 8d 05 b3 0e 00 00    lea    rax,[rip+0xeb3]        # 2004 <_IO_stdin_used+0x4>
    1151:       48 89 c7                mov    rdi,rax
    1154:       e8 d7 fe ff ff          call   1030 <puts@plt>
    1159:       83 45 fc 01             add    DWORD PTR [rbp-0x4],0x1
    115d:       83 7d fc 09             cmp    DWORD PTR [rbp-0x4],0x9
    1161:       7e e7                   jle    114a <main+0x11>
    1163:       b8 00 00 00 00          mov    eax,0x0
    1168:       c9                      leave
    1169:       c3                      ret

Disassembly of section .fini:

000000000000116c <_fini>:
    116c:       48 83 ec 08             sub    rsp,0x8
    1170:       48 83 c4 08             add    rsp,0x8

```

\
1139: = adresse mémoire en Hexadécimal

55 = Instruction codée sur 1 byte (2 caractères hexa décimal)

push = traduction de l'instruction 55

rbp,rsp = rbp est la destination et rsp la source

Le format destination,source peut contenir différentes valeurs :&#x20;

* Un registre
* Une adresse mémoire
* Une valeur

## Utilisation de gdb pour afficher l'état des registres

Les registres sont des variables internes au processeur utilisées pour répondre à différents besoins.

```
gdb -q ./a.out  
Reading symbols from ./a.out...
(No debugging symbols found in ./a.out)
(gdb) break main
Breakpoint 1 at 0x113d
(gdb) run
Starting program: /home/yanis/a.out 
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

Breakpoint 1, 0x000055555555513d in main ()
(gdb) info registers
rax            0x555555555139      93824992235833
rbx            0x7fffffffdf68      140737488346984
rcx            0x555555557dd8      93824992247256
rdx            0x7fffffffdf78      140737488347000
rsi            0x7fffffffdf68      140737488346984
rdi            0x1                 1
rbp            0x7fffffffde50      0x7fffffffde50
rsp            0x7fffffffde50      0x7fffffffde50
r8             0x0                 0
r9             0x7ffff7fcfb30      140737353939760
r10            0x7fffffffdb80      140737488345984
r11            0x206               518
r12            0x0                 0
r13            0x7fffffffdf78      140737488347000
r14            0x7ffff7ffd000      140737354125312
r15            0x555555557dd8      93824992247256
rip            0x55555555513d      0x55555555513d <main+4>
eflags         0x246               [ PF ZF IF ]
cs             0x33                51
ss             0x2b                43
ds             0x0                 0
es             0x0                 0
fs             0x0                 0
gs             0x0                 0
fs_base        0x7ffff7fad680      140737353799296
gs_base        0x0                 0
(gdb) 

```

[https://cs.wellesley.edu/\~cs240/s16/slides/x86-basics.pdf](https://cs.wellesley.edu/~cs240/s16/slides/x86-basics.pdf)

## General-Purpose registers :

EAX = Accumulator

ECX = Counter

EDX = Data

EBX = Base registers

-> Ces 4 registres sont utilisés comme variables temporaires quand le CPU exécutent des instructions

ESP = Stack pointer

EBP = Base Pointer

-> ESP et EBP Stockent des adresses 32 bits correspondant à un emplacement mémoire

ESI = Source index

EDI = Destination index

EIP = Instruction Pointer

-> EIP pointe sur l'instruction en cours de lecture par le processeur

EFLAGS = several bit flags that are used for comparisons and memory segmentations

## Exemple d'instructions

mov = "move" déplacer une valeur depuis la source vers la destination

sub = substract source from destination and stores it in destination

inc = increment de 1 sans modifier le CF Flag

cmp = comparaison de 2 valeurs, par exemple `cmp DWORD PTR [ebp-4],0x9` compare une valeur de 4 byte située à ebp-4 avec la valeur 9

Toute instruction qui commence par j est utilisée pour jump vers une autre partie du code

jle = "jump if less than or equal to" va aller vers une adresse mémoire spécifiée si la valeur précédente est inférieure ou égale à 9

jump = aller à une prochaine instruction

## Faire un débug avancé en soumettant le code source à gdb

```
gdb -q ./a.out                               
Reading symbols from ./a.out...
(No debugging symbols found in ./a.out)
(gdb) set disassembly-flavor intel
(gdb) disassemble main
Dump of assembler code for function main:
   0x0000000000001139 <+0>:     push   rbp
   0x000000000000113a <+1>:     mov    rbp,rsp
   0x000000000000113d <+4>:     sub    rsp,0x10
   0x0000000000001141 <+8>:     mov    DWORD PTR [rbp-0x4],0x0
   0x0000000000001148 <+15>:    jmp    0x115d <main+36>
   0x000000000000114a <+17>:    lea    rax,[rip+0xeb3]        # 0x2004
   0x0000000000001151 <+24>:    mov    rdi,rax
   0x0000000000001154 <+27>:    call   0x1030 <puts@plt>
   0x0000000000001159 <+32>:    add    DWORD PTR [rbp-0x4],0x1
   0x000000000000115d <+36>:    cmp    DWORD PTR [rbp-0x4],0x9
   0x0000000000001161 <+40>:    jle    0x114a <main+17>
   0x0000000000001163 <+42>:    mov    eax,0x0
   0x0000000000001168 <+47>:    leave
   0x0000000000001169 <+48>:    ret
End of assembler dump.

```

## Storage of bytes in memory address

info register show the content of a specific register

```
(gdb) break main
Breakpoint 1 at 0x113d
(gdb) run
Starting program: /home/yanis/a.out 
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

Breakpoint 1, 0x000055555555513d in main ()
(gdb) i r rip
rip            0x55555555513d      0x55555555513d <main+4>

```

x command (for examine) examines the memory

```
x/o 0x8048384 -> displays in octal
x/x $eip -> displays in hexadecimal
x/u $eip -> displays in unsigned (standard base-10
x/t $eip -> displays in binary

```

A memory address can store many bytes :

* A WORD or DWORD (4-Byte) -> default size (w)
* A HALFWORD or A SHORT (2-Byte) (h)
* A SINGLE (1-Byte) (b)
* A GIANT (8-Byte) (g)

Bytes are stored in little-endian byte order which means the least significant byte is stored in first&#x20;

```
(gdb) x/4ub $rip
0x55555555513d <main+4>:        72      131     236     16

// So the value is : 
bc -ql
16*(256^3) + 236*(256^2) + 131*(256^1) + 72*(256^0)
283935560

```

It is also possible to examine instructions :&#x20;

```
(gdb) info register $rip
rip            0x55555555513d      0x55555555513d <main+4>
(gdb) x/i $rip
=> 0x55555555513d <main+4>:     sub    $0x10,%rsp
(gdb) x/3i $rip
=> 0x55555555513d <main+4>:     sub    $0x10,%rsp
   0x555555555141 <main+8>:     movl   $0x0,-0x4(%rbp)
   0x555555555148 <main+15>:    jmp    0x55555555515d <main+36>
(gdb) x/7xb $rip
0x55555555513d <main+4>:        0x48    0x83    0xec    0x10    0xc7    0x450xfc

```

## Example of memory content during program execution

1. Observe memory content at the beggining

The following instruction will move 0 to the memory located at the address stored in the RBP register minus 4

```
0x555555555141 <main+8>:     movl   $0x0,-0x4(%rbp)
```

Examine the content of rbp register minus 4

```
(gdb) info register rbp
rbp            0x7fffffffde50      0x7fffffffde50
(gdb) x/4xb $rbp-4
0x7fffffffde4c: 0xff    0x7f    0x00    0x00
(gdb) x/4xb 0x7fffffffde4c
0x7fffffffde4c: 0xff    0x7f    0x00    0x00
(gdb) print $rbp - 4
$1 = (void *) 0x7fffffffde4c

```

-> Note that 0x7fffffffde50-4 = 0x7fffffffde4c

At this time it contains random garbage bytes that will be zeroed out during next instruction

2. Go to next instruction

```
(gdb) nexti
0x0000555555555148 in main ()
(gdb) x/4xb 0x7fffffffde4c
0x7fffffffde4c: 0x00    0x00    0x00    0x00

```

