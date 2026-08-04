## 1. Definition

Macroscopic Optimization（宏观优化）是一种针对大型电路结构的优化方法。


与 Peephole Optimization 不同：

Peephole 关注局部 gate pattern。

Macroscopic Optimization 关注整个 subcircuit 的高级结构。


---

## 2. Basic Principle

一个复杂的量子线路可能隐藏着更高级的数学结构。


例如：

一个由多个 gate 组成的线路：
CX  
Rz  
CX  
Rz  
CX

可能整体表示：

$$
e^{i\alpha P}
$$


其中：

P 是 Pauli operator。


因此可以：

复杂 gate sequence

↓

高级数学表示

↓

优化

↓

重新展开为基础 gate


---

## 3. Example: Pauli Gadget


Pauli gadget 是 t|ket> 中用于表示多量子比特操作的一种高级结构。


数学形式：

$$
e^{i\alpha P}
$$


其中：

$$
P=P_1\otimes P_2\otimes ... \otimes P_n
$$


P_i 可以是：

- X
- Y
- Z


---

## 4. Optimization Process


Macroscopic optimization 通常包含：

1. Structure recognition

识别电路中的特殊结构。


2. Conversion

将普通 gate sequence 转换为高级表示。


3. Algebraic optimization

利用数学规则进行化简。


4. Decomposition

重新转换为硬件支持的 gate set。


---

## 5. Difference from Peephole Optimization


Peephole Optimization:

- small window
- local pattern
- individual gates


Macroscopic Optimization:

- large subcircuit
- global structure
- mathematical representation


---

## 6. Implementation in t|ket>


t|ket>通过 IR 对电路进行分析。

如果发现某个 subcircuit 具有特殊结构：

例如：

- Pauli gadget
- Clifford structure


则可以应用对应优化规则。


---

## 7. Mathematical Foundation


Macroscopic Optimization 需要：

1. Linear algebra

理解 unitary equivalence。


2. Quantum information

理解：

- Pauli operators
- Clifford circuits


3. Group theory

理解：

- Clifford group
- algebraic transformation


---

## 8. Main Idea


Peephole optimization:

"发现小错误并修正"


Macroscopic optimization:

"发现隐藏结构并重新表达"