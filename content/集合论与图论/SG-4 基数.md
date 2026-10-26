---
publish: true
---

# SG-4 基数
## $\mathrm I$ 集合的基数
### $\it 1.1$ 基数的概念
**等势**。设 $A, B$ 为两个集合，称 $A$ 与 $B$ 等势，若存在 $f:A\to B$ 为双射。记作 $A\approx B$。
- 或者 $\overline{\overline{A}}=\overline{\overline{B}}$，以及 $\mathrm{card}A=\mathrm{card}B$，都表示等势。

离散情形的重要结果。所有偶数、所有奇数、自然数 $\mathbb N$、整数 $\mathbb Z$、二维离散平面 $\mathbb N^2$ 以及 $\mathbb N^n$ 都是互相等势的。接下来整理一些重要的证明技巧。

### $\it 1.2$ 经典证明
$\mathbb N^2\approx\mathbb N$ 的经典证明。对角线证明方法，考虑蛇形对角线：
$$
\begin{matrix}
1 & 2 & 6 & 7 & \cdots \\ 3 & 5 & 8 & 14 \\ 4 & 9 & 13 \\ 10 & 12 \\ 11 \\ \vdots
\end{matrix}
$$
容易验证该函数建立了 $\mathbb N$ 到 $\mathbb N^2$ 的双射。

$\mathbb N\approx\mathbb Q$ 的证明。通过 $\mathbb N^2=\mathbb N$，我们知道可列个可列集的并是可列的。根据 $\mathbb Q$ 的定义，其可以表述为：
$$
\mathbb Q=\bigcup\limits_{p=1}^\infty\left\{ \dfrac{q}{p}\mid q\in\mathbb Z \right\}
$$
令 $A_p:=\left\{ \dfrac{q}{p}\mid q\in\mathbb Z \right\}$，那么每一个 $A_p$ 都是可列的，而 $p=1, 2, \cdots$，总体也是可列的。

$(0, 1)\approx\mathbb R$ 的证明。$y=e^x$ 建立了 $\mathbb R\to\mathbb R^+$ 的双射。$y=e^{-x}$（$x>0$）建立了 $\mathbb R^+\to(0, 1)$ 的双射。或者 $y=\dfrac1x$ 或者 $y=\arctan x$，其都建立了 $\mathbb R$ 与某个有限区间 $(a, b)$ 的双射。
- $(0, 1)\approx[0, 1]$ 的证明。可以考虑将 $0$“塞进”其中，例如 $0\to\dfrac12, \dfrac12\to\dfrac13, \dfrac13\to\dfrac14, \cdots$。这同样建立了双射。对 $1$ 同理。
### $\it 1.3$ Cantor 定理
$\mathbb N\not\approx\mathbb R$。证明。因为 $\mathbb R\approx[0, 1]$，只需证 $\mathbb N\not\approx[0, 1]$。采用反证法，假设 $\mathbb N\approx[0, 1]$，则存在双射 $f:\mathbb N\to[0, 1]$，记 $f(n)=x_{n+1}$。所以 $\mathrm{ran}f=[0, 1]=\{x_1, x_2, \cdots, x_n, \cdots\}$。
- 将 $x_i$ 表示成十进制小数形式，即 $x_i=0.a_1^{(i)}a_2^{(i)}a_3^{(i)}\cdots$。如果存在 $0.14999\cdots$ 的小数，规定其表示为 $0.15000\cdots$。
- 构造一个小数 $b=0.b_1b_2b_3\cdots$ 使得对任意 $i\ge1$，均有 $b_i\neq a_i^{(i)}$（且其不以无穷 $9$ 结尾）。

那么 $b$ 不等于任何一个 $a_i$，这与 $[0, 1]=\{x_1, x_2, \cdots, x_n, \cdots\}$ 矛盾。证毕。

