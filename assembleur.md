# Assembleur

## Utilisation de objdump pour lire le progamme en assembleur

2 principaux types de langages pour la décompilation de binaire :&#x20;

* AT\&T : Syntaxe avec de nombreux $ et %
* INTEL : Syntaxe + simple

```
objdump -M intel -d a.out

0000000000000000 <main>:
   0:	55                   	push   rbp
   1:	48 89 e5             	mov    rbp,rsp
   4:	c7 45 fc 00 00 00 00 	mov    DWORD PTR [rbp-0x4],0x0
   b:	83 45 fc 14          	add    DWORD PTR [rbp-0x4],0x14
   f:	90                   	nop
  10:	5d                   	pop    rbp
  11:	c3                   	ret   
```

\
0: = adresse mémoire en Hexadécimal

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

(gdb) break main
(gdb) run
(gdb) info registers
```

[https://cs.wellesley.edu/\~cs240/s16/slides/x86-basics.pdf](https://cs.wellesley.edu/\~cs240/s16/slides/x86-basics.pdf)

## General-Purpose registers :

EAX = Accumulator

ECX = Counter

EDX = Data

EBX = Base registers

\-> Ces 4 registres sont utilisés comme variables temporaires quand le CPU exécutent des instructions

ESP = Stack pointer

EBP = Base Pointer

\-> ESP et EBP Stockent des adresses 32 bits correspondant à un emplacement mémoire

ESI = Source index

EDI = Destination index

EIP = Instruction Pointer

\-> EIP pointe sur l'instruction en cours de lecture par le processeur

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
gcc -g helloworld.c 
[Aug 13, 2024 - 11:36:12 (UTC)] exegol-oscp_lab hacking_art_of_exploitation # gdb -q ./a.out     
pwndbg: loaded 154 pwndbg commands and 47 shell commands. Type pwndbg [--shell | --all] [filter] for a list.
pwndbg: created $rebase, $base, $ida GDB functions (can be used with print/break)
/root/.gdbinit:2: Error in sourced command file:
No symbol table is loaded.  Use the "file" command.
Reading symbols from ./a.out...
------- tip of the day (disable with set show-tips off) -------
GDB's follow-fork-mode parameter can be used to set whether to trace parent or child after fork() calls
pwndbg> list
1	#include <stdio.h>
2	int main() {
3	
4		int i;
5	   
6		// printf() displays the string inside quotation
7		for(i=0;i<10;i++)
8		{        
9			printf("Hello, World! \n");
10		}	
pwndbg> disassemble main
Dump of assembler code for function main:
   0x0000000000000754 <+0>:	stp	x29, x30, [sp, #-32]!
   0x0000000000000758 <+4>:	mov	x29, sp
   0x000000000000075c <+8>:	str	wzr, [sp, #28]
   0x0000000000000760 <+12>:	b	0x77c <main+40>
   0x0000000000000764 <+16>:	adrp	x0, 0x0
   0x0000000000000768 <+20>:	add	x0, x0, #0x7b0
   0x000000000000076c <+24>:	bl	0x630 <puts@plt>
   0x0000000000000770 <+28>:	ldr	w0, [sp, #28]
   0x0000000000000774 <+32>:	add	w0, w0, #0x1
   0x0000000000000778 <+36>:	str	w0, [sp, #28]
   0x000000000000077c <+40>:	ldr	w0, [sp, #28]
   0x0000000000000780 <+44>:	cmp	w0, #0x9
   0x0000000000000784 <+48>:	b.le	0x764 <main+16>
   0x0000000000000788 <+52>:	mov	w0, #0x0                   	// #0
   0x000000000000078c <+56>:	ldp	x29, x30, [sp], #32
   0x0000000000000790 <+60>:	ret
End of assembler dump.
pwndbg> 
```

## Storage of bytes in memory address

info register show the content of a specific register

```
(gdb) info register eip
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
(gdb) x/4ub
0x8048384    <main+16>:    199    69    252    0

// So the value is : 0*256^3 + 252*256^2 + 69*256^1 + 199*256^0 = 16532935
```
