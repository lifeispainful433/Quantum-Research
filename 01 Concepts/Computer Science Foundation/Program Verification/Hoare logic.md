# Hoare Triple（霍尔三元组）

## 1. Definition

Hoare triple is the central concept of Hoare Logic.

It describes how executing a program changes the state of computation.

The general form is:

$$
\{P\}\ C\ \{Q\}
$$


where:

- $P$ is the **precondition** (前条件)
- $C$ is the **command** (程序命令)
- $Q$ is the **postcondition** (后条件)


---

## 2. Intuitive Meaning

The Hoare triple:

$$
\{P\}\ C\ \{Q\}
$$

can be read as:

> If condition $P$ is true before executing command $C$, then condition $Q$ will be true after executing $C$.


In simple words:

Before execution:  
P must be satisfied
      ↓
Execute program C
      ↓
After execution:  
Q is guaranteed


---

## 3. Components


### Precondition P

The condition that must be satisfied before executing the program.

Example:

\[x>0]


means:

Before execution, x must be greater than 0.


---

### Command C

The operation or program being executed.

Example:

python
x = x + 1

---
### Post condition Q

The property that should be satisfied after execution.

Example:

x>1

means:
After execution, x should be greater than 1.

---
## 4.Example

Consider the program:

```
x = x + 1
```

A Hoare triple:

{x>0} x:=x+1 {x>1}\{x>0\}\ x:=x+1\ \{x>1\}{x>0} x:=x+1 {x>1}

Meaning:

If x is greater than 0 before execution,

after increasing x by 1,

x will be greater than 1.

---

## 5. Assertions

In Hoare Logic:

- P and Q are called **assertions**.
- Assertions are formulas expressed using predicate logic.

They describe properties of program states.

Example:

x>0

is an assertion because it describes a property of the current state.

---

## 6. Relation to Compiler

Compiler transformations should preserve program meaning.

A compiler pass can be described as:

Original program

↓

Compiler Pass

↓

Optimized program

The transformation should guarantee that the program behavior remains correct.

Related concepts:

- [[Compiler Correctness]]
- [[Semantic Preservation]]
- [[Compiler Pass]]

---

## 7. Relation to Quantum Compiler

Quantum compiler also performs transformations:

Logical circuit

↓

Compiler optimization pass

↓

Hardware executable circuit

The transformation should preserve the meaning of the quantum computation.

Examples:

- Circuit optimization
- Gate cancellation
- Gate rewriting

Related concepts:

- [[Quantum Compiler]]
- [[IR]]
- [[DAG]]
- [[Compiler Pass]]

---

## 8. Key Idea

Hoare triple provides a formal way to answer:

> "Does this program transformation keep the computation correct?"

It is an important tool in:

- Program Verification
- Formal Methods
- Compiler Theory
- Quantum Compiler Correctness