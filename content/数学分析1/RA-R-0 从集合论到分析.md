---
publish: true
---

（**草稿，待更新中**）

# 数学分析1 笔记 (0)
## $\mathrm I$ 集合论基础
其实很多知识在《集合论》中有详细的讲解，有关集合论、函数与映射的内容暂且不作记录。
### $\it 1.1$ 集合的基数（势）
主要先做一些结论性的陈述，等到系统学习集合基数之后再做整理。

省流，在数分范围中，所遇到的基数为：有限数 $n$、可列集 $\aleph_0$ 以及不可列集 $\aleph$。
### $\it 1.2$ 基础代数方法
## $\mathrm{II}$ 从集合到实数系
### $\it 2.1$ 自然数公理
自然数公理由 Peano 发表：
1. $0$ 属于 $\mathbb N$。
2. 每一个确定的自然数 $n$ 有唯一的后继 $n^+$。
3. 没有以 $0$ 为后继的自然数。
4. 不同自然数对应不同的后继，即：若 $m\neq n$，则 $m^+\neq n^+$。
5. 若 $E\subseteq\mathbb N$，其包含 $0$ 以及其每一个元素的后继，则 $E=\mathbb N$。

从自然数公理中可以看出自然数是**全序结构**（按照后继关系定义）。可以通过后继的形式（递归的）定义加法（$n+0=n$ 且 $m+n^+=(m+n)^+$）。然后定义乘法，将 $\mathbb N$ 扩充为 $\mathbb Z$ 以及 $\mathbb Q$。

**有理数的稠密性**。任意给定 $a, b\in\mathbb Q$，存在 $(a, b)$ 中的有理数。
- 取 $r=\dfrac{a+b}{2}$ 即可。
- 有理数的 Archimedes 性：若 $p$ 为正有理数，则对任意有理数 $q$，存在 $n\in\mathbb N$ 使得 $np>q$。证明不难，将有理数写成 $\dfrac ab$ 即可。

**数学归纳法**基于自然数公理。

自然数的 **von Neumann** 构造。定义 $0:=\varnothing$，$1:=\{\varnothing\}=\{0\}$，$2:=\{\varnothing, \{\varnothing\}\}=\{0, 1\}$。递归地定义 $n+1:=\{0, 1, 2, \cdots, n\}=n\cup\{n\}$。这直接给出了基于空集的自然数构造，因此我们可以不再将 Peano 自然数公理视作“公理”。
### $\it 2.2$ 实数公理
从这里开始，正式引入实数系以及其上的各种运算。
#### $\it 2.2.1$ 实数系公理体系
$\mathbb R$ 首先应当有加法、乘法、全序关系。满足下列公理系统的 $(\mathbb R, +, \cdot, \le)$ 被称为**实数系**。
- (F) 域公理。$(\mathbb R, +\, \cdot)$ 是一个域。满足加法结合律、加法交换律。存在加法单位元 $0$，存在加法负元 $-x$。满足乘法结合律、乘法交换律。存在乘法单位元 $1$ 以及乘法逆元 $x^{-1}$（非零）。乘法与加法之间存在分配律 $x(y+z)=xy+xz$。
- (O) 序公理。
- (C) **连续公理**。
	- Archimedes 公理：给定 $y$ 与 $x>0$，存在 $n\in\mathbb N$ 使得 $nx:=x+x+\cdots+x(n次)>y$。
	- **完备公理**：若 $\mathbb R'\supseteq\mathbb R$ 且 $(\mathbb R', +, \cdot, \le)$ 满足公理 (F), (O), (C1)，则 $\mathbb R'=\mathbb R$。
#### $\it 2.2.2$ 定义实数系常用运算
在此定义一些常见的运算。
- **绝对值**。$|x|:=\begin{cases} x\quad x\ge 0 \\ -x\quad x<0 \end{cases}$。
- **阶乘**。对 $n\in\mathbb N$ 定义 $n(n-1)(n-2)\cdots \cdot 2\cdot 1$。定义双阶乘 $n!!$ 为 $n(n-2)(n-4)\cdots\cdot 2$（此处为偶数，若为奇数则应当停止在 $1$）。

