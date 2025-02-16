# Rapport VM Reverse

## Introduction

### Rappel des questions

- La VM a des registres ? Si oui, décrivez les précisément [**10xp**]
- La VM a une pile ? Si oui, décrivez la précisément [**10xp**]
- Décrire précisément chaque opérations implémentées par la VM (opcode, nom, sémantique, encodage) [**15xp**]
- Fournir et écrire un script python qui désassemble le bytecode [**10xp**]
- Reverse le bytecode et fournir une version commentée du bytecode désassemblé [**15xp**]
- Trouver le flag qui est accepté par la VM [**10xp**]

## Projet GHydra

Disponible dans le rendu sur Google Classroom

## 1. La VM a-t-elle des registres ?

Oui, la VM possède les registres suivants :

- **instruction_pointer** (IP) : Pointeur d'instruction, utilisé pour suivre la position actuelle de l'exécution du programme dans le bytecode.
- **stack_pointer** (SP) : Indique le sommet de la pile utilisée par la VM.
- **cmp_flag** : Un drapeau utilisé pour stocker le résultat des comparaisons (true/false).

Ces registres sont implémentés dans la structure `astruct` comme suit :

```c
typedef struct {
    int instruction_pointer;
    int stack_pointer;
    char DATA[100];
    bool cmp_flag;
} astruct;
```

---

## 2. La VM a-t-elle une pile ?

Oui, la VM dispose d'une pile de taille 100 octets, stockée dans le tableau `DATA` de la structure `astruct`.

- La pile est manipulée via deux fonctions principales :
    - `push(astruct *stack_pointer, char DATA)`: Ajoute une valeur à la pile.
    - `pop(astruct *data)`: Retire une valeur de la pile.

### Implémentation :

```c
char pop(astruct *data) {
    data->stack_pointer = data->stack_pointer - 1;
    return data->DATA[data->stack_pointer];
}
```

```c
void push(astruct *stack_pointer, char DATA) {
    stack_pointer->DATA[stack_pointer->stack_pointer] = DATA;
    stack_pointer->stack_pointer = stack_pointer->stack_pointer + 1;
}
```

---

## 3. Description des opérations de la VM

Chaque opération de la VM est identifiée par un opcode. Voici la liste complète :

| Opcode | Nom | Sémantique | Usage |
|--------|-----|------------|-------|
| `0x00` | ADD | Additionne deux valeurs du sommet de la pile | `pop(), pop(), push(result)` |
| `0x01` | SUB | Soustrait deux valeurs du sommet de la pile | `pop(), pop(), push(result)` |
| `0x02` | MUL | Multiplie deux valeurs du sommet de la pile | `pop(), pop(), push(result)` |
| `0x03` | DIV | Divise deux valeurs du sommet de la pile | `pop(), pop(), push(result)` |
| `0x04` | PUSH | Empile une valeur immédiate | `push(value)` |
| `0x05` | PRINT | Affiche le sommet de la pile | `putc(pop())` |
| `0x06` | CMP | Compare deux valeurs du sommet de la pile | `pop(), pop(), cmp_flag = (val1 == val2)` |
| `0x07` | JMP | Effectue un saut inconditionnel | `IP = pop()` |
| `0x08` | JMP_IF_TRUE | Effectue un saut si `cmp_flag == true` | `if (cmp_flag) IP = pop()` |
| `0x09` | READ | Lit un caractère et l'empile | `push(getchar())` |
| `0x0A` | DUP | Duplique la valeur au sommet de la pile | `push(top), push(top)` |
| `0x0B` | SWAP | Échange les deux premières valeurs de la pile | `swap(top1, top2)` |
| `0x0C` | HALT | Arrête l'exécution | `return 0` |
| `0x0D` | XOR | Applique un XOR bit-à-bit entre deux valeurs | `pop(), pop(), push(result)` |
| `0x0E` | JMP_IF_FALSE | Effectue un saut si `cmp_flag == false` | `if (!cmp_flag) IP = pop()` |
| `0x0F` | POP | Décrémente le pointeur de pile | `stack_pointer--` |

---
## 4. Script python de désassemblage du bytecode

