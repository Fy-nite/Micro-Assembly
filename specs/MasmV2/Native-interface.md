# Module Native Interface (MNI) – Implementation Details

## Overview

The Module Native Interface (MNI) enables Micro-Assembly to call native code in host languages (such as Java, C#, C, C++, Rust, Go, etc.) via a unified module/function registry. The implementation approach differs depending on whether the host language supports annotations/attributes (Java, C#, Kotlin, etc.) or not (C, C++, Rust, Go, etc.).

---

## Implementation in Languages with Annotation/Attribute Support

### Supported Languages

- Java (annotations)
- C# (attributes)
- Kotlin (annotations)
- Others with similar reflection/metadata systems

### Module and Function Registration

- **Modules** are classes marked with a special annotation/attribute (e.g., `@MNIClass("name")`).
- **Functions** are static methods marked with an annotation/attribute (e.g., `@MNIFunction(module, name)`).
- At runtime, the MNI system uses reflection to scan loaded classes for these annotations/attributes and registers them automatically.

#### Example (Java)

```java
@MNIClass("math")
public class MathModule {
    @MNIFunction(module = "math", name = "add")
    public static void add(MNIMethodObject obj) {
        // ...implementation...
    }
}
```

#### Registration Flow

1. On startup, the MNI loader scans the classpath or module JARs for classes with `@MNIClass`.
2. For each such class, it scans for static methods with `@MNIFunction`.
3. Each discovered function is registered in the `ModuleRegistry` under its module and function name.
4. When Micro-Assembly code calls `MNI math.add ...`, the registry locates and invokes the corresponding method.

#### Advantages

- **Automatic discovery**: No manual registration needed.
- **Extensible**: New modules/functions are picked up automatically.

---

## Implementation in Languages Without Annotation/Attribute Support

### Supported Languages

- C, C++
- Rust
- Go
- Others without runtime reflection/metadata

### Module and Function Registration

- **Modules** are registered via explicit function calls at startup.
- **Functions** are registered by passing function pointers or descriptors to the registry.
- No automatic discovery; all registration is manual.

#### Example (C)

```c
// Define the function signature expected by MNI
void math_add(MNIMethodObject* obj) {
    // ...implementation...
}

// Register module and function at startup
void register_math_module() {
    mni_register_module("math");
    mni_register_function("math", "add", &math_add);
}
```

#### Registration Flow

1. At program initialization, call registration functions for each module and function.
2. The MNI system stores function pointers in the `ModuleRegistry`.
3. When Micro-Assembly code calls `MNI math.add ...`, the registry invokes the registered function pointer.

#### Advantages

- **Portable**: Works in any language, even without reflection.
- **Explicit**: Registration is clear and controlled.

---

## Core Concepts (Common to All Languages)

- **ModuleRegistry**: Central registry mapping module/function names to native implementations.
- **MNIMethodObject**: Encapsulates execution context, memory/register access, stack, and state variables.
- **Memory/Register Access**: Provided via methods or function arguments, depending on language.
- **Stack Operations**: Push/pop available via API.
- **State Variables**: Accessed via API or struct fields.

---

## Directory Structure and Configuration

```
MASM_ROOT/
├── logs/         # Log files and execution traces
├── modules/      # Native modules (JARs, DLLs, SOs, etc.)
├── config/       # Configuration files
└── temp/         # Temporary files
```

- Directories are created on startup if missing.
- Paths are configurable via system properties or environment variables.
- Defaults to `.masm/` in the user's home directory if unspecified.

---

## Extending MNI

- **With annotations/attributes**: Add new classes/methods with the appropriate metadata.
- **Without annotations/attributes**: Add new registration calls for each module/function.
- Advanced features (custom include/macro providers, argument types, etc.) can be implemented by extending the registration and invocation APIs.

---

## Safety and Best Practices

- Always validate memory/register access.
- Free any allocated memory as required by the host language.
- Ensure thread safety if modules are used concurrently.

---

## Further Reference

- For a list of available modules/functions, see [mni-instructions.md](../mni-instructions.md).
- Consult the runtime documentation for language-specific integration details.
