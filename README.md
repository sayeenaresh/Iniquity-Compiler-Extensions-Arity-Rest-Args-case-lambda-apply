# Iniquity+ Compiler Extensions: Arity, Rest Arguments, case-lambda, and apply

**Course:** CMSC 430 – Compilers  
**Language:** Racket  
**Project Type:** Individual compiler implementation

## Overview
This project extends a small functional language compiler (Iniquity+) with support for **runtime arity checking**, **variable-arity functions**, **multi-arity dispatch**, and the **apply** form. The goal was to design and implement correct runtime behavior for function calls in a language expressive enough that arity cannot be fully checked statically.

All features were implemented at the compiler level by modifying code generation logic, with correctness validated against a provided interpreter.

---

## Features Implemented

### 1. Runtime Arity Checking
- Implemented runtime checks to ensure functions are called with a valid number of arguments.
- The caller communicates the number of supplied arguments via a designated register.
- Each function definition verifies the received arity against its expected constraints and signals an error on mismatch.

### 2. Rest Arguments (Variable-Arity Functions)
- Added support for functions with rest parameters (e.g. `(define (f x . xs) ...)`).
- Generated code that:
  - Checks that at least the minimum required arguments are provided.
  - Pops extra arguments from the stack.
  - Constructs a proper list of remaining arguments in the correct order.
  - Binds this list to the rest parameter.

### 3. case-lambda (Multi-Arity Functions)
- Implemented case-lambda–style functions that dispatch between multiple clauses based on the number of arguments supplied.
- Each clause may be a fixed-arity or rest-arity function.
- At runtime, the compiler-generated code selects the first clause whose arity constraints are satisfied; otherwise, it signals an arity error.

### 4. apply
- Implemented the `apply` form, allowing a function to be invoked using a list of arguments.
- The compiler:
  - Evaluates explicit arguments and pushes them onto the stack.
  - Evaluates the final list argument.
  - Traverses the list at runtime, pushing each element onto the stack.
  - Invokes the target function with the combined arguments.
- Includes error handling when the final argument is not a proper list or when arity constraints are violated.

---

## Design Highlights
- Careful management of **stack discipline** to preserve argument order.
- Separation of concerns between:
  - argument counting (caller),
  - arity validation (callee),
  - argument reconstruction (rest arguments and apply).
- Interpreter-guided testing used to confirm semantic equivalence between interpreted and compiled execution.

---

## Testing
Correctness was validated using:
- Provided test cases
- Custom edge-case programs covering:
  - arity mismatch errors
  - rest argument binding
  - case-lambda dispatch behavior
  - apply with varying argument structures

---

## Repository Notes
This repository contains **only original implementation code** written for this project. Starter files and provided interpreter code are excluded or unmodified, in accordance with course academic integrity policies.

---

## Acknowledgments
Starter language structure and interpreter provided as part of CMSC 430 coursework.