```python
bytecode = [0x04, 0x02, 0x04, 0x18, 0x04, 0x66, 0x04, 0x66, 0x04, 0x18, 0x04, 0x4b, 0x04, 0x54, 0x04, 0x48, 0x04, 0x18, 0x04, 0x5c, 0x04, 0x4f, 0x04, 0x48, 0x04, 0x0c, 0x0b, 0x04, 0x38, 0x0d, 0x05, 0x04, 0x01, 0x0b, 0x01, 0x0a, 0x04, 0x00, 0x06, 0x04, 0x1a, 0x0e, 0x04, 0x0a, 0x05, 0x04, 0x00, 0x09, 0x0a, 0x04, 0x0a, 0x06, 0x0b, 0x04, 0x01, 0x00, 0x04, 0x2f, 0x0e, 0x04, 0x1e, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x54, 0x07, 0x04, 0x4e, 0x05, 0x04, 0x6f, 0x05, 0x04, 0x70, 0x05, 0x04, 0x65, 0x05, 0x04, 0x0a, 0x0c, 0x0f, 0x0f, 0x04, 0x2d, 0x0d, 0x04, 0x50, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x08,
    0x0d, 0x04, 0x69, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x42, 0x0d, 0x04, 0x36, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x77, 0x0d, 0x04, 0x12, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x1d, 0x0d, 0x04, 0x50, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x1e, 0x0d, 0x04, 0x69, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x53, 0x0d, 0x04, 0x36, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x5c, 0x0d, 0x04, 0x12, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x35, 0x0d, 0x04, 0x50, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x01, 0x0d, 0x04, 0x69, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x62, 0x0d, 0x04, 0x36, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x61, 0x0d, 0x04, 0x12, 0x06, 0x04, 0x44,
    0x0e, 0x04, 0x19, 0x0d, 0x04, 0x50, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x01, 0x0d, 0x04, 0x69, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x55, 0x0d, 0x04, 0x36, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x77, 0x0d, 0x04, 0x12, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x04, 0x0d, 0x04, 0x50, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x07, 0x0d, 0x04, 0x69, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x53, 0x0d, 0x04, 0x36, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x7b, 0x0d, 0x04, 0x12, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x3c, 0x0d, 0x04, 0x50, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x28, 0x0d, 0x04, 0x69, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x4d, 0x0d, 0x04, 0x36,
    0x06, 0x04, 0x44, 0x0e, 0x04, 0x53, 0x0d, 0x04, 0x12, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x19, 0x0d, 0x04, 0x50, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x3b, 0x0d, 0x04, 0x69, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x73, 0x0d, 0x04, 0x36, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x46, 0x0d, 0x04, 0x12, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x1f, 0x0d, 0x04, 0x50, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x4e, 0x05, 0x04, 0x69, 0x05, 0x04, 0x63, 0x05, 0x04, 0x65, 0x05, 0x04, 0x21, 0x05, 0x04, 0x0a, 0x05, 0x0c, 0x00, 0x01, 0x1b, 0x03, 0x3b
    ]

def disassemble(bytecode):
    opcodes = {
        0x00: "ADD",
        0x01: "SUB",
        0x02: "MUL",
        0x03: "DIV",
        0x04: "PUSH",
        0x05: "PRINT",
        0x06: "CMP",
        0x07: "JMP",
        0x08: "JMP_IF_TRUE",
        0x09: "READ",
        0x0A: "DUP",
        0x0B: "SWAP",
        0x0C: "HALT",
        0x0D: "XOR",
        0x0E: "JMP_IF_FALSE",
        0x0F: "POP"
    }
    i = 0
    while i < len(bytecode):
        opcode = bytecode[i]
        if opcode == 0x04:  # PUSH a une valeur immédiate
            i += 1
            print(f"{hex(opcode)} : PUSH {bytecode[i]}")
        else:
            print(f"{hex(opcode)} : " + opcodes.get(opcode, "UNKNOWN"))
        i += 1

disassemble(bytecode)
```

Résultat : 

```plain text
0x4 : PUSH 2
0x4 : PUSH 24
0x4 : PUSH 102
0x4 : PUSH 102
0x4 : PUSH 24
0x4 : PUSH 75
0x4 : PUSH 84
0x4 : PUSH 72
0x4 : PUSH 24
0x4 : PUSH 92
0x4 : PUSH 79
0x4 : PUSH 72
0x4 : PUSH 12
0xb : SWAP
0x4 : PUSH 56
0xd : XOR
0x5 : PRINT
0x4 : PUSH 1
0xb : SWAP
0x1 : SUB
0xa : DUP
0x4 : PUSH 0
0x6 : CMP
0x4 : PUSH 26
0xe : JMP_IF_FALSE
0x4 : PUSH 10
0x5 : PRINT
0x4 : PUSH 0
0x9 : READ
0xa : DUP
0x4 : PUSH 10
0x6 : CMP
0xb : SWAP
0x4 : PUSH 1
0x0 : ADD
0x4 : PUSH 47
0xe : JMP_IF_FALSE
0x4 : PUSH 30
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 84
0x7 : JMP
0x4 : PUSH 78
0x5 : PRINT
0x4 : PUSH 111
0x5 : PRINT
0x4 : PUSH 112
0x5 : PRINT
0x4 : PUSH 101
0x5 : PRINT
0x4 : PUSH 10
0xc : HALT
0xf : POP
0xf : POP
0x4 : PUSH 45
0xd : XOR
0x4 : PUSH 80
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 8
0xd : XOR
0x4 : PUSH 105
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 66
0xd : XOR
0x4 : PUSH 54
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 119
0xd : XOR
0x4 : PUSH 18
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 29
0xd : XOR
0x4 : PUSH 80
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 30
0xd : XOR
0x4 : PUSH 105
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 83
0xd : XOR
0x4 : PUSH 54
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 92
0xd : XOR
0x4 : PUSH 18
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 53
0xd : XOR
0x4 : PUSH 80
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 1
0xd : XOR
0x4 : PUSH 105
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 98
0xd : XOR
0x4 : PUSH 54
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 97
0xd : XOR
0x4 : PUSH 18
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 25
0xd : XOR
0x4 : PUSH 80
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 1
0xd : XOR
0x4 : PUSH 105
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 85
0xd : XOR
0x4 : PUSH 54
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 119
0xd : XOR
0x4 : PUSH 18
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 4
0xd : XOR
0x4 : PUSH 80
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 7
0xd : XOR
0x4 : PUSH 105
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 83
0xd : XOR
0x4 : PUSH 54
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 123
0xd : XOR
0x4 : PUSH 18
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 60
0xd : XOR
0x4 : PUSH 80
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 40
0xd : XOR
0x4 : PUSH 105
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 77
0xd : XOR
0x4 : PUSH 54
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 83
0xd : XOR
0x4 : PUSH 18
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 25
0xd : XOR
0x4 : PUSH 80
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 59
0xd : XOR
0x4 : PUSH 105
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 115
0xd : XOR
0x4 : PUSH 54
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 70
0xd : XOR
0x4 : PUSH 18
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 31
0xd : XOR
0x4 : PUSH 80
0x6 : CMP
0x4 : PUSH 68
0xe : JMP_IF_FALSE
0x4 : PUSH 78
0x5 : PRINT
0x4 : PUSH 105
0x5 : PRINT
0x4 : PUSH 99
0x5 : PRINT
0x4 : PUSH 101
0x5 : PRINT
0x4 : PUSH 33
0x5 : PRINT
0x4 : PUSH 10
0x5 : PRINT
0xc : HALT
0x0 : ADD
0x1 : SUB
0x3 : DIV

Process finished with exit code 0
```

## 5. Commentage du bytecode en utilisant le script en C

