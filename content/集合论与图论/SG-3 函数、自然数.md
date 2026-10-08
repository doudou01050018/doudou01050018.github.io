---
publish: true
---

# SG-3 函数、自然数
这一节基本上是工具性的，其涉及的证明思路基本与二元关系一致。
## $\mathrm I$ 函数的基本概念
### $\it 1.1$ 函数是特殊的二元关系
**函数的定义**。设 $F$ 是一个二元关系，若 $F$ 是单值的，则称 $F$ 是函数或映射。即：任意的 $xFy\land xFz\to y=z$。将 $(x, y)\in F$ 以及 $xFy$ 记作 $F(x)=y$。
- **偏函数、前域**。设 $A, B$ 是两个集合，$F$ 是函数，且 $\mathrm{dom}F\subseteq A$，$\mathrm{ran}F\subseteq B$。则称 $F$ 是 $A$ 到 $B$ 的偏函数，记作 $F:A\xrightarrow | B$，并称 $A$ 是 $F$ 的前域。并定义 $A\xrightarrow |B:=\{F\mid F:A\xrightarrow |B\}$，即全体 $A$ 到 $B$ 的偏函数。
	- 若额外有 $\mathrm{dom}F\neq A$，则称 $F$ 是 $A$ 到 $B$ 的真偏函数，记作 $F:A\xrightarrow{||}B$。
- **全函数、后域**。设 $A, B$ 是两个集合，$F$ 是 $A$ 到 $B$ 的偏函数，且 $\mathrm{dom}F=A$。则称 $F$ 是 $A$ 到 $B$ 的全函数，记作 $F:A\to B$，并称 $B$ 是 $F$ 的后域。并定义 $A\to B:=\{F\mid F:A\to B\}$，即全体 $A$ 到 $B$ 的全函数。
### $\it 1.2$ 函数的性质
设 $f:A\to B$。
- 若 $\mathrm{ran}f=B$，则称 $f$ 是**满射**。
- 若 $f$ 是单根的（$F(x_1)=y\land F(x_2)=y\to x_1=x_2$），则称 $f$ 是**单射**。
- 若 $f$ 既是满射又是单射，则称 $f$ 是**双射**。

