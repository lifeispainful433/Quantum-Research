## 1. Definition

Port describes the input and output connections of an operation in a DAG.

It specifies how resources enter and leave an operation.


---

# 2. Why Port is needed?


Some quantum operations are not symmetric.

The typical example is CX gate.

CX:

control ──●──

target   ──X──


The two qubits have different roles:

- control qubit
- target qubit


A normal edge only shows connection.

Port preserves the meaning of each connection.


---

# 3. Port in DAG


For a quantum operation:

Input Port

↓

Operation

↓

Output Port


Each input resource has a corresponding output resource.


This allows the compiler to trace a specific qubit through the circuit.


---

# 4. Role in Compilation


Ports allow:

- distinguishing multi-qubit gate inputs
- tracking qubit states
- composing circuits correctly


Related:

- [[DAG Representation]]
- [[Register]]
