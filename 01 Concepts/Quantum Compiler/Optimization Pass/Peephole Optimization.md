## 1. Definition

Peephole Optimization（窥孔优化）是一种局部电路优化方法。

其核心思想是：

在电路中寻找较小范围内的特定 gate pattern，并利用数学等价关系将其替换为更简单的结构。


优化目标：

- Reduce gate count
- Reduce CX count
- Reduce circuit depth


---

## 2. Basic Principle

量子线路可以表示为 unitary operator 的乘积：

$$
U=U_nU_{n-1}...U_2U_1
$$


如果两个不同的 gate sequence 满足：

$$
U_A=U_B
$$

那么它们表示相同的量子操作，可以互相替换。


优化的本质：

寻找：

较复杂的 unitary decomposition

↓

较简单的 unitary decomposition


---

## 3. Examples


### 3.1 Identity cancellation

Hadamard gate:

$$
H^2=I
$$


因此：

H  H

可以删除。


---

### 3.2 Gate inverse cancellation

对于 unitary：

$$
UU^\dagger=I
$$


因此：

一个 gate 和它的逆操作连续出现时，可以消除。


例如：

CX  CX


因为：

$$
CX^2=I
$$


可以删除。


---

## 4. Implementation in t|ket>

t|ket>首先将量子线路转换为 IR，例如 DAG representation。

Peephole optimization 会扫描 DAG：

寻找：

node1 → node2 → node3


这样的局部结构。


如果发现该结构满足某个数学恒等式：

则替换 DAG 中对应部分。


流程：

Circuit

↓

DAG IR

↓

Pattern matching

↓

Circuit rewriting


---

## 5. Characteristics

优点：

- 实现简单
- 计算速度快
- 适合大量局部优化


缺点：

- 只能看到局部结构
- 无法识别大型 subcircuit 的隐藏结构


---

## 6. Mathematical Foundation

Peephole optimization 主要依赖：

1. Linear algebra

量子门等价关系来自矩阵乘法。


2. Unitary operator

量子线路对应 unitary transformation。


3. Circuit identity

利用量子门之间的恒等式进行替换。

