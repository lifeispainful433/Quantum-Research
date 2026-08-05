## 0. Metadata

Authors: Gushu Li

Year: 2019-04-04

Venue: ASPLOS '19: Architectural Support for Programming Languages and Operating Systems

Related topics:

- Quantum Compiler
- Qubit Mapping
- Routing
- Optimization



---

# 1. Motivation (Why)

## Background

The NISQ (Noisy Intermediate-Scale Quantum) era is approaching. IBM, Google, and Intel have released devices with 50–100+ qubits. However, the qubit count is still insufficient for Quantum Error Correction (QEC). Consequently, all logical qubits in a quantum circuit are directly mapped to physical qubits without redundancy. This means the compiler must bridge the gap between the ideal circuit model and imperfect hardware.
## Problem

The qubit mapping problem on NISQ devices requires inserting SWAP gates to satisfy 
hardware coupling constraints. The author aims to minimize the overall circuit 
depth while balancing the total number of gates, since these two objectives are 
in conflict: parallel SWAPs reduce depth but increase gate count, while sequential 
SWAPs reduce gates but increase depth. As the problem is NP-Complete, exact 
solutions are infeasible for scalable NISQ devices.
## Why important?

-Qubit Lifetime
-Operation Fidelity
-Qubits Coupling

---

# 2. Core Idea (What)

## Main contribution

>SABRE is a SWAP-based bidirectional heuristic search algorithm. It efficiently solves the qubit mapping problem for NISQ devices by restricting the search space to qubits associated with the Front Layer, leveraging circuit reversibility via reverse traversal to optimize the initial mapping, and introducing the decay effect to balance the total number of gates and circuit depth. 

## key concepts

### SWAP-based Heuristic Search

### Reverse Traversal Technique

### Decay Effect

---

# 3. Method (How)

SWAP-based Heuristic Search
![[Pasted image 20260804115547.png|193]]     

Reverse Traversal Technique

Leverage the intrinsic reversibility of quantum circuits to bootstrap a high-quality initial mapping. Instead of guessing the initial mapping blindly, SABRE first runs forward to see "where things end up," then uses that endpoint as the starting point for a backward run. The endpoint of this backward run becomes a globally informed initial mapping for the original circuit.

Quantum circuits are composed of unitary gates, meaning every gate is reversible:
- CNOT⁻¹ = CNOT
- H⁻¹ = H  
- T⁻¹ = T†

Therefore, reversing the gate sequence produces a valid quantum circuit with the same qubit interaction structure. The "difficulty" of mapping is symmetric between forward and backward directions. By traversing both directions, SABRE propagates global dependency information into the initial mapping decision.

Decay Effect


Without decay, the heuristic search tends to greedily move the same qubit along the shortest path to its target. This creates **serial SWAP chains**, increasing circuit depth. The decay effect penalizes recently moved qubits to encourage **parallel SWAPs** on disjoint qubit pairs.

## Mathematics

$$
H = \max(\text{decay}(q_1), \text{decay}(q_2)) \times \left\{ \frac{1}{|F|}\sum_{g \in F} D[\pi(g.q_1)][\pi(g.q_2)] + W \cdot \frac{1}{|E|}\sum_{g \in E} D[\pi(g.q_1)][\pi(g.q_2)] \right\}
$$


| 符号                  | 含义                                           |
| ------------------- | -------------------------------------------- |
| $q_1, q_2$          | 候选 SWAP 涉及的两个逻辑量子比特                          |
| $\text{decay}(q_i)$ | 量子比特 $q_i$ 的疲劳值，初始为 1，每参与一次 SWAP 增加 $\delta$ |
| $\max(\dots)$       | 取两个量子比特中疲劳值较大的那个作为惩罚系数                       |

| 符号             | 含义                                                                    |
| -------------- | --------------------------------------------------------------------- |
| $F$            | **Front Layer**，当前 DAG 中入度为 0 的所有两量子比特门（软件上可执行，但硬件上可能不相邻）             |
| $\|F\|$        | Front Layer 中门的数量                                                     |
| $g \in F$      | 遍历 Front Layer 中的每一个门                                                 |
| $g.q_1, g.q_2$ | 门 $g$ 涉及的两个逻辑量子比特                                                     |
| $\pi(\cdot)$   | 当前映射函数，把逻辑量子比特映射到物理量子比特编号                                             |
| $D[i][j]$      | 距离矩阵，$D[i][j]$ 表示物理量子比特 $Q_i$ 到 $Q_j$ 的最短路径长度（预处理用 Floyd-Warshall 计算） |