设 $A$ 是任意的集合，那么 $A\not\approx P(A)$。
- 证明：否则，存在双射函数 $f:A\to P(A)$。定义 $B:=\{x\in A\mid x\notin f(x)\}$，则 $B\in P(A)$。根据假设，存在唯一的 $x_0\in A$ 使得 $f(x_0)=B$。
- 下面分析是否有 $x_0\in f(x_0)$。若其为真，则根据 $B$ 的定义，$x_0\notin f(x_0)$。若其为假，则其满足性质 $x_0\notin f(x_0)$，根据 $B$ 的定义有 $x_0\in f(x_0)$。这类似 Russell 悖论，命题为假。
## $\mathrm{II}$ 无穷集合与有穷集合
### $\it 2.1$ 有穷集合的势的定义
定义。若集合 $A$ 与某一个自然数 $n$ 等势，即 $A\approx n$，则称 $A$ 是有穷集合，否则称 $A$ 是无穷集合。

为了让每一个自然数能代表唯一的势，我们就要证明：若 $m\neq n$，则 $m\not\approx n$。由三歧性，不妨设 $m\in n$，那么得到 $m\subseteq n$，而不能取等，所以 $m\subset n$。接下来证明：若 $m\subset n$，则 $m\not\approx n$。这相当于证明：设 $n\in\mathbb N$，那么任意 $f:n\to n$ 是单射，则 $f$ 是满射。
- 使用归纳法，设 $S:=\{n\in\mathbb N\mid \forall f(f\in(n\to n)\land f是单射\to\mathrm{ran}f=n)\}$。
- 对于 $0$ 而言，只有 $\varnothing:\varnothing\to\varnothing$，因此 $0\in S$。
- 假设 $n\in S$。考虑 $n^+$ 以及任意一个单射的 $f:n^+\to n^+$。设 $\overline f:=f\restriction n:n\to n^+$，那么 $\overline f$ 也是单射。若 $n$ 在 $f$ 下是封闭的，即 $f\restriction n\in(n\to n)$，则由归纳假设 $\mathrm{ran}\overline f=n$。又 $f$ 是单射，只能有 $f(n)=n$。也就是 $\mathrm{ran}f=\mathrm{ran}\overline f\cup\{n\}=n^+$，归纳成立。
- 若 $n$ 在 $f$ 下不封闭，即存在 $m\in n$ 使得 $f(m)=n$，因此 $f(n)\in n$。考虑 $\hat f:n^+\to n^+$ 满足 $\hat f(x)=\begin{cases} n \quad x=n \\ f(n)\quad x=m \\ f(x)\quad \mathrm{otherwise} \end{cases}$。由前一种情形得知 $\mathrm{ran}\hat f=n^+$。因此 $\mathrm{ran}f=\mathrm{ran}\hat f=n^+$，归纳成立。

由此得到，$S=\mathbb N$。
### $\it 2.2$ 有穷集合与无穷集合的重要性质
不存在与自己真子集等势的有穷集合。
- 证明。反证法，否则存在有穷的 $B\subset A$ 使得 $A\approx B$，故存在双射 $f:A\to B$。由于 $B\subset A$，所以存在一个 $a\in A-B$，所以 $a\notin B=\mathrm{ran}f$。
- 由于 $A$ 有穷，故存在 $n\in\mathbb N$ 使得 $A\approx n$。于是存在双射 $g:A\to n$，以及 $g^{-1}:n\to A$ 也是双射。令 $h=(g\restriction B)\circ f\circ g^{-1}$，那么

推论（自上一条）：
1. 任何与自己真子集等势的集合都是无穷集。
2. $\mathbb N$ 是无穷集。

任何有穷集合都存在唯一的自然数 $n$ 与其等势。

任何有穷集合的子集仍然为有穷集合。
- 引理。设 $c\subseteq n$，则 $c$ 与某个属于 $n$ 的自然数等势。
## $\mathrm{III}$ 基数的性质
### $\it 3.1$ 定义基数
设 $A$ 为任意一个集合，称其中“元素个数”为 $A$ 的基数，记作 $\mathrm{card}A$。
- 对任意集合 $A, B$，规定 $\mathrm{card}A=\mathrm{card}B\iff A\approx B$。
- 对任意有穷集合 $A$，规定与之等势的（唯一）自然数为其基数，记作 $\mathrm{card}A=n$。
- 规定 $\mathrm{card}\mathbb N=\aleph_0$。
- 规定 $\mathrm{card}\mathbb R=\aleph$。

