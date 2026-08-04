## 1. Definition 
Pauli operators are fundamental single-qubit operators in quantum computing. The three Pauli matrices: $$ X = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix},\quad Y = \begin{bmatrix} 0 & i \\ -i & 0 \end{bmatrix},\quad Z = \begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix} $$ They describe fundamental state transformations of qubits, and form the foundation of nearly all quantum operations. 
## 2. Pauli String 
For multi-qubit systems, Pauli operators are combined via tensor product. General form of a Pauli string: $$ P = P_1 \otimes P_2 \otimes \dots \otimes P_n $$ where each $P_i \in \{I,X,Y,Z\}$.

### Example
$$P = X \otimes Y \otimes Z$$
A Pauli string acts on multiple qubits simultaneously. It offers a compact algebraic notation for multi-qubit transformations.
## 3. Relation with Quantum Gates 
Single-qubit rotation gates are defined as exponential functions of Pauli operators: $$ \begin{aligned} R_x(\theta) &= e^{-i\theta X/2} \\ R_y(\theta) &= e^{-i\theta Y/2} \\ R_z(\theta) &= e^{-i\theta Z/2} \end{aligned} $$ Pauli operators provide a unified algebraic language to express all single-qubit rotations. 
## 4. Pauli Gadget in t|ket>
A Pauli gadget is a high-level IR object representing the unitary generated from a Pauli string: $$ U = e^{i\alpha P} $$ - $P$: Pauli string - $\alpha$: rotation angle 
### Example 
Take $P = X\otimes Y\otimes Z$: 
Instead of expanding into a long primitive gate sequence like 
`CX → Rz → CX → CX → Rz`
t|ket⟩ stores the whole block as one high-level structure: 

t|ket⟩ stores the whole block as one unified high-level Pauli Gadget object representing $e^{i\alpha XYZ}$.

Algebraic simplification can be performed before decomposing into hardware gates.

## 5. Why Pauli Gadgets are useful

Pauli gadgets act as an elevated intermediate representation.

### Advantages

1. **Algebraic manipulation**
    
    The compiler can directly compute and simplify mathematical expressions without expanding to basic gates.
2. **Circuit simplification**
    
    Identical or inverse Pauli gadgets can be merged or eliminated entirely.
3. **Reduce CX count**
    
    CX gates introduce heavy noise and overhead on NISQ hardware; high-level simplification drastically cuts total CX usage.
4. **Enable macroscopic optimisation**
    
    Optimisation passes process large subcircuit blocks as single objects, rather than scanning individual small gates.

## 6. Relation with DAG Representation

Workflow in t|ket⟩ IR:

Quantum Circuit

↓

DAG Representation

↓

DAG Node Types: Gate / Box / Pauli Gadget

↓

Optimisation Pass

↓

Hardware-compliant circuit

A Pauli gadget is treated as a single high-level node inside the DAG.

Instead of storing many separate CX/Rz nodes, the whole exponential operator \(e^{i\alpha P}\) becomes one unified entry for global optimisation before hardware decomposition.

## 7. Relation to t|ket> Optimisation Pipeline

Full optimisation flow based on Pauli gadgets:

1. Input primitive gate circuit
2. Detect continuous subcircuits matching Pauli gadget structure
3. Rewrite subcircuits into compact Pauli gadget IR nodes
4. Simplify via Pauli algebra identities
5. Decompose simplified gadgets back to native hardware gate set
6. Output circuit with reduced CX count and shorter depth

## Core Summary

- Pauli operators: base single-qubit matrix building blocks
- Pauli strings: tensor-product combinations for multi-qubit systems
- Pauli gadgets: high-level exponential IR objects used in t|ket⟩ for macroscopic circuit optimisation

## Related Links

[[Macroscopic Optimization]]

[[Peephole Optimization]]

[[Box]]

[[DAG Representation]]

[[Intermediate Representation]]