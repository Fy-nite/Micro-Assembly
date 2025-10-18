# Micro Virtual Computer (MVC) Languages

The **Micro Virtual Computer (MVC)** family is a set of programming languages and specifications designed to run on a common virtual architecture. These languages are intended to provide a range of abstraction levels, from low-level assembly-like programming to higher-level, C-like syntax, all targeting the same virtual machine model.

## What is MVC?

MVC stands for **Micro Virtual Computer**. It is a conceptual architecture and language ecosystem that includes:

- **Micro-Assembly (MicroASM, MASM):**
  - A low-level, assembly-inspired language designed for portability, clarity, and direct control over the virtual machine's state and memory.
  - Features a rich instruction set, state variables, macros, and a Micro Native Interface (MNI) for native code integration.

- **Wake:**
  - A higher-level, C-like language that compiles directly to Micro-Assembly.
  - Provides improved readability and familiar syntax while retaining the low-level power of MicroASM.

- **Other Languages (Planned/Future):**
  - The MVC ecosystem is designed to support additional languages that target the same virtual machine, enabling a variety of programming styles and use cases.

## Goals of MVC

- **Portability:** Code written in any MVC language can run on any compliant virtual machine implementation, regardless of host platform.
- **Transparency:** Minimal hidden features; the programmer has explicit control over program behavior.
- **Extensibility:** The Micro Native Interface (MNI) allows integration with native code and system services.
- **Educational Value:** MVC languages are designed to be approachable for learning about low-level programming, virtual machines, and language design.

## Relationship Between Languages

- **Micro-Assembly** is the core language and virtual machine specification. All other MVC languages ultimately compile down to MicroASM.
- **Wake** and other higher-level languages provide more ergonomic syntax and features, but their semantics map directly to MicroASM instructions.

## Documentation

- See the [Micro-Assembly documentation](specs/MasmV2/) for the full instruction set, memory model, and runtime details.
- See the [Wake language reference](specs/WakeV1/Wakev1.md) for details on the C-like syntax and its mapping to MicroASM.

## Example Workflow

1. Write code in Wake (or another MVC language).
2. Compile to Micro-Assembly.
3. Run the resulting MicroASM code on any MVC-compliant virtual machine or interpreter.

---

The MVC family is under active development. Contributions and feedback are welcome!