称 $0, 1, 2\cdots$ 为**有穷基数**，称 $\aleph_0, \aleph$ 等为无穷基数。用 $\kappa, \lambda, \mu$ 表示任意的基数。
### $\it 3.2$ 比较基数
设 $A, B$ 是任意的两个集合。
- 若存在 $f:A\to B$ 为单射，则称 $B$ **优势于** $A$，或 $A$ **劣势于** $B$，记作 $A\preceq\cdot B$。
- 若 $A\preceq\cdot B$ 且 $A\not\approx B$，则称 $B$ 绝对优势于 $A$，或 $A$ 绝对劣势于 $B$，记作 $A\prec\cdot B$。

定理。设 $A, B$ 为两集合，则 $A\preceq\cdot B\iff\exists C(C\subseteq B\land A\approx B)$。
- 推论。若 $A\subseteq B$，则 $A\preceq\cdot B$。
- 推论。若 $A\approx B$，则 $A\preceq\cdot B$ 且 $B\preceq\cdot A$。

定义基数的比较。设 $\kappa, \lambda$ 为两个基数，那么：
- $\kappa\le\lambda\iff\exists A\exists B(\mathrm{card}A=\kappa\land\mathrm{card}B=\lambda\land A\preceq\cdot B)$。
- $\kappa<\lambda\iff\exists A\exists B(\mathrm{card}A=\kappa\land\mathrm{card}B=\lambda\land A\prec\cdot B)$。
### $\it 3.3$ Schroder-Bernstein 定理
设 $A, B$ 为两个集合，若 $A\preceq\cdot B$ 且 $B\preceq\cdot A$，那么 $A\approx B$。
- 结论再自然不过，但已知条件只有存在两个双射，这两个双射不但很可能不对应，而且对于无穷集很可能都覆盖不全（例如对 $\mathbb N\to\mathbb N$，取 $f(x)=2x$ 以及 $g(x)=3x$）。

先给出构造的结果。存在单射 $f:A\to B$ 以及单射 $g:B\to A$。若 $A-\mathrm{ran}g=\varnothing$，则 $g$ 是满射，因此 $g$ 是双射，$A\approx B$ 成立。否则设 $C_0=A-\mathrm{ran}g$，$C_{n^+}=g(D_n)$，$D_n=f(C_n)$。令 $h:A\to B$ 满足 $h(x)=\begin{cases} f(x)\quad\exists n(n\in\mathbb N\land x\in C_n) \\ g^{-1}(x)\quad\mathrm{otherwise} \end{cases}$。我们需要证明 $h$ 是双射。
#### $\it 3.3.1$ 人话
考虑根据 $f$ 和 $g$ 构造一组双射 $h:A\to B$。对于 $g^{-1}$，其只有部分 $A$ 中的元素在 $B$ 中有对应，因此这是“保底”，而其余的 $A-\mathrm{ran}g$ 的元素就只能依靠 $f$。那么 $C_0$ 中的点匹配的对象就是 $D_0=f(C_0)$。那么 $D_0$ 中的元素就被 $C_0$ 预定了，其依赖 $g$ 关联的 $A$ 中的元素 $C_1=g(D_0)$ 就“失配”了（且 $C_1\cap C_0=\varnothing$，因为 $C_0\notin\mathrm{ran}g$）。所以 $C_1$ 不能找 $g^{-1}$，就只能找 $f$，因此其匹配 $D_1=f(C_1)$（$D_1\cap D_0=\varnothing$，因为 $C_1\cap C_0=\varnothing$，以及 $f$ 是单射）。以此类推，因为 $f$ 和 $g$ 都是单射，所以任意的 $C_i$ 之间不交，以及任意的 $D_i$ 之间不交。

