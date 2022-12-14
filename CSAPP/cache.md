# 高速缓存

## 局部性原理(locality)

### 时间局部性与空间局部性

局部性通常有两种不同的形式，时间局部性(temporal locality)和空间局部性(spatial locality)。在一个具有良好时间局部性的程序中，被引用过一次的内存位置很可能在将来再被多次引用。再一个具有良好空间局部性的程序中，如果一个内存位置被引用了一次，那么程序很可能再不远的将来引用附近的一个内存位置。

### 利用局部性原理提高程序效率

- 一般而言，具有良好局部性原理的程序比局部性差的程序运行的更快。
- 重复引用相同变量的程序具有良好的时间局部性
- 对于具有步长为k的引用模式的程序，步长越小，空间局部性越好
- 对于取指令来说，循环具有好的时间和空间局部性。循环体越小，循环迭代次数越多，局部性越好

## 存储器层次结构

### 基本属性

- **存储技术**：不同存储技术访问时间差异很大。速度较快的技术每字节的成本比速度较慢的技术高，而且容量较小
- **计算机软件特性**：一个编写良好的程序倾向于展示出良好的局部性
- 硬件和软件的基本属性某种意义上是互相补充的
- **存储器层次结构**：从高层往底层走，存储设备变得更慢、更便宜和更大

### 缓存

- 高速缓存 (cache) 是一个小而快速的存储设备，它作为存储更大、也更慢的设备中的设备中的数据的缓冲区域。
- 对于每个 k，位于 k 层的更快更小的存储设备作为位于 k + 1 层的更大更慢的存储设备的缓存。换句话说，存储器层次结构中的每一层都缓存来自较低一层的数据对象。
- 在逻辑上将内存划分为多个数据块，数据以块大小为传送单元在层之间来回传递

### 几个缓存设计原则

- 每一个缓存项都要包含**标签**（ID）和**内容**
- 需要一种快速有效的机制用来判断给定的数据是否被缓存
  - 通过对有效位置建立索引和约束规则
- 未命中时，通常要选择一个现有缓存项，并被新缓存项所替代
  - 称为**替换策略** (replacement policy)
- 在写入时，需要向下传递更改的内容，或将该缓存标记为**脏** (dirty)
  - **直写** (write-through) 与**回写** (write-back)
- 不同的缓存场景采用不同的解决方案

### 高速缓存的结构

- 一个存储器地址有 m 位的机器的高速缓存被组织成为有 S = 2 ^ s 个 **高速缓存组** (cache set) 的数组，每个组包含 E 个**高速缓存行** (vavhe line)，每一行由一个 B = 2 ^ b 字节的数据块 (block)，一个**有效位** (valid bit)，t = m - (b + s) 个**标记位** (tag bit) 组成
- 参数 S 和 B 将 m 个地址分为了三个字段：标记、组索引和块偏移。**组索引**是一到 S 个组的数组的索引，**标记**表明这个组中哪一行包含这个字（如果有的话）。在确定了由标号多标识的行后，**块偏移**位表示在 B 个字节的数据块中的字偏移
- 写操作
  - 写命中
    - 直写 (write-through)：立即写入主存
    - 回写 (write-back)：至行被替换时才写入主存
  - 写未命中
    - 写分配 (write-allocate)：加载至缓存，然后更新缓存中的行
    - 非写分配 (no-write-allocate)：直接写入内存，不加载数据块至缓存

### 高速缓存的性能指标

- **不命中率** (miss rate)：不命中数量/引用数量
- **命中率** (hit rate)：1 - miss rate
- **命中时间** (hit time)：将缓存中的行数据发送到处理器所需的时间，包括判定该行是否在缓存中的时间
- **不命中处罚** (miss penalty)：由于缓存未命中所需要的额外数据访问时间
