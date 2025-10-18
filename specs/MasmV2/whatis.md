# Micro-Assembly

`Micro-Assembly is NOT Assembly`

## What is Micro-Assembly?
Micro-Assembly (often abbreviated as **masm**) is a high-level interpreted programming language that draws inspiration from traditional assembly languages. It is designed to be easy to read and write, while still providing low-level control over system resources. Unlike assembly languages that are closely tied to specific hardware architectures, Micro-Assembly is intended to be portable and versatile, making it suitable for a wide range of applications.

Micro-Assembly is not a direct replacement for assembly languages like x86 or ARM assembly, but rather a higher-level language that abstracts away many of the complexities associated with low-level programming. It is designed to be interpreted by a runtime environment, allowing developers to write code that can run on different platforms without modification.

## Core philosophy
Micro-Assembly was created from the need for a text based programming language that gave users control over what happend in their codebase without explicit "hidden features".

*somewhat* powered by side-effects, Micro-Assembly is great for general tasks.

## Key Features

*   **Named State Variables:** Declare typed variables using `STATE <name> <Type>`.
*   **Floating-Point Support:** Dedicated `FPR` registers and instructions (`FADD`, `FCMP`, etc.).
*   **Assembler Macros:** Define reusable code blocks with `MACRO`/`ENDMACRO`.
*   **Label Scoping:** Create local labels within `SCOPE`/`ENDSCOPE` blocks.
*   **Micro Native Interface (MNI):** Extend functionality by calling native code modules.
*   **Standardized System Calls:** Access runtime/OS services via `SYSCALL`.
*   **Static Data Definition:** Define data using `DB`, `DQ`, `RESB`, etc.
*   **Stack Frame Management:** Simplified setup/teardown with `ENTER`/`LEAVE`.

## Target Audience

Micro-Assembly is targeted to General programmers and scripters for simple and complex tasks like small file movement scripts or complex codebases that run ATM machines


## Elaborate on "Not Assembly"

While Micro-Assembly borrows syntax and concepts from traditional assembly languages, it differs in several key ways:

*   **Interpretation:** Primarily designed to be interpreted by a runtime, not directly compiled to machine code for specific hardware.
*   **Higher-Level Abstractions:** Includes features like `STATE` variables, MNI, and standardized `SYSCALL`s that abstract away hardware details.
*   **Portability:** Aims for portability across different systems via the runtime environment, unlike hardware-specific assembly.


## Role of the Runtime Environment

Micro-Assembly code executes within a runtime environment (interpreter/VM). This runtime is responsible for:

*   Parsing and interpreting the Micro-Assembly instructions.
*   Managing memory, including handling `STATE` variables and dynamic allocation via MNI (`Memory.allocate`/`free`).
*   Implementing the Micro Native Interface (MNI) to bridge to native code.
*   Virtualizing system calls made via the `SYSCALL` instruction, translating them into safe operations on the host system.
*   Managing registers, flags, and the execution stack.

## Simple Code Example

```microassembly
; Example: Print "Hello, World!" using the flavorfull code.
#include "stdio.print"
lbl main
    mov RAX 1
    mov RBX 100
    db $100 "hello world!\n"
    call #print
hlt
```

## Bytecode

Micro-Assembly (or **masm**) is designed primarily as an interpreted language. This means the focus is not on direct compilation to specific targets like x86, leaving that aspect to the community. Direct compilation to efficient, standalone machine code presents significant challenges due to several core design aspects:

*   **Runtime Dependency:** Features like `SYSCALL` virtualization and the Micro Native Interface (`MNI`) inherently rely on a runtime environment to function. Compiling these requires either embedding a substantial portion of the runtime or complex preprocessing to map them to host OS equivalents, losing portability.
*   **Managed State:** Instructions like `STATE` imply managed memory or specific runtime handling for variable storage and typing, which doesn't map cleanly to raw machine memory models without runtime support.
*   **Higher-Level Abstractions:** Local labels (`SCOPE`/`ENDSCOPE`) and advanced memory instructions are designed for the interpreter/assembler level and may not have direct, efficient machine code equivalents across different architectures.

Instructions like `STATE` (defined in the v2 instruction set, might not be available everywhere), local labels, and new memory instructions are not intended to be fully compiled down into platform-specific binary or assembly code without significant effort.

To handle these instructions during compilation, developers often utilize preprocessors or sophisticated toolchains that effectively bundle parts of a runtime or translate Micro-Assembly concepts into platform-specific code, potentially sacrificing some of the language's intended simplicity or portability.

Micro-Assembly's instruction methods will not be changed to specifically support native compilation to machine code, either before, during, or after the language specification is finalized.