**实数系的重要运算规则**。
- 三角不等式。$|x+y|\le|x|+|y|$。
- Newton 二项展开式。$n\in\mathbb N$，则 $(x+y)^n=\sum\limits_{k=0}^nx^ky^{n-k}\dbinom nk$。
#### $\it 2.2.3$ 广义实数系
广义实数系相比实数系所新增的元素为正、负无穷。
### $\it 2.3$ Dedekind 分割构造实数系
### $\it 2.4$ 上确界存在原理
上确界存在原理是直接来源于 Dedekind 分割的原理，其等价于完备公理。其内容是：**任何 $\mathbb R$ 中的非空上有界集都存在上确界**。
- 证明依赖 Dedekind 分割，暂时不作讨论。
- 类似有下确界存在原理。
- 有关上确界存在原理的理解，可以对照 $\mathbb Q$ 来理解。考虑将 $\sqrt 2$ 的小数的前 $k$ 位写称序列，那么该集合的上确界不存在于 $\mathbb Q$ 中。而 $\mathbb R$ 不存在这样的问题，因为实数系是完备的。
	- 这等价于，任意实数系中的极限行为，其结果一定落在实数系内。有待进一步证明。

需要知道一种等价写法。若 $M=\sup A$，则其等价于如下两条：
- $\forall x(x\in A\to x\le M)$。
- $\forall\varepsilon(\varepsilon>0\to\exists x(x\in A\land |M-x|<\varepsilon))$。
	- 第二条尤其重要，其相当于在说 $A$ 中一定存在元素 $x$ 使得其与 $M$ 的差距**足够小**（极限写法）。

有关是否存在多个实数系的问题：这涉及到序同构问题，是一些高深的代数内容。

### $\it 2.5$ 实数系的运算法则
我们到此建立的实数系的基本理论，在此之上定义各种常见的运算规则：
- 十进制表示实数：先定义 $\lfloor x\rfloor$ 表示 $\sup\{n\in\mathbb Z\mid n\le x\}$，然后定义 $\{x\}$ 表示 $x-\lfloor x\rfloor$。
	- 于是基于此可以定义十进制小数 $a_k:=\lfloor 10^kx\rfloor-10\lfloor 10^{k-1}x\rfloor$ 为 $x$ 在十进制下的第 $k$ 位小数。此时有 $x=\sup\limits_{k\ge 1}\left( \sum\limits_{i=0}^k\dfrac{a_i}{10^i} \right)$。
	- 类似定义 $p$ 进制数。
	- 一个重要的结果为，十进制小数的全体等价于 $\mathbb R$（作业题）。根据十进制小数同样可以构造出 $\mathbb R$。这本质上是在拿有理数逼近实数，与 Dedekind 分割是同样的。
- $n$ 次方根。设 $a>0$，而 $n\in\mathbb N$，则方程 $x^n=a$ 有唯一的正解。记作 $\sqrt[n]a$ 或 $a^{\frac 1n}$。
- 实指数幂与对数。可以拿上确界定义，也可以按照无穷级数的方法定义 $e^x$。
- 三角函数与反三角函数。
### $\it 2.6$ 常用不等式
**Prop**. **Bernoulli 不等式**：设 $h>-1, n\in\mathbb N^+$，则 $(1+h)^n\ge1+nh$。
- 其中 $n>1$ 时等号成立的充要条件是 $h=0$。 

> **Proof**. $n=1$ 或 $h=0$ 是显然成立。讨论 $n>1$ 且 $h\neq0$。
> 考虑 $(1+h)^n-1=h[1+(1+h)+(1+h)^2+\cdots+(1+h)^{n-1}]$。当 $h>0$ 时，右侧括号中每一项都大于等于 $1$，因此总体大于 $n$，整体大于 $nh$。当 $h<0$ 时，右侧括号中每一项都小于等于 $1$，总体小于 $n$（此时 $h<0$），整体大于 $nh$。

**Prop**. **平均值不等式**：设 $a_1, a_2, \cdots, a_n$ 是 $n$ 个非负实数，则 $\dfrac{a_1+a_2+\cdots+a_n}{n}\ge\sqrt[n]{a_1a_2\cdots a_n}$。
- 其中等号成立的充要条件是 $a_1=a_1=\cdots=a_n$。

