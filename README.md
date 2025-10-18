# Micro-Assembly

This repository serves as the central hub for the specification, documentation, and examples related to the Micro-Assembly language and the Micro Virtual Computer (MVC) family of languages.


## Overview

Here you can find:

* Documentation
* Language specifications for Micro-Assembly and other MVC languages (including different versions)
* Examples and usage guides (potentially)

## What is the Micro Virtual Computer (MVC)?

The **MVC** is a virtual architecture and language ecosystem that includes:
- **Micro-Assembly (MicroASM/MASM):** The core low-level language and VM specification.
- **Wake:** A higher-level, C-like language that compiles to Micro-Assembly.
- **Other planned languages:** All targeting the same virtual machine model.

See [MVC.md](MVC.md) for an introduction to the MVC family and its goals.


## Runtimes

Micro-Assembly has a bunch of Offical runtimes, including:
- **JMASM:** A Java-based runtime for Micro-Assembly.


## Specification

The current development focuses on the **v2 Specification** for Micro-Assembly. Please note that this specification is currently a **work in progress**, and details may change.

* **Instruction Set:** Find the detailed list of instructions, registers, addressing modes, and Micro Native Interface (MNI) functions in the [v2 Instruction Set Documentation](specs/MasmV2/MicroV2.md).
* **General Notes:** Read about syntax conventions and development philosophy in the [v2 Spec Notes](specs/MasmV2/notes.md).
* **Wake Language:** See the [Wake language reference](specs/WakeV1/Wakev1.md) for the higher-level syntax and its mapping to MicroASM.

## Key Features (v2 Spec)

* Standard arithmetic, logic, and flow control instructions.
* Stack operations (PUSH, POP, ENTER, LEAVE).
* I/O operations (IN, OUT, COUT).
* Memory manipulation (DB, COPY, FILL).
* Bitwise operations (AND, OR, XOR, NOT, SHL, SHR).
* Extended memory addressing (MOVADDR, MOVTO).
* **Micro Native Interface (MNI):** Allows interaction with native code (e.g., Java via JMASM, Python via PyMasm) for extended functionality like:
  * Math operations
  * Memory management
  * Advanced string operations
  * Data structures (Lists, Maps)
  * Network operations
  * File system access
  * System calls
  * Debugging tools

## License

This project is licensed under the GNU Affero General Public License v3.0. See the [LICENSE](LICENSE) file for full details.

Please refer to the specific directories within this repository, such as [`docs/`](docs/) and [`specs/`](specs/), for detailed information. The content and structure may evolve over time.
