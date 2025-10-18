# MicroASM Instruction Set Documentation

This document details all instructions available in the MicroASM language.

## Comments

Comments are denoted by ether single ';' or double ';;' semicolons. Comments can be placed on their own line or at the end of a line.

```asm
;; This is a comment
MOV R1 R2 ; This is also a comment
```

## Extra info

- All instructions are case-insensitive, 'mov' == 'MOV'
- All registers are 32-bit, and all values are 64-bit. this will change later
- All values are signed integers which means you can do negative numbers, but be careful with the overflow
- Micro-Assembly does not do `mov RAX, RBX`. this comes later in the language development
- Be aware of the register state before and after operations to avoid unintended behavior.
- The stack is not automatically managed, so you must push and pop values as needed.
- The stack grows downwards, so pushing values will decrement the stack pointer and popping values will increment it.
- The stack is not automatically cleared, so you must manage the stack pointer yourself.


for more infomation about MNI (Micro-Assembly native interface and memory management) see [MNI](mni.md)

## Registers

- RAX
- RBX
- RCX
- RDX
- RSI
- RDI
- RBP
- RSP
- RIP (Instruction Pointer, though you shouldn't use this besides reading it and maybe comparing it? or using it as a jump destination)
- R0 -> 15 (General Purpose Registers, use as you wish, but be careful with RSP and RBP. allways push and pop in pairs, and allways pop in the reverse order you pushed) don't be like me and forget to pop the registers you pushed, or you'll have a bad time

## Basic Instructions

### MOV (Move)

```asm
MOV dest, src
```

Copies a value from the source to the destination register.
Example: `MOV R1 R2` - Copies value from R2 to R1

### ADD (Addition)

```asm
ADD dest src
```

Adds the source value to the destination register and stores the result in the destination.
Example: `ADD R1 R2` - R1 = R1 + R2

### SUB (Subtraction)

```asm
SUB dest src
```

Subtracts the source value from the destination register and stores the result in the destination.
Example: `SUB R1 R2` - R1 = R1 - R2

### MUL (Multiplication)

```asm
MUL dest src
```
Multiplies the destination register by the source value and stores the result in the destination.
Example: `MUL R1 R2` - R1 = R1 * R2

### DIV (Division)

```asm
DIV dest src
```

Divides the destination register by the source value and stores the result in the destination.
Example: `DIV R1 R2` - R1 = R1 / R2

### INC (Increment)

```asm
INC dest
```

Increments the value in the destination register by 1.
Example: `INC R1` - R1 = R1 + 1

### DEC (Decrement)

```asm
DEC dest
```

Decrements the value in the destination register by 1.
Example: `DEC R1` - R1 = R1 - 1

## logic instructions

### AND (Logical AND)

```asm
AND dest src
```

Performs a logical AND operation between the two source values and stores the result in the destination.
Example: `AND R1 R2` - R1 = R1 & R2

### OR (Logical OR)

```asm
OR dest src
```

Performs a logical OR operation between the two source values and stores the result in the destination.
Example: `OR R1 R2` - R1 = R1 | R2  

### XOR (Logical XOR)

```asm
XOR dest src
```

Performs a logical XOR operation between the two source values and stores the result in the destination.
Example: `XOR R1 R2` - R1 = R1 ^ R2

### NOT (Logical NOT)

```asm
NOT dest
```

Performs a logical NOT operation on the destination register.
Example: `NOT R1` - R1 = ~R1

## Comparison Instructions

### CMP (Compare)

```asm
CMP dest, src
```

Compares two values and sets internal flags for conditional jumps.
Example: `CMP R1 R2` - Compares R1 with R2

### JE (Jump if Equal)

```asm
JE label_true label_false
```

Jumps to label_true if the previous comparison was equal, otherwise jumps to label_false.
Example: `JE #equal #not_equal`

### JL (Jump if Less)

```asm
JL label
```

Jumps to the specified label if the previous comparison result was "less than".
Example: `JL #less`

### CALL (Call Function)

```asm
CALL label
```

Calls a function at the specified label.
Example: `CALL #function`
or calls external code if not marked with a #
Example: `CALL $function`

Note: Unlike JMP, CALL saves the current instruction pointer (RIP) on the stack before jumping to the label. This allows the function to return to the point where it was called using the RET instruction.

## Stack Operations

### PUSH

```asm
PUSH src
```

Pushes a value onto the stack.
Example: `PUSH R1`

### POP

```asm
POP dest
```

Pops a value from the stack into the destination register.
Example: `POP R1`

## I/O Operations

### OUT

```asm
OUT port value
```

Outputs a value or string (prefixed with $) to 1. stdout, 2. stderr with a newline.
Example: `OUT 2 R1` or `OUT 1 $500`

### COUT

```asm
COUT port value
```

Outputs a single character value to stdout folowing the ASCII table and OUT rules.
Example: `COUT 1 R1` or `COUT 2 $65`

## Program Control

### HLT (Halt)

```asm
HLT
```

Stops program execution and returns to the operating system.

### EXIT

```asm
EXIT code
```

Exits the program with the specified return code.
Example: `EXIT R1`

## Command Line Arguments

### ARGC

```asm
ARGC dest
```

Gets the count of command line arguments into the destination register.
Example: `ARGC R1`

### GETARG

```asm
GETARG dest index
```

Gets the command line argument at the specified index into the destination register.
Example: `GETARG R1 R2`

in jmasm, the index is 0-based and comes after the launched program name, unlike how csharp wants -- --<argument> or -<argument> or <argument> to be the passing to the program

## Data Definition

### DB (Define Bytes)

```asm
DB address "string"
```

Defines a string constant at the specified address.
Example: `DB $1 "Hello, World!"` (using $ prefix)

## Labels

### LBL (Label)

```asm
LBL name
```

Defines a label that can be jumped to.
Example: `LBL loop`

## Notes

- Registers are referenced as R0, R1, R2, etc.
- String constants can be defined using DB and referenced with a $ prefix
- Labels are referenced with a # prefix in jump instructions
- All numeric values are treated as 64-bit integers
