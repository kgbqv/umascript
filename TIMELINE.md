# UmaScript Development Timeline

## Project goal

Build a small domain-specific language for describing racers and race strategies:

```text
.umx source
    ↓
compiler
    ↓
.uma bytecode
    ↓
Uma virtual machine
    ↓
race simulator
```

The first version should prioritize learning compiler and VM fundamentals over feature count.

## Six-week v0.1 plan

Assumption: approximately four focused hours per week.

### Week 1 — Design the language

Decide and document the smallest useful language:

- numbers
- variables
- arithmetic
- comparisons
- `when` conditions
- racer declarations
- strategies
- actions such as `accelerate`

Deliverable: a first `SPEC.md` and several example `.umx` programs.

### Week 2 — Build the lexer and parser

Implement the source-to-AST pipeline for arithmetic and assignments.

Start with:

```text
speed = 10 + 5 * 2
```

Focus on tokenization, recursive-descent parsing, operator precedence, parentheses, and source errors.

Deliverable: valid source produces a readable abstract syntax tree.

### Week 3 — Compile to `.uma`

Define a small, readable instruction set and compile the AST into it.

Example:

```text
PUSH 10
PUSH 5
PUSH 2
MUL
ADD
STORE speed
HALT
```

Keep `.uma` as text initially. Do not optimize or design a binary format yet.

Deliverable: an arithmetic `.umx` file compiles into a readable `.uma` file.

### Week 4 — Build the virtual machine

Implement a simple stack-based VM with:

- program counter
- operand stack
- variable storage
- instruction dispatch
- arithmetic operations
- `LOAD`, `STORE`, and `HALT`

Deliverable: the VM executes the compiled arithmetic program and produces the expected result.

### Week 5 — Add conditions and racers

Add comparisons and control flow:

- `GT`, `LT`, and `EQ`
- `JUMP`
- `JUMP_IF_FALSE`
- `when` blocks

Then introduce the smallest racer model with a few stats such as speed and stamina.

Deliverable: a program can define a racer and update a stat conditionally.

### Week 6 — Add a minimal race simulation

Connect the VM to a deliberately small simulator:

- two or more racers
- simulation ticks
- stamina changes
- acceleration
- distance or position
- a winner condition

Deliverable: a complete example can be compiled and run from `.umx` through the VM to a basic race result.

## After v0.1

Only after the compiler, VM, and basic simulator are stable:

1. Improve diagnostics and test coverage.
2. Add strategies and reusable actions.
3. Consider a structured or binary `.uma` format.
4. Add skills, inheritance, and richer race rules.
5. Build a visual editor that generates `.umx`.

The visual editor is a second phase, not a prerequisite for the language runtime.

## Scope boundaries

Defer these until the core pipeline works:

- visual node graphs
- binary bytecode
- optimization or JIT compilation
- networking
- arbitrary object systems
- garbage collection
- a full game engine

## Definition of success

Version 0.1 is successful when this workflow works:

```text
umascript run examples/first_race.umx
```

and the command lexes, parses, compiles, executes, and reports the result of a small race.

