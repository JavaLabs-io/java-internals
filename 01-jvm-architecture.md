# 01 — JVM Architecture
> Java is "write once, run anywhere." The JVM is why that's actually true.
---
## What is the JVM?
The Java Virtual Machine is the engine that runs Java code.
`.java` files don't run directly. The JVM runs `.class` files compiled bytecode, not the source code written by the developer.
This is why Java works on any operating system. The code is written once, compiled to bytecode, and the JVM installed on each platform takes care of the rest.
---
## What happens when Java code runs?
1. `.java` source code is written
2. `javac` compiles it into `.class` bytecode not machine code, not something human-readable, something in between
3. The JVM picks up that `.class` file and runs it
Bytecode is platform-neutral. Machine code is not. That's the whole point.
---
## What's inside the JVM?
Three jobs — loading code, managing memory, and running instructions.
### 1 — Loading code (ClassLoader)
Before anything runs, the JVM finds and loads `.class` files. That's the ClassLoader.
Classes aren't all loaded at once. They load on demand only when first needed.
The ClassLoader follows a parent-first order. Core Java classes like `String` or `ArrayList` are always loaded first by the bootstrap loader. Application classes come after. This is why `String` can't be redefined the JVM finds its own version before it ever looks at application code.
### 2 — Memory (where things live at runtime)
The JVM splits memory into areas, each with a specific job:
**Heap** — every object created with `new` lives here. Shared across all threads. Garbage Collection runs here.
**Stack** — every method call gets its own frame. Local variables live inside that frame. When the method finishes, the frame is removed. Each thread gets its own stack.
**Method Area** — class-level data lives here. Static variables, bytecode, class metadata. Loaded once per class.
This is what those common errors actually mean:
- `StackOverflowError` — recursion kept pushing frames onto the stack until there was no space left
- `OutOfMemoryError` — the Heap filled up, objects were being created faster than GC could clean them
### 3 — Running bytecode (Execution Engine)
The execution engine reads bytecode and runs it. Two modes:
**Interpreter** — reads and runs bytecode one line at a time. Simple, but slow for repeated operations.
**JIT Compiler** — watches which parts of the code run often (called hot code), then compiles those parts into native machine code. That version runs directly on hardware with no interpretation overhead.
This is why Java gets faster the longer it runs. The JVM is watching and optimizing while the program is already running.
---
## Errors that trace back to JVM internals
| Error | What's actually happening |
|---|---|
| `OutOfMemoryError` | Heap is full objects are being held in memory longer than needed |
| `StackOverflowError` | Stack is full almost always runaway recursion |
| `ClassNotFoundException` | ClassLoader couldn't find the `.class` file usually a classpath problem |
| Slow startup, fast after | JIT is warming up normal behavior for long-running Java applications |
---
## The one thing worth remembering
Java code never runs directly on hardware. It runs on the JVM, which sits between the code and the operating system. That layer is what makes Java portable and it's also where most of Java's quirks come from.