| 符号      | 含义                                                        |
| ------- | --------------------------------------------------------- |
| $E$     | **Extended Set**，Front Layer 之后"即将就绪"的一批门（论文中 \|E\| = 20） |
| $\|E\|$ | Extended Set 的大小                                          |
| $W$     | 权重参数，$0 \leq W < 1$，论文中设为 **0.5**                         |

---

# 4. Experiment


## Dataset / Benchmark

26 quantum circuits from 4 benchmark suites, executed on the IBM Q20 Tokyo coupling graph:

| Category           | Examples               | Qubits | Characteristics                                 |
| ------------------ | ---------------------- | ------ | ----------------------------------------------- |
| Small arithmetic   | 4mod5, decod24, alu    | 4–5    | Few gates; often admit perfect initial mappings |
| Quantum simulation | ising\_model\_10/13/16 | 10–16  | Nearest-neighbor structure; scalable patterns   |
| QFT                | qft\_10/13/16/20       | 10–20  | Highly entangled; long-range interactions       |
| Large arithmetic   | rd84, sym9, co14       | 10–15  | Thousands of gates; complex dependency graphs   |

## Baseline

Zulehner et al. 2018 (BKA) — Best Known Algorithm prior to SABRE.

- C++ implementation, compiled with GCC -O3.
    
- Open-source code was downloaded and modified to target the same IBM 20-qubit symmetric coupling model for fair comparison.
    
- Uses A* search with exhaustive concurrent-SWAP exploration.
## Evaluation metrics

- **Additional Gate Count (д_add / д_op):** Number of SWAP gates inserted to make the circuit hardware-compliant. Serves as a proxy for **fidelity** (fewer gates → less accumulated error).
    
- **Circuit Depth (d):** Longest path of sequential operations. Serves as a proxy for **coherence-time compliance** (shallower circuits → less decoherence).
    
- **Runtime:** Compilation time in seconds. Tests **scalability**.
    
- **Memory Usage:** Peak RAM consumption. Tests **scalability limits**.

Note: Fidelity is not directly measured in the experiment; gate count and depth are used as its primary proxies, since each SWAP introduces 3 imperfect CNOTs and deeper circuits exceed qubit coherence time.



## Results

| Comparison         | Small / Ising                                                                          | Large / QFT                                                                                      |
| ------------------ | -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| **Gate reduction** | Up to **91%** fewer additional gates; often **zero overhead** after reverse traversal. | ~**10%** average reduction vs. BKA.                                                              |
| **Speedup**        | Exponential; BKA finishes in <1s, SABRE in <0.01s.                                     | BKA **out of memory** (>378GB) on qft\_20 and ising\_16; SABRE solves both in ~0.1s with ~300MB. |
| **Depth control**  | —                                                                                      | Decay tuning enables ~**8% depth variation** at the cost of increased gate count (Figure 8).     |

1. **SWAP-based search collapses complexity:** By restricting candidate SWAPs to those adjacent to the Front Layer, the per-step search space drops from O(exp(N)) to O(N). This makes SABRE scalable to larger devices where BKA exhausts memory.
    
2. **Reverse traversal bootstraps a better starting point:** The forward-backward-forward procedure propagates global circuit information into the initial mapping. On small benchmarks, this frequently finds a perfect subgraph match between logical interactions and physical coupling, eliminating the need for SWAPs entirely.
    
3. **Decay effect enables platform-specific tuning:** The multiplicative decay penalty forces the heuristic to distribute SWAPs across disjoint qubit pairs rather than serially moving the same qubit. This directly controls the depth-gate trade-off, allowing the compiler to favor shallower circuits on short-coherence devices or fewer gates on high-fidelity devices.



---

# 5. Relationship with My Knowledge


## Quantum Compiler

Logical Circuit 
↓ 
Qubit Mapping 
↓ 
Routing

## Related papers
[Noise-Adaptive Compiler Mappings for Noisy Intermediate-Scale Quantum Computers]



---

## 6. Limitation

[Pasted image 20260804130828.png]
