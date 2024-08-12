# Assembleur

2 principaux types de langages pour la décompilation de binaire :&#x20;

* AT\&T : Syntaxe avec de nombreux
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

55 = Instruction codée sur 2 bytes (2 caractères hexa décimal

push = traduction de l'instruction 55

rbp = ?

