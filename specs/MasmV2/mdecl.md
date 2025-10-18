# Masm Calling Convention
The masm calling convention (formally called mdecl) is very similar to cdecl (the c calling convention) 

## Arguments
Arguments are pushed to the stack from right to left

## Return values
Return values are stored in RAX

## Stack tracing
Right after a function gets called the function should call `ENTER <size>` where size can be used to allocate space for local variables (see [[MASM_ENTER]])

Before a function returns it should call LEAVE. This allows for [[stack tracing]]

## Persistant registers
Only rax and rbx can change. RSP can change if values are being pushed for returing many values.

## Example
If you where to write
```
int add (int a, int b) {
    return a+b
}

void main() {
    add(1,2)
}
```
in masm following mdecl you would get
```
lbl add
ENTER
; get values
mov rax $[rbp-8]
mov rbx $[rbp-12]
add rax rbx
; return value is already in rax
; restore stack frame
LEAVE
ret