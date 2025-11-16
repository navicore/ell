# ELL — The Executable Logic Language

A new experimental logic programming language implemented in Rust and compiled
through LLVM.

ELL is a personal research language designed to explore unification,
backtracking, inference, and declarative logic execution on a modern optimizing
compiler toolchain. It takes inspiration from Prolog, miniKanren, Mercury, and
LLVM-based systems while maintaining a small, simple core.

**ELL is not intended to replace Prolog**.  Its goal is to *learn* by *building*
— to understand how logic languages can compile to LLVM IR, how unification can
be optimized, how backtracking interacts with SSA form, and how to design a
modern logic VM.

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

ancestor(X, Y) :- parent(X, Z), ancestor(Z, Y).