**像、原像**。设 $f:A\to B$。有 $A'\subseteq A$ 与 $B'\subseteq B$。定义 $f(A'):=\{y\mid y=f(x)\land x\in A'\}$ 为 $A'$ 在 $f$ 下的像。特别地，称 $f(A)$ 为函数 $f$ 的像。定义 $f^{-1}(B'):=\{x\mid x\in A\land f(x)\in B'\}$ 为 $B'$ 的**原像**。
- 特殊函数。若 $\exists c\forall x(x\in A\to f(x)=c)$，则称 $f$ 为常函数。若 $\forall x(x\in A\to f(x)=x)$，则称 $f$ 为恒等函数。将 $A$ 上的恒等函数记作 $I_A$。
- 特征函数。设 $A\subseteq U$。定义 $\chi_A:U\to\{0,1\}$，$\chi_A(x)=1$ 当且仅当 $x\in A$，否则 $\chi_A(x)=0$，称 $\chi_A$ 为 $A$ 的特征函数。

**单调关系**。设 $f:A\to B$。若 $\forall x,y\in A(x\preceq_A y\to f(x)\preceq_B f(y))$，则称 $f$ 关于 $\preceq_A,\preceq_B$ 单调递增；若 $\forall x,y\in A(x\preceq_A y\to f(y)\preceq_B f(x))$，则称 $f$ 单调递减。
- 类似定义**严格单调**。

**自然映射**。设 $R$ 是 $A$ 上的等价关系，$A / R$ 是 $A$ 关于R 的商集，定义 $g:A\to A / R$，$g(x)=[x]_R$，称 $g$ 为 $A$ 关于 $R$ 的自然映射。
- 人话：$g(x)$ 将 $x$ 映射到其所在的等价类 $[x]_R$。

例题。若 $|A|=n, |B|=m$ 且 $n\ge m$，有多少个不同的 $f:A\to B$ 是满射？
- 解法1：这相当于在问，$n$ 个标号（不同）的球放到 $m$ 的标号的盒子里，有多少种放法。若 $m$ 无标号，则答案即为第二类 Stirling 数 $\begin{Bmatrix} n \\ m \end{Bmatrix}$。计入标号，答案为 $m!\begin{Bmatrix} n \\ m \end{Bmatrix}$。
- 解法2：递推，考虑 $n$ 号球的位置，其所处的盒子或者只有一个球（$m\cdot f(n-1, m-1)$），或者有两个及以上的球（$m\cdot f(n-1, m)$）。递推式即为 $f(n, m)=m[f(n-1, m-1)+f(n-1, m)]$。
- 解法3：容斥原理 / 反演，先不考虑满射的限制，所有函数有 $m^n$ 个。减去至少有一个元素没有原像的函数。由容斥原理，答案为 $\sum\limits_{k=0}^{m}(-1)^k\dbinom{m}{k}(m-k)^n$。
### $\it 1.3$ 函数的复合
**定理**。若 $f:A\to B, g:B\to C$，则 $g\circ f:A\to C$。且 $\forall x(x\in A\to g\circ f(x)=g(f(x)))$。
- 证明。首先证明 $g\circ f$ 是函数。任取 $x\in\mathrm{dom}(g\circ f)$，若存在 $z_1, z_2\in C$ 使得 $x(g\circ f)z_1\land x(g\circ f)z_2$，即存在 $y_1, y_2\in B$ 使得 $xfy_1\land y_1gz_1\land xfy_2\land y_2gz_2$。因为 $f$ 是函数，所以 $xfy_1\land xfy_2\to y_1=y_2=y$。所以 $y_1gz_1\land y_2gz_2\to ygz_1\land ygz_2$。同理，因为 $g$ 是函数，所以 $z_1=z_2$。
- 然后证明 $\mathrm{dom}(g\circ f)=A$。根据定理，首先 $x(g\circ f)z\iff\exists y(xfy\land ygz)\Rightarrow \exists y(xfy)$。即 $x\in A$，故 $\mathrm{dom}(g\circ f)\subseteq A$。任取 $x\in A$，则 $y=f(x)\in B$，而 $z=g(y)\in C$，即存在 $y, z$ 使得 $xfy\land ygz$，故 $x(g\circ f)z$。即 $x\in\mathrm{dom}(g\circ f)$，故 $\mathrm{dom}(g\circ f)=A$。
- 而且 $g\circ f$ 是函数。任取 $x\in A$，存在唯一的 $y=f(x)\in B$ 使得 $xfy$，存在唯一的 $z=g(y)\in C$ 使得 $ygz$，故存在唯一的 $z=g(f(x))$ 使得 $x(g\circ f)z$。
- 综上，$g\circ f(x)=g(f(x))$。

**定理**。设 $f:A\to B$，$g:B\to C$。
- 若 $f, g$ 均满射，则 $g\circ f$ 也是满射。
- 若 $f, g$ 均单射，则 $g\circ f$ 也是单射。
- 若 $f, g$ 均双射，则 $g\circ f$ 也是双射。
- 若 $g\circ f$ 是满射，则 $g$ 是满射。
- 若 $g\circ f$ 是单射，则 $f$ 是单射。
- 若 $g\circ f$ 是双射，则 $f$ 是单射，$g$ 是满射。
### $\it 1.4$ 反函数
**反函数**。设 $A$ 是任一集合（不一定全是二元关系），但 $A^{-1}:=\{(y, x)\mid xAy\}$ 一定是二元关系。什么时候 $A^{-1}$ 是函数？$A^{-1}$ 为函数，当且仅当 $A$ 是单根的。
- 若 $f:A\to B$ 是双射，则 $f^{-1}:B\to A$，且其也是双射。称此时的 $f^{-1}$ 是 $f$ 的**反函数**。

**逆**。若 $f:A\to B$，$g:B\to A$，若 $g\circ f=I_A$，则称 $g$ 是 $f$ 的左逆。若 $f\circ g=I_B$，则称 $g$ 是 $f$ 的右逆。
- 定理。设 $f:A\to B$，则 $f$ 是单射当且仅当 $f$ 存在左逆；$f$ 是满射当且仅当 $f$ 存在右逆。
- $f$ 同时存在左、右逆当且仅当 $f$ 是双射，且此时左右逆相等，且其一定为 $f^{-1}$。
## $\mathrm{II}$ 自然数
### $\it 2.1$ Peano 公理
**封闭**。设 $F$ 为函数，若 $A\subseteq\mathrm{dom} F$ 满足 $x\in A\to F(x)\in A$，则称 $A$ 在 $F$ 下是封闭的。

**Peano 系统**。Peano 系统是形如 $(M, F, e)$ 的有序三元组。其中 $M$ 是集合，$F:M\to M$ 的函数，$e$ 为首元素。其五条公设为：
1. $e\in M$。
2. $M$ 在 $F$ 下是封闭的。
3. $e\notin\mathrm{ran} F$。
4. $F$ 是单射，即 $\forall m,n\in M(F(m)=F(n)\to m=n)$。
5. 若 $A\subseteq M$ 满足 $e\in A$ 且 $A$ 在 $F$ 下是封闭的，则 $A=M$。
- 人话：$e$ 是第一个元素，那么令 $e_1:=F(e)$，则由 $e\notin\mathrm{ran}F$ 得出 $e_1\neq e$。同时令 $e_2:=F(e_1)$，则因为 $F$ 是单射，所以 $e_2$ 与前两者都不同。这样就得到了彼此不同的 $e_1, e_1, e_3, \cdots$。
- 第五条被称为**极小性假设**。令 $A=\{e, e_1, e_2, \cdots\}$，那么 $A\subseteq M$ 且 $A$ 对 $F$ 封闭。所以 $A=M$。也就是从 $e$ 出发得到了这一系列元素就是 $M$，其不允许含有任意不在这条链上的多余值。

**后继与归纳集**。设 $A$ 是集合，则定义后继运算 $A^+:=A\cup\{A\}$。
- 后继运算满足，$A\subseteq A^+$ 且 $A\in A^+$。
- **归纳集**。设 $A$ 是集合，若其满足 $\varnothing\in A$ 且 $\forall a(a\in A\to a^+\in A)$，则称 $A$ 是归纳集。
	- 任意的归纳集一定包含了 $\varnothing, \varnothing^+, \varnothing^{++}, \cdots$，因此将其定义为自然数。
### $\it 2.2$ 自然数的定义与性质
**自然数的定义**。设 $D:=\{V\mid V\mathrm{ 是归纳集}\}$，则 $\mathbb N:=\bigcap\limits D$（注：这个定义依赖无穷公理）。
- 根据这个定义证明 $\mathbb N$ 是归纳集：首先任意的归纳集包含 $\varnothing$，因此 $\varnothing\in\mathbb N$。然后，任取 $a\in\mathbb N$，则 $\forall V(V\in D\to a\in V)$，即 $\forall V(V\in D\to a^+\in V)$，故 $a^+\in\mathbb N$，因此 $\mathbb N$ 是归纳集。
- $\mathbb N$ 是最小的归纳集。任取归纳集 $V$，则由定义，$\mathbb N\subseteq V$。

**自然数集的性质**。
- （Peano 公设 5）若 $S\subseteq\mathbb N$ 满足 $\varnothing\in S$ 且 $n\in S\to n^+\in S$（后继封闭），则 $S=\mathbb N$。
	- 证明。考虑证明 $\mathbb N\subseteq S$。根据定义，$S$ 是归纳集。因此 $S\in D$。于是 $\mathbb N=\bigcap\limits D\subseteq S$。
- **数学归纳法**。如果要证明 $\forall n(n\in\mathbb N\to P(n))$，则可以先构造集合 $S:=\{n\mid n\in\mathbb N\land P(n)\}$。由 $S$ 的构造知 $S\subseteq\mathbb N$。如果能证明 $S$ 是归纳集，则由公设 5 可知 $S=\mathbb N$。
- 设 $n\in\mathbb N$，$m\in n$，则 $m\subseteq n$。
	- 证明。这个命题需要从 $\varnothing$ 开始归纳，无法凭空证明。构造 $S:=\{n\mid n\in\mathbb N\land\forall x(x\in n\to x\subseteq n)\}$。那么我们需要证明其是归纳集。
	- 当 $n=\varnothing$ 时，$n$ 中没有任何元素，故 $\forall x(x\in n\to x\subseteq n)$ 平凡成立，$\varnothing\in S$。
	- 假设 $n\in S$。任取 $x\in n^+=n\cup\{n\}$。若 $x\in n$，则由 $n\in S$ 得 $x\subseteq n\subseteq n^+$。若 $x=n$，则 $x=n\subseteq n^+$。因此 $n^+\in S$。
	- 由归纳原理，$S=\mathbb N$，即命题成立。
	- 分析。这条定理说明，在由 $\varnothing$ 构造的归纳集中，**$\in$ 单向 $\subseteq$ 等价于**。但反过来则不成立（丢失信息），因为 $m\subseteq n$ 中的 $m$ 可能不具有一个自然数的结构。
- 设 $n, m\in\mathbb N$。若 $m^+=n^+$，则 $m=n$。
	- 证明。反证，若 $m\neq n$，则 $n\in n^+=m^+=m\cup\{m\}$。因为 $n\neq m$，所以只能是 $n\in m$。根据上一条，$n\subseteq m$。而 $n\neq m$，所以 $n\subset m$。
	- 同理 $m\subset n$，矛盾。因此 $m^+=n^+\to m=n$。
- **自然数是 Peano 系统**。若记 $\sigma:\mathbb N\to\mathbb N$ 使得 $\sigma(n)=n^+$，则 $(\mathbb N, \sigma, \varnothing)$ 是 Peano 系统。
	- 证明。第一条，首先 $\varnothing\in\mathbb N$。第二条，因为 $\mathbb N$ 是归纳集，所以 $n\in\mathbb N\to n^+\in\mathbb N$。第三条，任意取 $n\in\mathbb N$，则 $n^+=n\cup\{n\}\neq\varnothing$，因此 $\varnothing\notin\mathrm{ran}\sigma$。
	- 第四条，证明 $\sigma$ 单射，即 $m^+=n^+\to m=n$，已经证明。
	- 第五条（如上）。
- 设 $n, m\in\mathbb N$。则 $m^+\in n^+\iff m\in n$。
	- 证明。必要性。若 $m^+\in n^+$，则 $m^+\in n\lor m^+=n$。如果是 $m^+\in n$，则 $m^+\subseteq n$；如果 $m^+=n$，那么自然也有 $m^+\subseteq n$。总之即 $m\subseteq n\land\{m\}\subseteq n$，也就是 $m\in n$。
	- 充分性。用数学归纳法，设 $S=\{n\mid n\in\mathbb N\land\forall m(m\in n\to m^+\in n^+)\}$。需要证明其为归纳集。首先 $\varnothing\in S$ 自然成立。若 $n\in S$，则 $\forall m(m\in\mathbb N\to m^+\in n^+)$。考虑 $n^+$ 以及 $\forall m(m\in n^+)$，则 $m\in n\lor m=n$。若 $m\in n$，则 $m^+\in n^+\subseteq n^{++}$。若 $m=n$，则 $m^+=n^+\in n^{++}$。因此 $n^+\in S$。归纳成立。
- 设 $n\in\mathbb N$，则 $n\notin n$。
	- 证明。数学归纳法，设 $S=\{n\mid n\in\mathbb N\land n\notin n\}$。首先 $\varnothing\in S$。考虑已知 $n\notin n$。假设有 $n^+\in n^+$，那么 $n^+\in n\lor n^+=n$。如果 $n^+\in n$，则 $n^+\subseteq n$，即 $n\subseteq n\land n\in n$，矛盾。如果 $n^+=n$，同样有 $n^+\subseteq n$，得出一样的矛盾。因此 $n^+\notin n^+$，即 $n^+\in S$。归纳成立。
- 设 $n\in\mathbb N$ 且 $n\neq\varnothing$，则 $\varnothing\in n$。
	- 证明。数学归纳法，设 $S=\{0\}\cup\{n\mid n\in\mathbb N\land n\neq\varnothing\land\varnothing\in n\}$。首先 $\varnothing\in S$。若 $n\in S$，如果 $n=0$，则 $\varnothing\in\{\varnothing\}$。否则 $\varnothing\in n$，所以 $\varnothing\in n\cup\{n\}=n^+$。归纳成立。
- **三歧性**。任取 $n, m\in\mathbb N$，则 $m\in n, m=n, n\in m$ 有且仅有一成立。
	- 先证至多成立一式。假设 $m=n$ 成立，则 $n\notin n$，其余两式均不成立。假设 $m\in n$ 成立，如果此时额外成立 $n=m$ 会导出 $n\notin n$，矛盾。如果此时额外成立 $n\in m$，则导出 $m\subseteq n\land n\subseteq m$，即 $m=n$，矛盾。
	- 证明至少有一成立。设 $S=\{n\mid n\in\mathbb N\land\forall m(m\in\mathbb N\to m\in n\lor m=n\lor n\in m)\}$。首先 $\varnothing\in S$，因为若 $m\neq\varnothing$，则 $\varnothing\in m$。
		- 若 $n\in S$，任取 $m\in\mathbb N$。由 $n\in S$，有 $m\in n\lor m=n\lor n\in m$。证 $n^+\in S$：
		- 任取 $m\in n$，则因为 $n\subseteq n^+$，所以 $m\in n^+$。
		- 任取 $m=n$，则 $m=n\in n^+$。
		- 任取 $n\in m$，则 $n^+\in m^+\to n^+\in m\lor n^+=m$。

自然数集的同构。任取一个 Peano 系统 $(M, F, e)$，则其与 $(\mathbb N, \sigma, \varnothing)$ 同构。

**$\mathbb N$ 上的递归定理**。设 $A$ 是集合，$F:A\to A$，$a\in A$，则存在唯一的函数 $h:\mathbb N\to A$ 使得 $h(0)=a$ 以及 $\forall n(n\in\mathbb N\to h(n^+)=F(h(n)))$。
### $\it 2.3$ 传递集合
**传递集**。设 $A$ 是集合，若 $\forall x(x\in A\to x\subseteq A)$，则称 $A$ 是传递集（transitive set）。
- 等价定义1：$\forall x(x\in A\to\forall y(y\in x\to y\in A))$。
- 等价定义2：$\bigcup A\subseteq A$。
- 等价定义3：$A\subseteq P(A)$。
- 人话。按照生成元的视角来理解传递集，假设 $A$ 中一开始只有 $X$，那么操作即为将 $X$ 中的所有元素丢到 $A$ 中，反复执行该操作，得到的 $A$ 即为传递集。而 $x\in A\to x\subseteq A$ 就是描述了这个“把元素丢到 $A$”的行为。

传递集的性质。
- $A$ 是传递集，当且仅当 $P(A)$ 是传递集。
	- 证明。必要性，若 $A$ 是传递集，则 $\forall x(x\in A\to x\subseteq A)$。任取 $B\in P(A)$，那么 $B\in P(A)\to B\subseteq A\to\forall x(x\in B\to x\in A\to x\subseteq A\to x\in P(A))\to B\subseteq P(A)$。
	- 充分性。若 $P(A)$ 是传递集，那么 $A\in P(A)$，则 $\forall x(x\in A\to x\in P(A)\to x\subseteq A)$。结论成立。
- 设 $A$ 是传递集，则 $\bigcup\limits(A^+)=A$。
	- 证明。$\bigcup\limits(A^+)=\left( \bigcup\limits A \right)\cup A$。由于 $\bigcup\limits A\subseteq A$，所以结果为 $A$。
- **任意自然数是传递集**。设 $n\in\mathbb N$，则 $n$ 是传递集。
	- 证明。由前文已证：若 $n\in\mathbb N$ 且 $m\in n$，则 $m\subseteq n$。这正是传递集的定义。
- **$\mathbb N$ 是传递集**。任取 $n\in\mathbb N$，则 $n\subseteq\mathbb N$。
	- 证明。归纳，首先 $\varnothing\subseteq\mathbb N$ 成立。设 $n\in\mathbb N\land n\subseteq\mathbb N$，则 $n^+=n\cup\{n\}$，其中 $n\subseteq\mathbb N$ 且 $n\in\mathbb N$（即 $\{n\}\subseteq\mathbb N$），所以 $n^+\subseteq\mathbb N$ 归纳成立。
- **传递集与归纳集的关系**。若 $A$ 是传递集，则 $A\cup\{A\}$ 也是传递集。
	- 证明。任取 $x\in A\cup\{A\}$。若 $x\in A$，则由 $A$ 传递，$x\subseteq A\subseteq A\cup\{A\}$。若 $x=A$，则 $x=A\subseteq A\cup\{A\}$。因此 $A\cup\{A\}$ 传递。
### $\it 2.4$ 自然数的运算法则
**加法**。定义 $A_m:\mathbb N\to\mathbb N$，表示 $A_m(0)=m$，以及 $A_m(n^+)=[A_m(n)]^+$（根据递归定理，存在唯一这样的函数）。则令 $+:\mathbb N^2\to\mathbb N$ 满足 $+(m, n):=A_m(n)$ 记作 $m+n$。
- 定理。$m+0=m$。以及 $m+n^+=(m+n)^+$。

**乘法**。定义 $M_m:\mathbb N\to\mathbb N$，表示 $M_m(0)=0$ 以及 $M_m(n^+)=M_m(n)+m$。令 $\cdot:\mathbb N^2\to\mathbb N$ 使得 $\cdot(m, n):=M_m(n)$。记作 $m\cdot n$ 或者 $mn$。

**指数**。定义 $E_m:\mathbb N\to\mathbb N$，表示 $E_m(0)=1$ 以及 $E_m(n^+)=E_m(n)\cdot m$。令 $\mathrm{exp}:\mathbb N^2\to\mathbb N$ 使得 $\mathrm{exp}(m, n):=E_m(n)$。记作 $m^n$。

加法交换律、加法结合律、乘法交换律、乘法结合律。
### $\it 2.5$ $\mathbb N$ 的序关系
**序关系**。定义 $n<m\iff n\in m$，$n\le m\iff n\in m\lor n=m$。
- 由三歧性，任取 $n,m\in\mathbb N$，则 $n<m, n=m, m<n$ 有且仅有一成立。
- 序关系与加法相容。若 $n,m,k\in\mathbb N$ 且 $n<m$，则 $n+k<m+k$。
	- 证明。对 $k$ 归纳。$k=0$ 时显然。设 $n+k<m+k$，由 $n+k\in m+k$，则 $(n+k)^+\in(m+k)^+$，即 $n+k^+<m+k^+$。
- 序关系与乘法相容。若 $n,m,k\in\mathbb N$ 且 $n<m$，则 $nk<mk$（$k\neq 0$ 时）。
- **良序性**。$\mathbb N$ 的任意非空子集都有最小元。
	- 证明。设 $A\subseteq\mathbb N$ 且 $A\neq\varnothing$。假设 $A$ 无最小元，设 $S=\{n\in\mathbb N\mid\forall m(m\in n\to m\notin A)\}$。首先 $\varnothing\in S$。若 $n\in S$，假设 $n^+\notin S$，则存在 $m\in n^+$ 且 $m\in A$。若 $m\in n$，与 $n\in S$ 矛盾；若 $m=n$，则 $n\in A$ 且 $n$ 的所有元素都不在 $A$ 中，即 $n$ 是 $A$ 的最小元，矛盾。因此 $n^+\in S$，归纳得 $S=\mathbb N$，从而 $A=\varnothing$，矛盾。