```plain text
/Users/rhenar/CLionProjects/VMEvalReverse/main

== Injection de "pwd pls ^^ : " et affichage du prompt du password 

[EXEC] Executing opcode: PUSH at position 0
[EXEC] Executing opcode: PUSH at position 2
[EXEC] Executing opcode: PUSH at position 4
[EXEC] Executing opcode: PUSH at position 6
[EXEC] Executing opcode: PUSH at position 8
[EXEC] Executing opcode: PUSH at position 10
[EXEC] Executing opcode: PUSH at position 12
[EXEC] Executing opcode: PUSH at position 14
[EXEC] Executing opcode: PUSH at position 16
[EXEC] Executing opcode: PUSH at position 18
[EXEC] Executing opcode: PUSH at position 20
[EXEC] Executing opcode: PUSH at position 22
[EXEC] Executing opcode: PUSH at position 24
[EXEC] Executing opcode: SWAP at position 26
[EXEC] Executing opcode: PUSH at position 27
[EXEC] Executing opcode: XOR at position 29
With/ XOR: 56
XOR: 72
[EXEC] Executing opcode: PRINT at position 30
p[EXEC] Executing opcode: PUSH at position 31
[EXEC] Executing opcode: SWAP at position 33
[EXEC] Executing opcode: SUBSTRACTION at position 34
[EXEC] Executing opcode: DUPLICATE at position 35
[EXEC] Executing opcode: PUSH at position 36
[EXEC] Executing opcode: COMPARISON at position 38
[DEBUG] Comparison: 0 == 11 -> FALSE
[EXEC] Executing opcode: PUSH at position 39
[EXEC] Executing opcode: JUMP_IF_FALSE at position 41
[DEBUG] Jumping to 26 (Condition Not Met)
[EXEC] Executing opcode: SWAP at position 26
[EXEC] Executing opcode: PUSH at position 27
[EXEC] Executing opcode: XOR at position 29
With/ XOR: 56
XOR: 79
[EXEC] Executing opcode: PRINT at position 30
w[EXEC] Executing opcode: PUSH at position 31
[EXEC] Executing opcode: SWAP at position 33
[EXEC] Executing opcode: SUBSTRACTION at position 34
[EXEC] Executing opcode: DUPLICATE at position 35
[EXEC] Executing opcode: PUSH at position 36
[EXEC] Executing opcode: COMPARISON at position 38
[DEBUG] Comparison: 0 == 10 -> FALSE
[EXEC] Executing opcode: PUSH at position 39
[EXEC] Executing opcode: JUMP_IF_FALSE at position 41
[DEBUG] Jumping to 26 (Condition Not Met)
[EXEC] Executing opcode: SWAP at position 26
[EXEC] Executing opcode: PUSH at position 27
[EXEC] Executing opcode: XOR at position 29
With/ XOR: 56
XOR: 92
[EXEC] Executing opcode: PRINT at position 30
d[EXEC] Executing opcode: PUSH at position 31
[EXEC] Executing opcode: SWAP at position 33
[EXEC] Executing opcode: SUBSTRACTION at position 34
[EXEC] Executing opcode: DUPLICATE at position 35
[EXEC] Executing opcode: PUSH at position 36
[EXEC] Executing opcode: COMPARISON at position 38
[DEBUG] Comparison: 0 == 9 -> FALSE
[EXEC] Executing opcode: PUSH at position 39
[EXEC] Executing opcode: JUMP_IF_FALSE at position 41
[DEBUG] Jumping to 26 (Condition Not Met)
[EXEC] Executing opcode: SWAP at position 26
[EXEC] Executing opcode: PUSH at position 27
[EXEC] Executing opcode: XOR at position 29
With/ XOR: 56
XOR: 24
[EXEC] Executing opcode: PRINT at position 30
 [EXEC] Executing opcode: PUSH at position 31
[EXEC] Executing opcode: SWAP at position 33
[EXEC] Executing opcode: SUBSTRACTION at position 34
[EXEC] Executing opcode: DUPLICATE at position 35
[EXEC] Executing opcode: PUSH at position 36
[EXEC] Executing opcode: COMPARISON at position 38
[DEBUG] Comparison: 0 == 8 -> FALSE
[EXEC] Executing opcode: PUSH at position 39
[EXEC] Executing opcode: JUMP_IF_FALSE at position 41
[DEBUG] Jumping to 26 (Condition Not Met)
[EXEC] Executing opcode: SWAP at position 26
[EXEC] Executing opcode: PUSH at position 27
[EXEC] Executing opcode: XOR at position 29
With/ XOR: 56
XOR: 72
[EXEC] Executing opcode: PRINT at position 30
p[EXEC] Executing opcode: PUSH at position 31
[EXEC] Executing opcode: SWAP at position 33
[EXEC] Executing opcode: SUBSTRACTION at position 34
[EXEC] Executing opcode: DUPLICATE at position 35
[EXEC] Executing opcode: PUSH at position 36
[EXEC] Executing opcode: COMPARISON at position 38
[DEBUG] Comparison: 0 == 7 -> FALSE
[EXEC] Executing opcode: PUSH at position 39
[EXEC] Executing opcode: JUMP_IF_FALSE at position 41
[DEBUG] Jumping to 26 (Condition Not Met)
[EXEC] Executing opcode: SWAP at position 26
[EXEC] Executing opcode: PUSH at position 27
[EXEC] Executing opcode: XOR at position 29
With/ XOR: 56
XOR: 84
[EXEC] Executing opcode: PRINT at position 30
l[EXEC] Executing opcode: PUSH at position 31
[EXEC] Executing opcode: SWAP at position 33
[EXEC] Executing opcode: SUBSTRACTION at position 34
[EXEC] Executing opcode: DUPLICATE at position 35
[EXEC] Executing opcode: PUSH at position 36
[EXEC] Executing opcode: COMPARISON at position 38
[DEBUG] Comparison: 0 == 6 -> FALSE
[EXEC] Executing opcode: PUSH at position 39
[EXEC] Executing opcode: JUMP_IF_FALSE at position 41
[DEBUG] Jumping to 26 (Condition Not Met)
[EXEC] Executing opcode: SWAP at position 26
[EXEC] Executing opcode: PUSH at position 27
[EXEC] Executing opcode: XOR at position 29
With/ XOR: 56
XOR: 75
[EXEC] Executing opcode: PRINT at position 30
s[EXEC] Executing opcode: PUSH at position 31
[EXEC] Executing opcode: SWAP at position 33
[EXEC] Executing opcode: SUBSTRACTION at position 34
[EXEC] Executing opcode: DUPLICATE at position 35
[EXEC] Executing opcode: PUSH at position 36
[EXEC] Executing opcode: COMPARISON at position 38
[DEBUG] Comparison: 0 == 5 -> FALSE
[EXEC] Executing opcode: PUSH at position 39
[EXEC] Executing opcode: JUMP_IF_FALSE at position 41
[DEBUG] Jumping to 26 (Condition Not Met)
[EXEC] Executing opcode: SWAP at position 26
[EXEC] Executing opcode: PUSH at position 27
[EXEC] Executing opcode: XOR at position 29
With/ XOR: 56
XOR: 24
[EXEC] Executing opcode: PRINT at position 30
 [EXEC] Executing opcode: PUSH at position 31
[EXEC] Executing opcode: SWAP at position 33
[EXEC] Executing opcode: SUBSTRACTION at position 34
[EXEC] Executing opcode: DUPLICATE at position 35
[EXEC] Executing opcode: PUSH at position 36
[EXEC] Executing opcode: COMPARISON at position 38
[DEBUG] Comparison: 0 == 4 -> FALSE
[EXEC] Executing opcode: PUSH at position 39
[EXEC] Executing opcode: JUMP_IF_FALSE at position 41
[DEBUG] Jumping to 26 (Condition Not Met)
[EXEC] Executing opcode: SWAP at position 26
[EXEC] Executing opcode: PUSH at position 27
[EXEC] Executing opcode: XOR at position 29
With/ XOR: 56
XOR: 102
[EXEC] Executing opcode: PRINT at position 30
^[EXEC] Executing opcode: PUSH at position 31
[EXEC] Executing opcode: SWAP at position 33
[EXEC] Executing opcode: SUBSTRACTION at position 34
[EXEC] Executing opcode: DUPLICATE at position 35
[EXEC] Executing opcode: PUSH at position 36
[EXEC] Executing opcode: COMPARISON at position 38
[DEBUG] Comparison: 0 == 3 -> FALSE
[EXEC] Executing opcode: PUSH at position 39
[EXEC] Executing opcode: JUMP_IF_FALSE at position 41
[DEBUG] Jumping to 26 (Condition Not Met)
[EXEC] Executing opcode: SWAP at position 26
[EXEC] Executing opcode: PUSH at position 27
[EXEC] Executing opcode: XOR at position 29
With/ XOR: 56
XOR: 102
[EXEC] Executing opcode: PRINT at position 30
^[EXEC] Executing opcode: PUSH at position 31
[EXEC] Executing opcode: SWAP at position 33
[EXEC] Executing opcode: SUBSTRACTION at position 34
[EXEC] Executing opcode: DUPLICATE at position 35
[EXEC] Executing opcode: PUSH at position 36
[EXEC] Executing opcode: COMPARISON at position 38
[DEBUG] Comparison: 0 == 2 -> FALSE
[EXEC] Executing opcode: PUSH at position 39
[EXEC] Executing opcode: JUMP_IF_FALSE at position 41
[DEBUG] Jumping to 26 (Condition Not Met)
[EXEC] Executing opcode: SWAP at position 26
[EXEC] Executing opcode: PUSH at position 27
[EXEC] Executing opcode: XOR at position 29
With/ XOR: 56
XOR: 24
[EXEC] Executing opcode: PRINT at position 30
 [EXEC] Executing opcode: PUSH at position 31
[EXEC] Executing opcode: SWAP at position 33
[EXEC] Executing opcode: SUBSTRACTION at position 34
[EXEC] Executing opcode: DUPLICATE at position 35
[EXEC] Executing opcode: PUSH at position 36
[EXEC] Executing opcode: COMPARISON at position 38
[DEBUG] Comparison: 0 == 1 -> FALSE
[EXEC] Executing opcode: PUSH at position 39
[EXEC] Executing opcode: JUMP_IF_FALSE at position 41
[DEBUG] Jumping to 26 (Condition Not Met)
[EXEC] Executing opcode: SWAP at position 26
[EXEC] Executing opcode: PUSH at position 27
[EXEC] Executing opcode: XOR at position 29
With/ XOR: 56
XOR: 2
[EXEC] Executing opcode: PRINT at position 30
:[EXEC] Executing opcode: PUSH at position 31
[EXEC] Executing opcode: SWAP at position 33
[EXEC] Executing opcode: SUBSTRACTION at position 34
[EXEC] Executing opcode: DUPLICATE at position 35
[EXEC] Executing opcode: PUSH at position 36
[EXEC] Executing opcode: COMPARISON at position 38
[DEBUG] Comparison: 0 == 0 -> TRUE
[EXEC] Executing opcode: PUSH at position 39
[EXEC] Executing opcode: JUMP_IF_FALSE at position 41
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 42
[EXEC] Executing opcode: PRINT at position 44

== Fin d'affichage de "pwd pls ^^ : " et en attente de l'entrée utilisateur

[EXEC] Executing opcode: PUSH at position 45
[EXEC] Executing opcode: READ at position 47
OTERIA{AlienTechIsTheNewMeta}

== Récupère l'entrée et vient l'injecter tant qu'il ne voit pas le caractère de retour à la ligne (10)

[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 79 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 84 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 69 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 82 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 73 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 65 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 123 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 65 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 108 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 105 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 101 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 110 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 84 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 101 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 99 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 104 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 73 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 115 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 84 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 104 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 101 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 78 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 101 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 119 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 77 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 101 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 116 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 97 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 125 -> FALSE
[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Jumping to 47 (Condition Not Met)
[EXEC] Executing opcode: READ at position 47
[EXEC] Executing opcode: DUPLICATE at position 48
[EXEC] Executing opcode: PUSH at position 49
[EXEC] Executing opcode: COMPARISON at position 51
[DEBUG] Comparison: 10 == 10 -> TRUE

== A terminé de mettre l'entrée utilisateur dans la stack et va maintenant vérifier qu'il fait 29 + 1 caractère (retour à la ligne)

[EXEC] Executing opcode: SWAP at position 52
[EXEC] Executing opcode: PUSH at position 53
[EXEC] Executing opcode: ADDITION at position 55
[EXEC] Executing opcode: PUSH at position 56
[EXEC] Executing opcode: JUMP_IF_FALSE at position 58
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 59
[EXEC] Executing opcode: COMPARISON at position 61
[DEBUG] Comparison: 30 == 30 -> TRUE
[EXEC] Executing opcode: PUSH at position 62
[EXEC] Executing opcode: JUMP_IF_FALSE at position 64
[DEBUG] Skipping jump (Condition Met)

== A vérifié que l'entrée fait 30 caractères et commence à venir XOR les caractères entrée et vérifier qu'ils correspondent à ceux présent dans la stack

[EXEC] Executing opcode: PUSH at position 65
[EXEC] Executing opcode: JUMP at position 67
[EXEC] Executing opcode: POP at position 84
[EXEC] Executing opcode: PUSH at position 85
[EXEC] Executing opcode: XOR at position 87
With/ XOR: 45
XOR: 125
[EXEC] Executing opcode: PUSH at position 88
[EXEC] Executing opcode: COMPARISON at position 90

== Comparaison du caractére inscrit dans la stack avec le caractère xoré entré par l'utilisateur

[DEBUG] Comparison: 80 == 80 -> TRUE
[EXEC] Executing opcode: PUSH at position 91
[EXEC] Executing opcode: JUMP_IF_FALSE at position 93
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 94
[EXEC] Executing opcode: XOR at position 96
With/ XOR: 8
XOR: 97
[EXEC] Executing opcode: PUSH at position 97
[EXEC] Executing opcode: COMPARISON at position 99
[DEBUG] Comparison: 105 == 105 -> TRUE
[EXEC] Executing opcode: PUSH at position 100
[EXEC] Executing opcode: JUMP_IF_FALSE at position 102
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 103
[EXEC] Executing opcode: XOR at position 105
With/ XOR: 66
XOR: 116
[EXEC] Executing opcode: PUSH at position 106
[EXEC] Executing opcode: COMPARISON at position 108
[DEBUG] Comparison: 54 == 54 -> TRUE
[EXEC] Executing opcode: PUSH at position 109
[EXEC] Executing opcode: JUMP_IF_FALSE at position 111
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 112
[EXEC] Executing opcode: XOR at position 114
With/ XOR: 119
XOR: 101
[EXEC] Executing opcode: PUSH at position 115
[EXEC] Executing opcode: COMPARISON at position 117
[DEBUG] Comparison: 18 == 18 -> TRUE
[EXEC] Executing opcode: PUSH at position 118
[EXEC] Executing opcode: JUMP_IF_FALSE at position 120
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 121
[EXEC] Executing opcode: XOR at position 123
With/ XOR: 29
XOR: 77
[EXEC] Executing opcode: PUSH at position 124
[EXEC] Executing opcode: COMPARISON at position 126
[DEBUG] Comparison: 80 == 80 -> TRUE
[EXEC] Executing opcode: PUSH at position 127
[EXEC] Executing opcode: JUMP_IF_FALSE at position 129
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 130
[EXEC] Executing opcode: XOR at position 132
With/ XOR: 30
XOR: 119
[EXEC] Executing opcode: PUSH at position 133
[EXEC] Executing opcode: COMPARISON at position 135
[DEBUG] Comparison: 105 == 105 -> TRUE
[EXEC] Executing opcode: PUSH at position 136
[EXEC] Executing opcode: JUMP_IF_FALSE at position 138
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 139
[EXEC] Executing opcode: XOR at position 141
With/ XOR: 83
XOR: 101
[EXEC] Executing opcode: PUSH at position 142
[EXEC] Executing opcode: COMPARISON at position 144
[DEBUG] Comparison: 54 == 54 -> TRUE
[EXEC] Executing opcode: PUSH at position 145
[EXEC] Executing opcode: JUMP_IF_FALSE at position 147
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 148
[EXEC] Executing opcode: XOR at position 150
With/ XOR: 92
XOR: 78
[EXEC] Executing opcode: PUSH at position 151
[EXEC] Executing opcode: COMPARISON at position 153
[DEBUG] Comparison: 18 == 18 -> TRUE
[EXEC] Executing opcode: PUSH at position 154
[EXEC] Executing opcode: JUMP_IF_FALSE at position 156
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 157
[EXEC] Executing opcode: XOR at position 159
With/ XOR: 53
XOR: 101
[EXEC] Executing opcode: PUSH at position 160
[EXEC] Executing opcode: COMPARISON at position 162
[DEBUG] Comparison: 80 == 80 -> TRUE
[EXEC] Executing opcode: PUSH at position 163
[EXEC] Executing opcode: JUMP_IF_FALSE at position 165
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 166
[EXEC] Executing opcode: XOR at position 168
With/ XOR: 1
XOR: 104
[EXEC] Executing opcode: PUSH at position 169
[EXEC] Executing opcode: COMPARISON at position 171
[DEBUG] Comparison: 105 == 105 -> TRUE
[EXEC] Executing opcode: PUSH at position 172
[EXEC] Executing opcode: JUMP_IF_FALSE at position 174
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 175
[EXEC] Executing opcode: XOR at position 177
With/ XOR: 98
XOR: 84
[EXEC] Executing opcode: PUSH at position 178
[EXEC] Executing opcode: COMPARISON at position 180
[DEBUG] Comparison: 54 == 54 -> TRUE
[EXEC] Executing opcode: PUSH at position 181
[EXEC] Executing opcode: JUMP_IF_FALSE at position 183
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 184
[EXEC] Executing opcode: XOR at position 186
With/ XOR: 97
XOR: 115
[EXEC] Executing opcode: PUSH at position 187
[EXEC] Executing opcode: COMPARISON at position 189
[DEBUG] Comparison: 18 == 18 -> TRUE
[EXEC] Executing opcode: PUSH at position 190
[EXEC] Executing opcode: JUMP_IF_FALSE at position 192
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 193
[EXEC] Executing opcode: XOR at position 195
With/ XOR: 25
XOR: 73
[EXEC] Executing opcode: PUSH at position 196
[EXEC] Executing opcode: COMPARISON at position 198
[DEBUG] Comparison: 80 == 80 -> TRUE
[EXEC] Executing opcode: PUSH at position 199
[EXEC] Executing opcode: JUMP_IF_FALSE at position 201
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 202
[EXEC] Executing opcode: XOR at position 204
With/ XOR: 1
XOR: 104
[EXEC] Executing opcode: PUSH at position 205
[EXEC] Executing opcode: COMPARISON at position 207
[DEBUG] Comparison: 105 == 105 -> TRUE
[EXEC] Executing opcode: PUSH at position 208
[EXEC] Executing opcode: JUMP_IF_FALSE at position 210
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 211
[EXEC] Executing opcode: XOR at position 213
With/ XOR: 85
XOR: 99
[EXEC] Executing opcode: PUSH at position 214
[EXEC] Executing opcode: COMPARISON at position 216
[DEBUG] Comparison: 54 == 54 -> TRUE
[EXEC] Executing opcode: PUSH at position 217
[EXEC] Executing opcode: JUMP_IF_FALSE at position 219
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 220
[EXEC] Executing opcode: XOR at position 222
With/ XOR: 119
XOR: 101
[EXEC] Executing opcode: PUSH at position 223
[EXEC] Executing opcode: COMPARISON at position 225
[DEBUG] Comparison: 18 == 18 -> TRUE
[EXEC] Executing opcode: PUSH at position 226
[EXEC] Executing opcode: JUMP_IF_FALSE at position 228
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 229
[EXEC] Executing opcode: XOR at position 231
With/ XOR: 4
XOR: 84
[EXEC] Executing opcode: PUSH at position 232
[EXEC] Executing opcode: COMPARISON at position 234
[DEBUG] Comparison: 80 == 80 -> TRUE
[EXEC] Executing opcode: PUSH at position 235
[EXEC] Executing opcode: JUMP_IF_FALSE at position 237
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 238
[EXEC] Executing opcode: XOR at position 240
With/ XOR: 7
XOR: 110
[EXEC] Executing opcode: PUSH at position 241
[EXEC] Executing opcode: COMPARISON at position 243
[DEBUG] Comparison: 105 == 105 -> TRUE
[EXEC] Executing opcode: PUSH at position 244
[EXEC] Executing opcode: JUMP_IF_FALSE at position 246
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 247
[EXEC] Executing opcode: XOR at position 249
With/ XOR: 83
XOR: 101
[EXEC] Executing opcode: PUSH at position 250
[EXEC] Executing opcode: COMPARISON at position 252
[DEBUG] Comparison: 54 == 54 -> TRUE
[EXEC] Executing opcode: PUSH at position 253
[EXEC] Executing opcode: JUMP_IF_FALSE at position 255
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 256
[EXEC] Executing opcode: XOR at position 258
With/ XOR: 123
XOR: 105
[EXEC] Executing opcode: PUSH at position 259
[EXEC] Executing opcode: COMPARISON at position 261
[DEBUG] Comparison: 18 == 18 -> TRUE
[EXEC] Executing opcode: PUSH at position 262
[EXEC] Executing opcode: JUMP_IF_FALSE at position 264
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 265
[EXEC] Executing opcode: XOR at position 267
With/ XOR: 60
XOR: 108
[EXEC] Executing opcode: PUSH at position 268
[EXEC] Executing opcode: COMPARISON at position 270
[DEBUG] Comparison: 80 == 80 -> TRUE
[EXEC] Executing opcode: PUSH at position 271
[EXEC] Executing opcode: JUMP_IF_FALSE at position 273
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 274
[EXEC] Executing opcode: XOR at position 276
With/ XOR: 40
XOR: 65
[EXEC] Executing opcode: PUSH at position 277
[EXEC] Executing opcode: COMPARISON at position 279
[DEBUG] Comparison: 105 == 105 -> TRUE
[EXEC] Executing opcode: PUSH at position 280
[EXEC] Executing opcode: JUMP_IF_FALSE at position 282
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 283
[EXEC] Executing opcode: XOR at position 285
With/ XOR: 77
XOR: 123
[EXEC] Executing opcode: PUSH at position 286
[EXEC] Executing opcode: COMPARISON at position 288
[DEBUG] Comparison: 54 == 54 -> TRUE
[EXEC] Executing opcode: PUSH at position 289
[EXEC] Executing opcode: JUMP_IF_FALSE at position 291
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 292
[EXEC] Executing opcode: XOR at position 294
With/ XOR: 83
XOR: 65
[EXEC] Executing opcode: PUSH at position 295
[EXEC] Executing opcode: COMPARISON at position 297
[DEBUG] Comparison: 18 == 18 -> TRUE
[EXEC] Executing opcode: PUSH at position 298
[EXEC] Executing opcode: JUMP_IF_FALSE at position 300
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 301
[EXEC] Executing opcode: XOR at position 303
With/ XOR: 25
XOR: 73
[EXEC] Executing opcode: PUSH at position 304
[EXEC] Executing opcode: COMPARISON at position 306
[DEBUG] Comparison: 80 == 80 -> TRUE
[EXEC] Executing opcode: PUSH at position 307
[EXEC] Executing opcode: JUMP_IF_FALSE at position 309
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 310
[EXEC] Executing opcode: XOR at position 312
With/ XOR: 59
XOR: 82
[EXEC] Executing opcode: PUSH at position 313
[EXEC] Executing opcode: COMPARISON at position 315
[DEBUG] Comparison: 105 == 105 -> TRUE
[EXEC] Executing opcode: PUSH at position 316
[EXEC] Executing opcode: JUMP_IF_FALSE at position 318
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 319
[EXEC] Executing opcode: XOR at position 321
With/ XOR: 115
XOR: 69
[EXEC] Executing opcode: PUSH at position 322
[EXEC] Executing opcode: COMPARISON at position 324
[DEBUG] Comparison: 54 == 54 -> TRUE
[EXEC] Executing opcode: PUSH at position 325
[EXEC] Executing opcode: JUMP_IF_FALSE at position 327
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 328
[EXEC] Executing opcode: XOR at position 330
With/ XOR: 70
XOR: 84
[EXEC] Executing opcode: PUSH at position 331
[EXEC] Executing opcode: COMPARISON at position 333
[DEBUG] Comparison: 18 == 18 -> TRUE
[EXEC] Executing opcode: PUSH at position 334
[EXEC] Executing opcode: JUMP_IF_FALSE at position 336
[DEBUG] Skipping jump (Condition Met)
[EXEC] Executing opcode: PUSH at position 337
[EXEC] Executing opcode: XOR at position 339
With/ XOR: 31
XOR: 79
[EXEC] Executing opcode: PUSH at position 340
[EXEC] Executing opcode: COMPARISON at position 342
[DEBUG] Comparison: 80 == 80 -> TRUE
[EXEC] Executing opcode: PUSH at position 343
[EXEC] Executing opcode: JUMP_IF_FALSE at position 345
[DEBUG] Skipping jump (Condition Met)

== Toute les conditions sont réunis et le mot de passe est complet, affichage de "Nice"

[EXEC] Executing opcode: PUSH at position 346
[EXEC] Executing opcode: PRINT at position 348
N[EXEC] Executing opcode: PUSH at position 349
[EXEC] Executing opcode: PRINT at position 351
i[EXEC] Executing opcode: PUSH at position 352
[EXEC] Executing opcode: PRINT at position 354
c[EXEC] Executing opcode: PUSH at position 355
[EXEC] Executing opcode: PRINT at position 357
e[EXEC] Executing opcode: PUSH at position 358
[EXEC] Executing opcode: PRINT at position 360
![EXEC] Executing opcode: PUSH at position 361
[EXEC] Executing opcode: PRINT at position 363

[EXEC] Executing opcode: HALT at position 364

Process finished with exit code 0

```


