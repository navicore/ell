# ELL — The Executable Logic Language

A new experimental logic programming language implemented in Rust and compiled
through LLVM.

ELL is a personal research language designed to explore unification,
backtracking, inference, and declarative logic execution on a modern optimizing
compiler toolchain. It takes inspiration from Prolog, miniKanren, Mercury, and
LLVM-based systems while maintaining a small, simple core.

ELL's goal is to *learn* by *building* — to understand how logic languages can
compile to LLVM IR, how unification can be optimized, how backtracking interacts
with SSA form, and how to design a modern logic VM.

---

## Why ELL?

### 🎯 **1. A clean, modern Prolog-like language** ELL will provide:
- declarative facts and rules  
- unification and backtracking  
- pattern-matching semantics  
- deterministic and nondeterministic execution  
- simple, consistent syntax  

### ⚙️ **2. A Rust implementation** Rust gives ELL:
- memory safety with zero-cost abstractions  
- an expressive type system for representing terms, environments, frames  
- ergonomic tooling (Cargo, crates, tests, docs)  
- the ability to embed ELL in Rust programs later  

### 🧱 **3. An LLVM backend** Compiling logic code to LLVM IR enables:
- native code generation  
- function inlining for predicates  
- register allocation instead of heap-heavy frames  
- potential JIT’ing of queries  
- experimentation with “compiled Prolog” strategies  

### 🧪 **4. A platform for language-design experiments** The goal is to explore:
- optimizing unification  
- representing logical variables in SSA form  
- emitting control flow graphs for nondeterminism  
- mixing logic and functional styles  
- the feasibility of a “compiled miniKanren”  

ELL can evolve freely without legacy constraints.

---

## Example: ELL in 20 seconds

The final syntax isn’t fixed yet, but here is a small draft:

```prolog parent(john, mary). parent(mary, alice).

ancestor(X, Y) :- parent(X, Y).

ancestor(X, Y) :- parent(X, Z), ancestor(Z, Y). ```

Query:

```text ell> ancestor(john, Who)? Who = alice. ```

Or with multiple results:

```text ell> parent(Who, mary)? Who = john. ```

Syntax will remain Prolog-influenced but with clearer rules around whitespace,
modules, and operator declarations.

---

## Project Status

| Component        | Status      | |------------------|-------------| | Parser /
AST     | ⚪ Planned  | | Unifier          | ⚪ Planned  | | WAM-like VM      |
⚪ Planned  | | LLVM codegen     | ⚪ Planned  | | Standard library | ⚪ Planned
| | REPL (`ell`)     | ⚪ Planned  | | Compiler (`ellc`)| ⚪ Planned  |

This repo starts at zero — intentionally — to allow a clean build-out with tight
iteration.

---

## Architecture Overview

ELL will be organized into several layers:

``` source code → parser → AST → IR → LLVM builder → native code ↑ unification
engine + environment model ```

### 🧩 1. **Frontend** Responsible for:
- parsing terms, rules, queries
- maintaining operator precedences
- generating a minimal AST

### 🔗 2. **Core IR** A logic-specific intermediate representation expressing:
- variables and scopes
- unification steps
- choice points
- predicate calls
- backtracking control

This IR will be purpose-built for ELL before lowering to LLVM.

### 🧠 3. **Execution Model** Options being explored:
- a WAM-inspired register model  
- a continuation-passing style (CPS)  
- or a fresh design more compatible with SSA and native code

### 🧱 4. **LLVM Backend** Using either:
- `inkwell` (Rusty and safe), or  
- `llvm-sys` (unsafe but precise)

Goals:
- compile predicates to functions
- emit CFGs matching choice points
- provide JIT for REPL
- optionally build static binaries with `ellc`

---

## Design Principles

1. **Small core, powerful composition**  Rules, facts, and patterns should be
simple and orthogonal.

2. **Explicit semantics, no surprises**  Unlike legacy Prolog operators, ELL
will avoid magic.

3. **Fast execution**  LLVM should reduce the overhead of unification and
backtracking.

4. **Embeddable**  Eventually, Rust programs should be able to call ELL
predicates directly.

5. **Readable**  The surface syntax should stay small and familiar.

---

## CLI Tools

### `ell` — REPL
- loads `.ell` files
- runs queries
- JIT-compiles hot predicates
- supports tracing / debugging modes

### `ellc` — Compiler
- compiles `.ell` sources → native binaries
- or `.ell` → `.ll` (LLVM IR) for inspection
- supports optimizations via LLVM passes

---

## Roadmap

### **Phase 1 — Bootstrap**
- basic lexer + parser  
- minimal AST  
- simple interpreter with backtracking  

### **Phase 2 — Logic Engine**
- unification algorithm  
- frames, environments  
- deterministic + nondeterministic execution  

### **Phase 3 — LLVM Integration**
- IR design  
- function lowering  
- REPL JIT support  
- compiled ELF/Mach-O binaries  

### **Phase 4 — Enrichment**
- modules  
- arithmetic  
- I/O predicates  
- meta-programming  

### **Phase 5 — Experimental Features**
- relational macros  
- hybrid functional/logic features  
- type inference  
- constraints (CLP)  

---

## Contributing

ELL is primarily a learning and research project, but contributions,
experiments, and discussions are welcome.

Issues and design notes will be maintained in this repo as the language evolves.

---

## License

MIT. ELL is free and open for study, modification, remixing, and exploration.

---

## Author

ELL is created by **Ed Sweeney (navicore)** as a continuation of his work
building experimental languages, Rust compilers, and declarative DSLs.
