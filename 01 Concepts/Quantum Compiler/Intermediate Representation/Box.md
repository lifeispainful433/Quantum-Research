## 1. Definition

A Box is an abstraction mechanism in t|ket> IR.

It allows a group of operations to be treated as a single operation.


---

# 2. Example

Instead of representing:

H

↓

CX

↓

Rz

↓

X

as individual nodes,


a compiler can represent it as:
+-----------+  
|     Box        |  
|     H           |  
|     CX         |  
|     Rz          |  
|     X            |  
+-----------+



---

# 3. Why use Box?


Boxes provide:

## Abstraction

Hide internal circuit details.


## Modularity

A complex subcircuit can be reused.


## Optimization

Compiler can optimize at different levels of abstraction.


---

# 4. Relationship with DAG


A Box can be considered a special node in the DAG.

Instead of:

Node  
Node  
Node  
Node


we can have:

Box Node


representing the whole structure.


---

# 5. Example Applications


Boxes can represent:

- subcircuits
- repeated structures
- higher-level operations


Related:

- [[DAG Representation]]