## 6. Script simili-C de la VM

Le script en C suivant reproduit le code de la VM :

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdbool.h>

typedef struct {
    int instruction_pointer;
    int stack_pointer;
    char DATA[100];
    bool cmp_flag;
} astruct;

void print_stack(astruct *data) {
    printf("\n[STACK] State: ");
    for (int i = 0; i < data->stack_pointer; i++) {
        printf("[%d] ", data->DATA[i]);
    }
    printf("\n");
}

char pop(astruct *data) {
    printf("[POP] Popping %d from stack at position %d\n", data->DATA[data->stack_pointer - 1], data->stack_pointer - 1);
    data->stack_pointer--;
    print_stack(data);
    return data->DATA[data->stack_pointer];
}

void push(astruct *stack_pointer, char DATA) {
    printf("[PUSH] Pushing %d to stack at position %d\n", DATA, stack_pointer->stack_pointer);
    stack_pointer->DATA[stack_pointer->stack_pointer] = DATA;
    stack_pointer->stack_pointer++;
    print_stack(stack_pointer);
}

void execute_bytecode(unsigned char *bytecode) {
    astruct *rom = (astruct *)malloc(sizeof(astruct));
    rom->instruction_pointer = 0;
    rom->stack_pointer = 0;
    memset(rom->DATA, 0, 100);

    while (1) {
        unsigned char op_code = bytecode[rom->instruction_pointer++];
        char cVar1, cVar2;
        if (op_code == 0x04) {
            printf("[EXEC] Executing opcode: PUSH at position %d\n", rom->instruction_pointer - 1);
        } else if (op_code == 0x00) {
            printf("[EXEC] Executing opcode: ADDITION at position %d\n", rom->instruction_pointer - 1);
        } else if (op_code == 0x01) {
            printf("[EXEC] Executing opcode: SUBSTRACTION at position %d\n", rom->instruction_pointer - 1);
        } else if (op_code == 0x02) {
            printf("[EXEC] Executing opcode: MULTIPLICATION at position %d\n", rom->instruction_pointer - 1);
        } else if (op_code == 0x03) {
            printf("[EXEC] Executing opcode: DIVISION at position %d\n", rom->instruction_pointer - 1);
        } else if (op_code == 0x05) {
            printf("[EXEC] Executing opcode: PRINT at position %d\n", rom->instruction_pointer - 1);
        } else if (op_code == 0x06) {
            printf("[EXEC] Executing opcode: COMPARISON at position %d\n", rom->instruction_pointer - 1);
        } else if (op_code == 0x07) {
            printf("[EXEC] Executing opcode: JUMP at position %d\n", rom->instruction_pointer - 1);
        } else if (op_code == 0x08) {
            printf("[EXEC] Executing opcode: JUMP_IF_TRUE at position %d\n", rom->instruction_pointer - 1);
        } else if (op_code == 0x09) {
            printf("[EXEC] Executing opcode: READ at position %d\n", rom->instruction_pointer - 1);
        } else if (op_code == 0x0A) {
            printf("[EXEC] Executing opcode: DUPLICATE at position %d\n", rom->instruction_pointer - 1);
        } else if (op_code == 0x0B) {
            printf("[EXEC] Executing opcode: SWAP at position %d\n", rom->instruction_pointer - 1);
        } else if (op_code == 0x0C) {
            printf("[EXEC] Executing opcode: HALT at position %d\n", rom->instruction_pointer - 1);
        } else if (op_code == 0x0D) {
            printf("[EXEC] Executing opcode: XOR at position %d\n", rom->instruction_pointer - 1);
        } else if (op_code == 0x0E) {
            printf("[EXEC] Executing opcode: JUMP_IF_FALSE at position %d\n", rom->instruction_pointer - 1);
        } else if (op_code == 0x0F) {
            printf("[EXEC] Executing opcode: POP at position %d\n", rom->instruction_pointer - 1);
        }

        switch (op_code) {
            case 0x00:
                cVar1 = pop(rom);
                cVar2 = pop(rom);
                push(rom, cVar1 + cVar2);
                break;
            case 0x01:
                cVar1 = pop(rom);
                cVar2 = pop(rom);
                push(rom, cVar1 - cVar2);
                break;
            case 0x02:
                cVar1 = pop(rom);
                cVar2 = pop(rom);
                push(rom, cVar1 * cVar2);
                break;
            case 0x03:
                cVar1 = pop(rom);
                cVar2 = pop(rom);
                push(rom, cVar1 / cVar2);
                break;
            case 0x04:
                cVar1 = bytecode[rom->instruction_pointer++];
                push(rom, cVar1);
                break;
            case 0x05:
                putchar(pop(rom));
                break;
            case 0x06:  // Comparaison
                cVar1 = pop(rom);
                cVar2 = pop(rom);
                rom->cmp_flag = (cVar1 == cVar2);
                printf("[DEBUG] Comparison: %d == %d -> %s\n", cVar1, cVar2, rom->cmp_flag ? "TRUE" : "FALSE");
                break;
            case 0x07:
                rom->instruction_pointer = pop(rom);
                break;
            case 0x08:  // Saut conditionnel si vrai
                cVar1 = pop(rom);
                if (rom->cmp_flag) {
                    printf("[DEBUG] Jumping to %d (Condition Met)\n", cVar1);
                    rom->instruction_pointer = cVar1;
                } else {
                    printf("[DEBUG] Skipping jump (Condition Not Met)\n");
                }
                break;
            case 0x09:
                push(rom, getchar());
                break;
            case 0x0A:
                cVar1 = pop(rom);
                push(rom, cVar1);
                push(rom, cVar1);
                break;
            case 0x0B:
                cVar1 = pop(rom);
                cVar2 = pop(rom);
                push(rom, cVar1);
                push(rom, cVar2);
                break;
            case 0x0C:
                free(rom);
                return;
            case 0x0D:
                cVar1 = pop(rom);
                printf("With/ XOR: %d\n", cVar1);
                cVar2 = pop(rom);
                printf("XOR: %d\n", cVar2);
                push(rom, cVar1 ^ cVar2);
                break;
            case 0x0E:  // Saut conditionnel si faux
                cVar1 = pop(rom);
                if (!rom->cmp_flag) {
                    printf("[DEBUG] Jumping to %d (Condition Not Met)\n", cVar1);
                    rom->instruction_pointer = cVar1;
                } else {
                    printf("[DEBUG] Skipping jump (Condition Met)\n");
                }
                break;
            case 0x0F:
                rom->stack_pointer--;
                break;
        }
    }
}