> **Proof**. (Cauchy's Forward and Backward method) 考虑已知二元情形 $\dfrac{a+b}{2}\ge\sqrt{ab}$。归纳，若 $n=2^k$ 时不等式成立，则 $n=2^{k+1}$ 时有 $\boxed{\begin{aligned} \dfrac{1}{2^{k+1}}\sum\limits_{i=1}^{2^{k+1}}a_i&=\dfrac12\left( \dfrac1{2^k}\sum\limits_{i=1}^{2^k}a_i+\dfrac1{2^k}\sum\limits_{i=2^k+1}^{2^{k+1}}a_i \right) \\ &\ge \sqrt{\left( \dfrac1{2^k}\sum\limits_{i=1}^{2^k}a_i \right)\left( \dfrac1{2^k}\sum\limits_{i=2^k+1}^{2^{k+1}}a_i \right)} \\ &\ge\sqrt{\sqrt[2^k]{\prod\limits_{i=1}^{2^k}a_i}\cdot\sqrt[2^k]{\prod\limits_{i=2^k+1}^{2^{k+1}}a_i}} \\ &= \sqrt[2^{k+1}]{\prod\limits_{i=1}^{2^{k+1}}a_i} \end{aligned}}$。这是“向前”的部分。
> 第二部证明，若不等式对 $n>2$ 成立，则对 $n-1$ 也成立。考虑对于 $a_1, a_2, \cdots, a_{n-1}$，令第 $n$ 项 $a_n=\dfrac1{n-1}\sum\limits_{i=1}^{n-1}a_i$。于是 $\dfrac1{n-1}\sum\limits_{i=1}^{n-1}a_i=\dfrac1n\sum\limits_{i=1}^na_i\ge\sqrt[n]{\prod\limits_{i=1}^{n-1}a_i\cdot\dfrac1{n-1}\sum\limits_{i=1}^{n-1}a_i}$。将两侧同时变为 $n$ 次幂得到 $\left( \dfrac1{n-1}\sum\limits_{i=1}^{n-1}a_i \right)^{n-1}\ge\prod\limits_{i=1}^{n-1}a_i$。这正是 $n-1$ 元均值不等式。

**Prop**. **绝对值不等式**：$|a+b|\le|a|+|b|$。
- 用于讨论 $\mathbb R$ 上“距离”的放缩。

**Prop**. **Cauchy 不等式**（实线性空间向量内积）对任意实向量 $\mathbf a=(a_1, a_2, \cdots, a_n)$ 和 $\mathbf b=(b_1, b_2, \cdots, b_n)$，有 $\mathbf a^T\mathbf b\le\|\mathbf a\|\cdot\|\mathbf b\|$。即 $\left|\sum\limits_{i=1}^na_ib_i\right|\le\sqrt{\sum\limits_{i=1}^na_i^2}\cdot\sqrt{\sum\limits_{i=1}^nb_i}$。

> **Proof**. 考虑引入变量 $\lambda$ 的二次函数 $0\le\sum\limits_{i=1}^n(\lambda a_i-b_i)^2=\lambda^2\sum\limits_{i=1}^na_i^2-2\lambda\sum\limits_{i=1}^na_ib_i+\sum\limits_{i=1}^nb_i^2$。写出该关于 $\lambda$ 的二次方程的判别式，令其小于零即得到 Cauchy 不等式。能看出，取等条件存在一个 $\lambda$ 使得 $\lambda a_i=b_i$ 成立（成比例，当然更严谨可以写成 $\lambda a_i+\mu b_i=0$）。

**Prop**. **Young 不等式**：设 $p, q\in(1, +\infty)$ 满足 $\dfrac{1}{p}+\dfrac{1}{q}=1$，则对任意 $a, b>0$ 有 $ab\le \dfrac{1}{p}a^p+\dfrac{1}{q}b^q$。
- 这相当于 $a^{\lambda}b^{1-\lambda}\le\lambda a+(1-\lambda)b$，将 AM-GM 的系数 $\lambda$ 推广到了实数范围。
- 等价形式：例如 $(1+x)^\lambda\ge1+\lambda x$（$\lambda\ge1, x\ge 0$），还有几个其他等价形式。