那么这个无穷的过程是否会停止？考虑 $C_{\mathrm{All}}=\bigcup\limits_{k=0}^\infty C_k$ 也就是所有 $C_k$ 的并集，其内的点都找到了 $B$ 的匹配。其显然是单调增长的，且其存在上界 $C_{\mathrm{All}}\subseteq A$。因此其必然存在一个“上确界”。若该上确界不等于 $A$，那么 $A-C_{\mathrm{All}}$ 就用 $g^{-1}$ 匹配（一定不会干涉 $D_i$，因为 $D_i$ 可以映射到 $C_{i+1}$）。
#### $\it 3.3.2$ 鬼话
首先证明任意的 $C_i$ 两两不交，以及任意 $D_i$ 两两不交。
- $C_0=A-\mathrm{ran}g$，$C_1=g\circ f(C_0)\subseteq\mathrm{ran}g$，因此 $C_0\cap C_1=\varnothing$。
- 归纳，假设对 $1\le i\le n$ 有 $C_i$ 两两不交以及对 $1\le i<n$ 有 $D_i$ 两两不交。那么因为 $f$ 是单射，因此 $f(C_n)$ 与 $f(C_{n-1}), f(C_{n-2}), \cdots, f(C_0)$ 不交，即 $D_n$ 跟其余 $D_i$（$1\le i<n$）不交。同理，$g$ 是单射，所以 $C_{n+1}=g(D_n)$ 与 $g(D_0), g(D_1), \cdots, g(D_n)$ 不交，也就是 $C_1, C_2, \cdots, C_n$。同时其跟 $C_0$ 不交，因为 $C_0\not\subseteq\mathrm{ran}g$。因此该假设对于 $n+1$ 也成立，归纳成功。

写出总集，令 $E:=\bigcup\limits_{k=0}^\infty C_k$ 以及 $F:=\bigcup\limits_{k=0}^\infty D_k$。其一定有 $E\subseteq A$ 和 $F\subseteq B$。
- 证明 $E$ 和 $F$ 建立了双射。考虑 $D_n=f(C_n)$，$f$ 在 $C_n$ 到 $D_n$ 显然是满的，其同时是单的（$f$ 本身单），因此任意 $C_n\to D_n$ 都存在双射。那么对于 $f:E\to F$，其首先是单的，然后对任意的 $y\in F$，因为 $D_n$ 之间两两不交（一个对 $F$ 的划分），所以存在唯一的 $n\in\mathbb N$ 使得 $y\in D_n$，因此借助 $C_n\to D_n$ 的双射，存在一个 $x\in C_n$ 使得 $f(x)=y$，故 $f:E\to F$ 是满的。所以 $f:E\to F$ 是双射。

最后证明，$g^{-1}:A-E\to B-F$ 是双射。
- 考虑 $x\in A-E$，那么 $x\in\mathrm{ran}g$，所以存在 $y=g^{-1}(x)$。我们需要要证明 $y\in B-F$。若不然，则其在 $F$ 中，也就是其属于某一个 $D_n$，则 $g(y)\in C_{n+1}$，矛盾。
- 所以 $y\in B-F$。因为 $g$ 是单的，所以 $g^{-1}$ 在其定义域里自然也是单的。
- 接下来证明其同时是满的，任取 $y\in B-F$，那么 $g(y)\in A-E$（否则矛盾，同理）。
- 所以 $g^{-1}:A-E\to B-F$ 是双射。

总体而言，由 $f:E\to F$ 以及 $g^{-1}:A-E\to B-F$ 拼成的函数 $h$ 有效地建立了 $A\to B$ 的双射，因此 $A\approx B$。$\square$ 

这比教材上的垃圾证明好一万倍。

直接推论：若两基数 $\kappa\le\lambda$ 且 $\lambda\le\kappa$，那么 $\kappa=\lambda$。
### $\it 3.4$ Bernstein 定理的应用
使用 Bernstein 定理可以推出很多重要的已知结论。

