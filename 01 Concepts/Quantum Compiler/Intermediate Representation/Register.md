## 1. Definition

A register is a structure used to organize and identify quantum or classical resources in a circuit.

In quantum circuits, registers usually contain qubits.

Example:

Quantum Register q:

q[0]  
q[1]  
q[2]

Each qubit has a unique identifier inside a register.


---

# 2. Why Register is needed?


A quantum circuit may contain many qubits.

The compiler needs to distinguish:

CX(q[0], q[1])

from:

CX(q[2], q[3])


Registers provide the identity and ordering of resource units.


---

# 3. Relationship with DAG


In DAG representation:

- Nodes represent operations
- Edges represent resource flow
- Registers identify the resources being transferred


Example:

q0:

H

↓

CX

↓

Measure


The edge corresponds to the same qubit resource.


---

# 4. Role in Compilation


Registers help compiler:

- track qubit usage
- perform mapping
- maintain input/output interface


Related:

- [[DAG Representation]]
- [[Qubit Mapping]]