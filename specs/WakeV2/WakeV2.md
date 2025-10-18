# Wake Language v2 Feature Additions

This document describes new features added to Wake, expanding its functionality while maintaining compatibility with MicroASM.

---

## 1. Variables and Constants
### Syntax
```wake
var <name> = <value>;       // Register-backed variable
const <NAME> = <value>;     // Compile-time constant
```
### Examples
```wake
var counter = 0;      // Allocates a register (e.g., R3)
const MAX = 100;      // Replaced with 100 during compilation
```
### Translation Rules
- `var`: Assigns a register (e.g., `R3`) or fixed memory address.  
  Example: `var x = 5;` → `MOV R3 5` (where `R3` is auto-selected).  
- `const`: Direct substitution.  
  Example: `add(RAX, MAX);` → `add(RAX, 100);`.

---

## 2. If-Else Statements
### Syntax
```wake
if (<condition>) {
    // True block
} else {
    // False block (optional)
}
```
### Conditions
- Supports: `==`, `!=`, `<`, `>`, `<=`, `>=` (e.g., `RAX < 10`).  
- Compound conditions with `&&`/`||` (translated to nested jumps).  

### Example
```wake
if (R1 == R2) {
    mov(RAX, 1);
} else {
    mov(RAX, 0);
}
```
### Translation
```masm
CMP R1 R2
JE #if_true
MOV RAX 0    ; False block
JMP #end_if
#if_true:
MOV RAX 1    ; True block
#end_if:
```

---

## 3. Functions with Parameters and Return Values
### Syntax
```wake
<return_type> <name>(<type> <arg>, ...) {
    // Body
    return <value>;
}
```
### Types
- Primitive: `int`, `byte` (treated as registers/memory addresses).  
- `void` for no return value.  

### Example
```wake
int add(int a, int b) {
    mov(RAX, a);
    add(RAX, b);
    return RAX;
}
```
### Translation
```masm
; Caller:
MOV R1 5     ; arg1
MOV R2 10    ; arg2
CALL #add    ; Result in RAX

; Function:
lbl add:
MOV RAX R1
ADD RAX R2
RET
```

---

## 4. While Loops
### Syntax
```wake
while (<condition>) {
    // Loop body
}
```
### Example
```wake
while (RAX > 0) {
    dec(RAX);
}
```
### Translation
```masm
lbl loop_start:
CMP RAX 0
JLE #loop_end
DEC RAX
JMP #loop_start
lbl loop_end:
```

---

## 5. Macros
### Syntax
```wake
#macro <NAME>(<args>) {
    // Body
}
```
### Example
```wake
#macro DOUBLE(x) {
    shl(x, 1);
}

DOUBLE(RAX);  // Expands to: shl(RAX, 1);
```

---

## 6. Memory Management
### Syntax
```wake
alloc(<name>, <size>);  // Dynamic allocation
free(<name>);           // Deallocation
```
### Example
```wake
alloc(buffer, 100);  // Reserves 100 bytes
mov(buffer, 42);     // $buffer is the base address
free(buffer);
```
### Translation
- `alloc` maps to a memory region (e.g., `$500`).  
- `free` marks it as reusable (no MicroASM output; runtime-dependent).  

---

## 7. Type Hints
### Syntax
```wake
<type> <name> = <value>;
```
### Example
```wake
int score = 0;
string msg = "Hello";
```
### Behavior
- No runtime overhead; used for compile-time checks.  
- `string` maps to `db` + address (e.g., `msg` → `$100`).  

---

## Example Program (v2)
```wake
#include "math.wake"

int factorial(int n) {
    var result = 1;
    while (n > 1) {
        mul(result, n);
        dec(n);
    }
    return result;
}

void main() {
    const N = 5;
    var x = factorial(N);
    if (x > 10) {
        call("printLarge");
    }
}
```

---

## Backward Compatibility
- Existing Wake v1 code remains valid.  
- New features compile to standard MicroASM.  

