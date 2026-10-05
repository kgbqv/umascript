# UmaScript v0.1 Specification

UmaScript is a small domain-specific language for describing racers, their
statistics, and simple race strategies.

The implementation pipeline is:

```text
.umx source → compiler → .uma bytecode → Uma VM → race simulator
```

This document defines the deliberately small first version. Features outside
this document belong in `FUTURE.md` until the core pipeline is working.

## Source files

UmaScript source files use the `.umx` extension. A source file contains zero or
more statements separated by newlines. Whitespace is insignificant except as a
separator between tokens.

Comments begin with `#` and continue to the end of the line.

## Values

Version 0.1 supports two value types:

- integers
- booleans: `true` and `false`

There are no strings, lists, user-defined functions, or arbitrary objects in
the first version.

## Variables and arithmetic

Variables are assigned with `=`:

```umx
speed = 10
bonus = 5
total = speed + bonus * 2
```

Supported arithmetic operators are:

| Operator | Meaning |
|---|---|
| `+` | addition |
| `-` | subtraction |
| `*` | multiplication |
| `/` | integer division |
| `-` | numeric negation |

Multiplication and division bind more tightly than addition and subtraction.
Parentheses may be used to override precedence:

```umx
total = (speed + bonus) * 2
```

Reading a variable before assigning it is an error.

## Comparisons

Comparisons produce boolean values:

```umx
is_fast = speed > 50
has_energy = stamina >= 10
```

Supported comparison operators are `>`, `>=`, `<`, `<=`, `==`, and `!=`.

## Conditional execution

The `when` statement executes a block when its condition is true:

```umx
when stamina > 30 {
    stamina = stamina - 5
}
```

The condition must evaluate to a boolean. An `else` branch is not part of
version 0.1.

## Racers

A racer is declared with `uma` followed by a name and a block of assignments:

```umx
uma Teio {
    speed = 80
    stamina = 60
}
```

The first version recognizes `speed` and `stamina` as racer statistics. Both
must be integer values. A racer must define each statistic exactly once.

## Strategies

A racer may contain one `strategy` block. The block contains conditional logic
and race actions:

```umx
uma Teio {
    speed = 80
    stamina = 60

    strategy {
        when stamina > 30 {
            accelerate 5
        }
    }
}
```

The initial action set is:

- `accelerate amount` — increase the racer's current movement for the tick
- `conserve` — skip acceleration for the tick

The `amount` in `accelerate` must be an integer expression. Race actions are
valid only inside a strategy block.

## Execution model

The compiler converts `.umx` source into readable `.uma` instructions. The
initial instruction set is expected to include:

```text
PUSH, LOAD, STORE
ADD, SUB, MUL, DIV
GT, GTE, LT, LTE, EQ, NEQ
JUMP, JUMP_IF_FALSE
CREATE_RACER, SET_STAT
ACCELERATE, CONSERVE
HALT
```

The Uma VM executes instructions using a stack, variable storage, and a
program counter. The `.uma` format is an implementation detail and may change
while the VM is being developed; it does not need to be binary in v0.1.

## Errors

The compiler should report source errors with at least:

- the input filename
- the line number
- a short description

Examples include invalid characters, missing braces, undefined variables,
duplicate racer statistics, invalid value types, and division by zero.

## Explicitly out of scope for v0.1

- strings and collections
- functions and imports
- inheritance
- skills and complex race rules
- concurrency
- binary bytecode
- optimization or JIT compilation
- visual editing tools