int main() {
    unsigned char bytecode[] = {
            0x04, 0x02, 0x04, 0x18, 0x04, 0x66, 0x04, 0x66, 0x04, 0x18, 0x04, 0x4b, 0x04, 0x54, 0x04, 0x48, 0x04, 0x18, 0x04, 0x5c, 0x04, 0x4f, 0x04, 0x48, 0x04, 0x0c, 0x0b, 0x04, 0x38, 0x0d, 0x05, 0x04, 0x01, 0x0b, 0x01, 0x0a, 0x04, 0x00, 0x06, 0x04, 0x1a, 0x0e, 0x04, 0x0a, 0x05, 0x04, 0x00, 0x09, 0x0a, 0x04, 0x0a, 0x06, 0x0b, 0x04, 0x01, 0x00, 0x04, 0x2f, 0x0e, 0x04, 0x1e, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x54, 0x07, 0x04, 0x4e, 0x05, 0x04, 0x6f, 0x05, 0x04, 0x70, 0x05, 0x04, 0x65, 0x05, 0x04, 0x0a, 0x0c, 0x0f, 0x0f, 0x04, 0x2d, 0x0d, 0x04, 0x50, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x08,
        0x0d, 0x04, 0x69, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x42, 0x0d, 0x04, 0x36, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x77, 0x0d, 0x04, 0x12, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x1d, 0x0d, 0x04, 0x50, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x1e, 0x0d, 0x04, 0x69, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x53, 0x0d, 0x04, 0x36, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x5c, 0x0d, 0x04, 0x12, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x35, 0x0d, 0x04, 0x50, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x01, 0x0d, 0x04, 0x69, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x62, 0x0d, 0x04, 0x36, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x61, 0x0d, 0x04, 0x12, 0x06, 0x04, 0x44,
        0x0e, 0x04, 0x19, 0x0d, 0x04, 0x50, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x01, 0x0d, 0x04, 0x69, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x55, 0x0d, 0x04, 0x36, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x77, 0x0d, 0x04, 0x12, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x04, 0x0d, 0x04, 0x50, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x07, 0x0d, 0x04, 0x69, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x53, 0x0d, 0x04, 0x36, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x7b, 0x0d, 0x04, 0x12, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x3c, 0x0d, 0x04, 0x50, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x28, 0x0d, 0x04, 0x69, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x4d, 0x0d, 0x04, 0x36,
        0x06, 0x04, 0x44, 0x0e, 0x04, 0x53, 0x0d, 0x04, 0x12, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x19, 0x0d, 0x04, 0x50, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x3b, 0x0d, 0x04, 0x69, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x73, 0x0d, 0x04, 0x36, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x46, 0x0d, 0x04, 0x12, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x1f, 0x0d, 0x04, 0x50, 0x06, 0x04, 0x44, 0x0e, 0x04, 0x4e, 0x05, 0x04, 0x69, 0x05, 0x04, 0x63, 0x05, 0x04, 0x65, 0x05, 0x04, 0x21, 0x05, 0x04, 0x0a, 0x05, 0x0c, 0x00, 0x01, 0x1b, 0x03, 0x3b
        };

    execute_bytecode(bytecode);
    return 0;
}
```

---

## 6. Trouver le flag

Pour trouver le flag, on part à l'envers de celui-ci et on XOR avec l'élément de comparaison pour obtenir le flag.

```
Flag : OTERIA{AlienTechIsTheNewMeta}
```

---