## 1. Overview

DAG (Directed Acyclic Graph, 有向无环图) is a common representation method for quantum circuit intermediate representation (IR).

In quantum compilers such as t|ket> and Qiskit, circuits are converted into DAG-based intermediate representations, which allow compiler passes to analyze and transform circuits.

The DAG is not only a mathematical graph. It contains additional structures designed for quantum computation.

---

## 2. Position in Quantum Compiler


Quantum Program

↓

Circuit Representation

↓

Intermediate Representation (IR)

↓

DAG-based Representation

↓

Compiler Passes

↓

Hardware Circuit


In t|ket>:

DAG is the core structure of the IR.


---

## 3. Basic Structure


A DAG contains:

- Nodes
- Directed edges


### Nodes

Nodes represent operations.

Examples:

- H gate
- CX gate
- Rz gate


Example:

H

↓

CX

↓

Rz


Each node stores information about the quantum operation.


---

### Edges

Edges represent the flow of quantum resources.

Usually the resource is:

- qubit
- classical bit


An edge describes the dependency between operations.


Example:

q0:

H

↓

CX

↓

Measure


The compiler knows that CX cannot be executed before H.


---

# 4. Why DAG?


A quantum circuit diagram is intuitive for humans:

q0 ─H──CX──Rz

q1 ────X────


However, compiler optimization requires:

- operation dependency
- parallel execution information
- resource usage
- transformation ability


DAG provides this information.


---

# 5. Additional Structures


A normal DAG is not enough for quantum circuits.

t|ket> adds:

- Register
- Port
- Box


to represent quantum information more completely.


---

# Related Concepts

- [[Register]]
- [[Port]]
- [[Box]]
- [[Compiler Pass]]