定理。$\mathbb R\approx(\mathbb N\to 2)$。
- 证明。先考虑证明 $\mathbb R\preceq\cdot(\mathbb N\to 2)$，也就是 $(0, 1)\preceq\cdot(\mathbb N\to 2)$。任取 $x\in(0, 1)$，其有唯一的二进制表示（不以 $111\cdots$ 结尾），且其为单射。
- 证明 $(\mathbb N\to 2)\preceq[0, 1]$。将 $0.f(0)f(1)f(2)\cdots$ 按照十进制小数写出，那么该数属于 $\left[ 0, \dfrac19 \right]\subseteq[0, 1]$。若 $f$ 存在一个全为 $1$ 的末尾，那么可以对应加上 $\dfrac19$ 映射到 $\left[ \dfrac19, \dfrac29 \right]$。其为单射。

定理。设 $A$ 为任意无穷集合，那么 $\mathbb N\preceq\cdot A$。即：$\mathbb N$ 是最小的无穷集。
- 设 $\kappa$ 是基数，那么 $\kappa<\aleph_0$ 当且仅当 $\kappa=n$（有穷）。
- 有穷集合的子集一定是有穷集合。
- 设 $A$ 是 $\mathbb N$ 的无穷子集，那么 $\mathrm{card}A=\aleph_0$。
- 若 $\mathrm{card}A\le\aleph_0$，那么称 $A$ 为**至多可列**集。

定理。可数个可数集的并是可数集。
- 证明：Cantor 构造的蛇形对角线。
## $\mathrm{IV}$ 基数的运算法则
### $\it 4.1$ 运算法则的定义
准备工作。若 $A_1, A_2, B_1, B_2$ 是四个集合，且 $A_1\approx A_2, B_1\approx B_2$，那么：
1. 若 $A_1\cap B_1=A_2\cap B_2=\varnothing$，则 $A_1\cup B_1\approx A_2\cup B_2$。
2. $A_1\times B_1\approx A_2\times B_2$。
3. $A_1\to B_1\approx A_2\to B_2$

**定义基数的运算**。设 $\kappa, \lambda$ 是两个基数。那么
1. $\kappa+\lambda=\mathrm{card}(A\cup B)$，其中 $\mathrm{card}A=\kappa, \mathrm{card}B=\lambda$ 以及 $A\cap B=\varnothing$。
2. $\kappa\cdot\lambda=\mathrm{card}(A\times B)$。
3. $\kappa^\lambda=\mathrm{card}(B\to A)$，其中 $\mathrm{card} A=\kappa, \mathrm{card}B=\lambda$。注意方向不要搞错了。

基于上面的定理，任取 $\mathrm{card}A=\kappa$ 以及 $\mathrm{card}B=\lambda$，都不会影响计算的结果。
### $\it 4.2$ 基数运算的性质
1. $\kappa+\lambda=\lambda+\kappa$，$\kappa\cdot\lambda=\lambda\cdot\kappa$。
2. $(\kappa+\lambda)+\mu=\kappa+(\lambda+\mu)$，$\kappa(\lambda\mu)=(\kappa\lambda)\mu$。
3. $\kappa\cdot(\lambda+\mu)=\kappa\cdot\lambda+\kappa\cdot\mu$。

上面这些性质比较基本。

1. $\kappa^{\lambda+\mu}=\kappa^\lambda\cdot\kappa^\mu$。
2. $(\kappa\cdot\lambda)^\mu=\kappa^\mu\cdot\lambda^\mu$。
3. $(\kappa^\lambda)^\mu=\kappa^{\lambda\cdot\mu}$。

### $\it 4.3$ 额外推论
推论。设 $\kappa$ 是无穷基数，则 $\kappa^2=\kappa$。

设 $\kappa$ 是无穷基数，则 $2^\kappa=\kappa^\kappa$。

基数的三歧性：任意两个基数 $\kappa, \lambda$，一定成立 $\kappa<\lambda\lor\kappa=\lambda\lor\kappa>\lambda$ 恰好其一（应用选择公理）。
