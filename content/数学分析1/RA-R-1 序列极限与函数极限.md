---
publish: true
---
#数分1 
# 数学分析1 笔记 (1) 序列极限与函数极限
有了集合论、实数构造等一系列准备工作，我们得以进入实分析的第一个定义：极限。并由此介绍**实数系中的重要基本定理**，其均为完备公理的直接推论。

对应 101 教材：Ch3, 4
## $\mathrm{III}$ 序列极限
### $\it 3.0$ Intro
离散数学是**可列的**，因此列表不但是性质，也是研究问题的重要方法：将研究的元素按照列表列出来，如 $x_1, x_2, \cdots$，这衍生出列表、数据结构、自动机等。但在实分析的主舞台——$\mathbb R$，不存在这样的列表结构（不可列），我们只能关注元素之间的**结构关系**（例如**序关系**）。为此有了基于逻辑严格定义的**极限**。

**符号约定**。
- 使用 $U(x, \delta)$ 表示开区间（邻域）$(x-\delta, x+\delta)$。
### $\it 3.1$ 序列的极限
**序列极限**。对任意一列数 $\{a_n\}$，若存在一个常数 $A\in\mathbb R$ 使得其对任意 $\varepsilon>0$ 都存在 $N\ge 1$ 使得 $\forall n\ge N$ 满足 $|a_n-A|<\varepsilon$。则称 $A$ 为数列 $\{a_n\}$ 的极限。
- 记作 $\lim\limits_{n\to\infty}a_n=A$，称数列 $\{a_n\}$ 收敛于 $A$，其极限存在。
- 人话：对任意小的一个正误差 $\varepsilon$，序列 $\{a_n\}$ 都存在一条“尾巴”完全落在 $U(A, \varepsilon)$ 内。

可以写作逻辑符号：
$$
\forall\varepsilon(\varepsilon\in\mathbb R^+\to \exists N(N\in\mathbb N^+\land \forall n(n\ge N\to |a_n-A|<\varepsilon)))
$$
**分析**。正如上文所述，在被禁止使用列表等有序结构研究问题时，极限是 $\mathbb R$ 上最重要的结构（之一）。其“结构”描述了实数轴上的“逼近”“相邻”等关系，且**与实际数值大小无关**。
- 例如，实数轴上的 $a<b$ 就描述了“严格小于”的关系，其意味着存在一个邻域 $U(a, \delta)$ 使得 $\forall x(x\in U(a, \delta)\to x<b)$，无论 $|b-a|$ 是 $10^{-1}$，$10^{-10}$ 还是 $10^{-n}$。
- 同时应当指出，按照“有效性”（工程）思路分析，重点在于 $\varepsilon\to0$ 以及 $n\to\infty$ 的时候仍然成立该式，因此 $\{a_n\}$ 的**尾部性质**才是本质。
	- 在实分析的早期阶段（Newton-Leibniz）“无穷小”是一个只存在于直觉中的模糊概念（尽管有效），现在看到的这个看起来很绕的极限定义是 Weierstrass 给出的。因此接受极限的定义是极为重要的。

**极限的基本性质**。
- 若 $\{a_n\}$ 收敛，那么 $a_n$ 有界且极限唯一。
	- 证明。存在 $\varepsilon>0$ 与 $N$ 使得 $n\ge N\to|a_n-A|<\varepsilon$，因此对于 $n<N$ 有 $|a_n|\le\sup\limits_{1\le i<N}|a_i|$，对 $n\ge N$ 有 $|a_n|<|A|+\varepsilon$，于是 $M=\max\left(\sup\limits_{1\le i<N}|a_i|,  |A|+\varepsilon\right)$ 是数列 $\{a_n\}$ 的一个界。
	- 证明。若存在 $A_1, A_2$ 同时为数列的极限，取 $\varepsilon=\dfrac{|A_1-A_2|}{2}$ 即可导出矛盾。
- **夹逼准则**。若数列 $\{x_n\}, \{y_n\}, \{z_n\}$ 满足 $\forall n\ge N$ 有 $y_n\le x_n\le z_n$ 且 $\lim\limits_{n\to\infty}y_n=\lim\limits_{n\to\infty}z_n=A$，则有 $\lim\limits_{n\to\infty}x_n=A$。
	- 证明。任取一个 $\varepsilon>0$，存在 $N$ 使得 $\forall n(n\ge N\to |y_n-A|<\varepsilon\land|z_n-A|<\varepsilon)$（取两个数列 $N$ 的 max 即可）。于是 $A-\varepsilon<y_n\le x_n\le z_n<A+\varepsilon$，所以相同条件下 $|x_n-A|<\varepsilon$。
- **四则运算**。设 $\lim\limits_{n\to\infty}a_n=A$，$\lim\limits_{n\to\infty}b_n=B$，那么：
	1. $\lim\limits_{n\to\infty}\lambda a_n + \mu b_n=\lambda A + \mu B$。
	2. $\lim\limits_{n\to\infty}a_nb_n=AB$。
	3. $\lim\limits_{n\to\infty}\dfrac{a_n}{b_n}=\dfrac AB$（$B\neq 0$）。
	- 证明加减是容易的。乘法主要需要用到两个序列的有界性进行放缩。
	- 注意：有限个序列的和的极限等于其极限的和，但是当项数为无穷的时候不满足，如 $\lim\limits_{n\to\infty}\sum\limits_{i=1}^n\dfrac{i}{n^2}$。

**广义实数系与无穷量**。仿照极限为有限值的情形，定义 $\lim\limits_{n\to\infty}a_n=\infty$ 的含义：任意取 $M>0$，存在一个 $N$ 使得 $n\ge N\to(a_n>M)$。同理定义 $\lim\limits_{n\to\infty}a_n=-\infty$ 的含义。
### $\it 3.2$ Stolz 公式与无穷小量的级别
**无穷小量**。若 $\lim\limits_{n\to\infty}a_n=0$，则称 $\{a_n\}$ 为无穷小量。
- 无穷小量的比较。若对于实数列 $\{a_n\}$，$\{b_n\}$（存在 $N$ 使得 $n\ge N\to b_n\neq 0$），若 $\lim\limits_{n\to\infty}\dfrac{a_n}{b_n}=0$，则记作 $a_n=o(b_n)$（$n\to\infty$），称 $a_n$ 是 $b_n$ 的**高阶无穷小量**。
- 有界量。若 $\dfrac{a_n}{b_n}$ 对任意 $n$ 有界，则记作 $a_n=O(b_n)$。即：存在 $M$ 使得 $|a_n|\le M|b_n|$。
- 等价。若 $\lim\limits_{n\to\infty}\dfrac{a_n}{b_n}=1$，则称两者等价，记作 $a_n\sim b_n$（$n\to\infty$）。

**Stolz 公式**。情形一。设 $\{x_n\}$ 和 $\{y_n\}$ 是两个实数列，若 $y_n$ 严格单增，且 $\lim\limits_{n\to\infty}y_n=\infty$，且 $\lim\limits_{n\to\infty}\dfrac{x_{n+1}-x_n}{y_{n+1}-y_n}=L$，则 $\lim\limits_{n\to\infty}\dfrac{x_n}{y_n}=L$。情形二。设 $\{x_n\}$ 和 $\{y_n\}$ 是两个实数列，若 $y_n$ 严格单减，且 $\lim\limits_{n\to\infty}x_n=\lim\limits_{n\to\infty}y_n=0$，且 $\lim\limits_{n\to\infty}\dfrac{x_n-x_{n+1}}{y_n-y_{n+1}}=L$，则 $\lim\limits_{n\to\infty}\dfrac{x_n}{y_n}=L$。
- 注：这里的 $L$ 可以为有限值或正负无穷。证明见习题 3.2 Q7。Stolz 定理类似离散版本的洛必达法则，其建立了（比值）序列与其差分序列极限的对应关系。
- 几个重要的推论：
	- $\lim\limits_{n\to\infty}x_{n+1}-x_n=\ell\Rightarrow\lim\limits_{n\to\infty}\dfrac{x_n}{n}=\ell$（或者写作 $\lim\limits_{n\to\infty}x_n=\ell\Rightarrow\lim\limits_{n\to\infty}\dfrac{\sum\limits_{k=1}^nx_k}{n}=\ell$）。
### $\it 3.3$ 实数系的各大基本定理
以 Dedekind 构造为基准，介绍一系列实数系所满足的最基本定理，其**都与完备公理等价**。
#### $\it 3.3.1$ 确界存在原理
任意取 $A\subseteq\mathbb R$，若 $A$ 存在上界，则 $\sup A\in\mathbb R$。同理，若 $A$ 存在下界，则 $\inf A\in\mathbb R$。
#### $\it 3.3.2$ 单调收敛定理
单调收敛定理是确界存在原理的的直接推论，其表述为：若 $\{a_n\}$ 是单调递增的实数列，且 $a_n$ 有界，则 $\lim\limits_{n\to\infty}a_n$ 存在。（单调递减同理）
- 证明。因为 $a_n$ 有界，故取其上确界 $M\in\mathbb R$，则根据上确界的定义，$\forall\varepsilon>0$ 有 $\exists a_t\in\{a_n\}$ 使得 $a_t>M-\varepsilon$。由单调性得出 $n\ge t\to|a_n-M|<\varepsilon$。这正是极限的定义。
#### $\it 3.3.3$ 闭区间套定理
Cauchy-Cantor 闭区间套定理。设 $\{[a_n, b_n]\}$ 是一系列闭区间，满足 $\forall n([a_{n+1}, b_{n+1}]\subseteq[a_n, b_n])$ 且 $\lim\limits_{n\to\infty}b_n-a_n=0$，则 $\bigcap\limits_{i=1}^\infty[a_i, b_i]=\{x_0\}$（且 $x_0=\lim\limits_{n\to\infty}a_n=\lim\limits_{n\to\infty}b_n$）。
- 证明。$\forall n(a_n\le a_{n+1}<b_{n+1}\le b_n)$，因此 $a_n$ 单调递增，$b_n$ 单调递减，两者都有界，因此极限存在。故 $\lim\limits_{n\to\infty}b_n=\lim\limits_{n\to\infty}a_n+\lim\limits_{n\to\infty}(b_n-a_n)=\lim\limits_{n\to\infty}a_n=x_0$。由单调性，$\forall i\forall j(a_i\le x_0\le b_j)$，因此 $x_0\in\bigcap\limits_{i=1}^\infty[a_i, b_i]$。另一方面，若 $\eta$ 也是该交集的元素，则 $a_n\le\eta\le b_n$ 夹逼得到 $\eta=x_0$。这说明这些区间的交集有且仅有一个公共点。
#### $\it 3.3.4$ 致密性定理与聚点原则
**$\mathbb R$ 中的一些拓扑概念**。
- **聚点**。设 $A\subseteq\mathbb R$。若对任意 $\delta>0$，$U(x_0, \delta)\cap A$ 为无限集，则称 $x_0$ 为 $A$ 的**聚点**。
- 开集。对 $A\subseteq\mathbb R$，若存在 $\delta>0$ 使得 $U(x_0, \delta)\subseteq A$，则称 $x_0$ 为 $A$ 的**内点**。若 $A$ 中的所有点都是内点，则称 $A$ 为开集。
- 闭集。若 $A$ 的所有聚点都属于 $A$，则称 $A$ 是闭集。

聚点原则。$\mathbb R$ 中的任意有界、无限集均存在聚点。
- 证明。设 $A\in\mathbb R$ 是有界、无限集。则存在 $A\subseteq[a_1, b_1]$。因为 $A$ 是无限集，因此 $A\cap\left[ a_1, \dfrac{a_1+b_1}2 \right]$ 和 $A\cap\left[ \dfrac{a_1+b_1}{2}, a_2\right]$ 至少有一个是无限集。若前者是无限集，则令 $[a_2, b_2]$ 为 $\left[ a_1, \dfrac{a_1+b_1}{2} \right]$，否则令其为 $\left[ \dfrac{a_1+b_1}{2}, a_2 \right]$，无论如何 $A\cap[a_2, b_2]$ 为无限集。类似得到 $[a_n, b_n]$。
- 对于区间长度，有 $b_n-a_n=\dfrac12(b_{n-1}-a_{n-1})$，因此 $\lim\limits_{n\to\infty}b_n-a_n=0$，则 $\{[a_n, b_n]\}$ 为闭区间套，其有唯一的公共点 $\xi$。
- 任取 $\delta>0$，考虑 $U(\xi, \delta)$，一定存在 $\xi-\delta<a_m<\xi<b_m<\xi+\delta$。由于 $A\cap[a_m, b_m]$ 为无限集，而 $[a_m, b_m]\subseteq U(\xi, \delta)$，所以 $A\cap U(\xi, \delta)$ 为无限集。证毕。
- **分析**。这类“聚点模型”以及在有限区间上“二分”的做法可以考虑记一下。

**子列与致密性定理**。先引入子列的概念，直观来说就是按顺序提取 $\{a_n\}$ 的一个子序列。则有如下致密性定理：$\mathbb R$ 中的任何有界数列存在一个子列，使得该子列收敛。
- 证明。设 $A:=\{a_n\mid n\ge 1\}$。若 $A$ 为有限集，则一定存在某个出现了无穷次的元素，将其提取出来即为一个收敛到其本身的常数列。
- 否则 $A$ 为有界、无限集。设 $\xi$ 是 $A$ 的一个聚点，则任取 $A\cap U(\xi, \delta)$ 为无限集。取 $n_1$ 使得 $a_{n_1}\in A\cap U(\xi, 1)$，取 $n_2>n_1$ 使得 $a_{n_2}\in A\cap U\left( \xi, \dfrac12 \right)$。即取 $n_k>n_{k-1}$ 使得 $a_{n_k}\in A\cap U\left( \xi, \dfrac1k \right)$。则 $\{a_{n_k}\}$ 是原序列的一个收敛子列。
#### $\it 3.3.5$ Cauchy 收敛准则
Cauchy 收敛准则是与数列极限等价的定义，其优点是无需事先知道数列的极限。其表述为：称 $\{x_n\}$ 为 **Cauchy 列**，若对 $\forall \varepsilon>0$ 存在 $N\ge 1$ 使得 $m, n\ge N\to(|x_n-x_m|<\varepsilon)$。
- 等价表示：$\lim\limits_{N\to\infty}\sup\limits_{n, m\ge N}|x_n-x_m|=0$。

**Cauchy 收敛准则**。数列收敛的充要条件是其为 Cauchy 列。
- 证明。必要性。$\lim\limits_{n\to\infty}a_n=L$，则 $\forall\varepsilon$ 都存在 $N$ 使得 $n\ge N\to(|a_n-L|<\varepsilon)$。于是 $n, m\ge N\to(|a_n-a_m|<2\varepsilon)$。满足 Cauchy 列的定理。
- 充分性。若 $\{a_n\}$ 是 Cauchy 列，则 $\forall\varepsilon$ 存在 $N$ 使得 $n, m\ge N\to(|a_n-a_m|<\varepsilon)$，所以 $|a_n-a_N|<\varepsilon$。故 $\{a_n\}$ 是有界数列，则存在收敛子列 $\{a_{n_k}\}$，设其极限为 $\xi$。则 $\forall\varepsilon$ 存在 $K$（取 $K$ 足够大）使得 $k\ge K\to|a_{n_k}-\xi|<\varepsilon$。而数列本身是 Cauchy 列，所以令 $N_1=\max(N, K)$，任取 $m\ge N_1$ 有 $|a_m-a_{n_K}|<\varepsilon$。于是 $|a_m-\xi|<2\varepsilon$。得证。
#### $\it 3.3.6$ 有限覆盖定理
开覆盖。称集族 $\mathscr F$ 是 $A\subseteq\mathbb R$ 的一个开覆盖，若 $\forall B\in\mathscr F$ 为开集，且 $A\subseteq\bigcup\limits_{B\in\mathscr F}B$。
**Heine-Borel 有限覆盖定理**。设 $A\subseteq\mathbb R$ 是有界闭集，则 $A$ 的任何开覆盖 $\mathscr F$ 均存在**有限子覆盖**（即：$\mathscr A\subseteq\mathscr F$，$|\mathscr A|<\infty$ 且 $A\subseteq\bigcup\limits_{B\in\mathscr A}B$）。
- 证明。暂时跳过。
#### $\it 3.3.7$ 小结
可以拿 $\mathbb Q$ 来对比理解这些 $\mathbb R$ 的基本定理。若将 $\mathbb R$ 换成 $\mathbb Q$，则以上定理均不成立。这些定理都是完备公理的等价表述，即：任意 $\mathbb R$ 中的无穷行为（如极限），其结果一定仍然存在于 $\mathbb R$ 中。通常在数学分析的学习中，采用如下顺序证明（仅参考）：
$$
\begin{aligned} &完备公理 \Rightarrow 确界原理 \Rightarrow 单调收敛 \\ \Rightarrow{} &闭区间套 \Rightarrow 聚点原则 \Rightarrow \mathrm{Cauchy}\  准则  \end{aligned}
$$

### $\it 3.4$ 自然常数
定理。$x_n=\left( 1+\dfrac1n \right)^n$ 单调递增且有界，进而收敛。构造 $x_n=\left(1+\dfrac1n\right)^n, y_n=\left(1+\dfrac1n\right)^{n+1}$。
- 证明 $x_n$ 单增：$x_n=\left(1+\dfrac1n\right)^n\cdot 1<\left(\dfrac{n\left(1+\dfrac1n\right)+1}{n+1}\right)^{n+1}=\left(1+\dfrac1{n+1}\right)^{n+1}=x_{n+1}$。
- 证明 $y_n$ 单减：$\begin{aligned}y_n&=\left(1+\dfrac1{n}\right)^{n+1}=\dfrac1{\left(\dfrac n{n+1}\right)^{n+1}\cdot 1}\\ &>\dfrac1{\left(\dfrac{\dfrac n{n+1}\left(n+1\right)+1}{n+2}\right)^{n+2}}=\left(1+\dfrac1{n+1}\right)^{n+2}=y_{n+1}\end{aligned}$。且有 $y_n=\left(1+\dfrac1n\right)x_n>x_n$。因此 $x_n, y_n$ 同时收敛。同时有 $1=\lim\limits_{n\to\infty}\dfrac{y_n}{x_n}=\dfrac{\lim\limits_{n\to\infty}y_n}{\lim\limits_{n\to\infty}x_n}$，因此两极限相等。
- **自然常数**。定义 $e:=\lim\limits_{n\to\infty}\left( 1+\dfrac1n \right)^n$。
- 得到 $\left(1+\dfrac1n\right)^n<e<\left(1+\dfrac1n\right)^{n+1}$。也就是 $\boldsymbol{\dfrac1{n+1}<\ln\left(1+\dfrac1n\right)<\dfrac1n}$。
- $\left(1+\dfrac1n\right)^n$ 与 $\sum\limits_{k=0}^n\dfrac1{k!}$ 的关系：
	- 利用二项式展开，得到 $\left(1+\dfrac1n\right)^n=\sum\limits_{k=0}^n\dfrac1{k!}\prod\limits_{i=1}^{k-1}\left(1-\dfrac in\right)<\sum\limits_{k=0}^n\dfrac1{k!}$。
	- 取任意前 $m$ 项，则 $1+1+\dfrac1{2!}\left(1-\dfrac1n\right)+\cdots+\dfrac1{m!}\prod\limits_{i=1}^{m-1}\left(1-\dfrac in\right)\le\left(1+\dfrac1n\right)^n\le\sum\limits_{k=0}^\infty\dfrac1{k!}$。
	- 令 $n\to\infty$，则 $1+1+\dfrac1{2!}+\cdots+\dfrac1{m!}\le e\le\sum\limits_{k=0}^\infty\dfrac1{k!}$。
	- 再令 $m\to\infty$，则 $e=\sum\limits_{k=1}^\infty\dfrac1{k!}$。
### $\it 3.5$ 上下极限
**上极限**。定义 $\overline{\lim}\limits_{n\to\infty}a_n:=\lim\limits_{n\to\infty}\sup\limits_{k\ge n}a_k$。（类似定义下极限：$\underline{\lim}\limits_{n\to\infty}:=\lim\limits_{n\to\infty}\inf\limits_{k\ge n}a_n$。
- 等价定义：$\overline{\lim}\limits_{n\to\infty}a_n$ 即为 $\{a_n\}$ 所有收敛子列的极限值的上确界。
	- 证明。首先任意收敛子列的极限值一定小于等于 $\overline{\lim}\limits_{n\to\infty}a_n$（因为 $a_{n_k}\le\sup_{i\ge n_k}a_i$）。然后对任意的 $\varepsilon>0$，存在 $N$ 使得 $n\ge N\to\sup\limits_{k\ge n}a_k-L<\varepsilon$。也就是存在 $a_{t_n}$ 使得 $a_{t_n}>L-\varepsilon$。取这样的 $t_n$ 作为下标（下标递增）可以得到一个极限值为 $L$ 的序列。

**Stolz 的上下极限版本**。设 $\{y_n\}$ 严格单增且 $\lim\limits_{n\to\infty}y_n=\infty$，则有
$$
\underline{\lim}\limits_{n\to\infty}\dfrac{x_{n+1}-x_n}{y_{n+1}-y_n}\le\underline{\lim}\limits_{n\to\infty}\dfrac{x_n}{y_n}\le\overline{\lim}\limits_{n\to\infty}\dfrac{x_n}{y_n}\le\overline{\lim}\limits_{n\to\infty}\dfrac{x_{n+1}-x_n}{y_{n+1}-y_n}
$$
## $\mathrm{IV}$ 函数极限
函数极限是与数列极限几乎一致的概念，但是在函数极限中，使用邻域 $U(x_0, \delta)$ 来描述“相近”结构，而非数列中可列的 $n\to\infty$。

符号约定：去心邻域 $U_0(a, \delta):=(a-\delta, a+\delta)-\{a\}$。
### $\it 4.1$ 函数极限的定义
**函数的极限**。若 $f(x)$ 在 $a$ 的去心邻域 $U_0(a, \delta)$ 有定义，且对任意 $\varepsilon>0$ 存在 $\delta>0$ 使得 $x\in U_0(a, \delta)\to|f(x)-A|<\varepsilon$，则称 $\lim\limits_{x\to a}f(x)=A$。
- 右侧极限。任意 $\varepsilon>0$ 存在 $\delta>0$ 使得 $x\in(a, a+\delta)\to|f(x)-A|<\varepsilon$。（同理有左侧极限）
- 四则运算性质。
	- （加减与数乘）$\lim\limits_{x\to a}\lambda f(x)+\mu g(x)=\lambda\lim\limits_{x\to a}f(x)+\mu\lim\limits_{x\to a}g(x)$。
	- （乘法）$\lim\limits_{x\to a}f(x)g(x)=\lim\limits_{x\to a}f(x)\cdot\lim\limits_{x\to a}g(x)$。
	- （除法）$\lim\limits_{x\to a}\dfrac{f(x)}{g(x)}=\dfrac{\lim\limits_{x\to a}f(x)}{\lim\limits_{x\to a}g(x)}$（当 $\lim\limits_{x\to a}g(x)\neq 0$）。

**Heine 定理**。设函数 $f(x)$ 在 $U_0(a, \delta)$ 有定义，则 $\lim\limits_{x\to a}f(x)=A$ 当且仅当 $U$ 中任意趋向 $a$（但不取 $a$）的序列 $\{x_n\}$，有 $\lim\limits_{n\to\infty}f(x_n)=A$。
- 证明。必要性。任取 $\varepsilon>0$，则存在邻域 $U_0(a, \delta)$ 使得 $x\in U_0\to|f(x)-A|<\varepsilon$，则对任意 $\lim\limits_{n\to\infty}x_n=a$ 的序列，存在 $N$ 使得 $n\ge N\to|x_n-a|<\delta$。即：$n\ge N\to|f(x_n)-A|<\varepsilon$。也就是 $\lim\limits_{n\to\infty}f(x_n)=A$。
- 充分性。假设 $\lim\limits_{x\to a}f(x)\neq A$，则存在 $\varepsilon_0>0$ 使得任意 $U_0(a, \delta)$ 都有 $\exists x(x\in U_0\land|f(x)-A|\ge\varepsilon_0)$。令 $x_n\in U_0\left( a, \dfrac1n \right)$ 而 $|f(x_n)-A|\ge\varepsilon_0$，则 $\lim\limits_{n\to\infty}x_n=a$ 而 $\lim\limits_{n\to\infty}f(x_n)\neq A$，矛盾。
- 注：在必要性中，若任意趋向 $a$ 的点列的函数值都收敛，则不同的序列一定收敛到同一值。否则假设有 $x_n$ 和 $y_n$ 使得 $\lim\limits_{n\to\infty}f(x_n)$ 和 $\lim\limits_{n\to\infty}f(y_n)$ 不同，则构造一个新的数列交替取 $x_n$ 和 $y_n$ 的项，其函数值振荡而不收敛，矛盾。
- 注：Heine 定理的逆否：若需要证明 $f(x)$ 不收敛，则只需要找到两个不同的序列分别收敛到不同的值。
### $\it 4.2$ 实数基本定理的函数版本
**Cauchy 收敛准则**。设 $f(x)$ 在 $a$ 的某个去心邻域有定义，则 $\lim\limits_{x\to a}f(x)$ 存在当且仅当：$\forall\varepsilon>0$，存在 $U_0(a, \delta)$ 使得 $x, y\in U_0\to|f(x)-f(y)|<\varepsilon$。
- 证明。必要性显然。充分性。任取趋向 $a$ 的点列 $\{x_n\}$，则 $\{f(x_n)\}$ 为 Cauchy 列，故其收敛。由 Heine 定理，$\lim\limits_{x\to a}f(x)$ 收敛。

**夹逼准则**。若在 $U_0(a, \delta)$ 中，有 $g(x)\le f(x)\le h(x)$ 且 $A=\lim\limits_{x\to a}g(x)=\lim\limits_{x\to a}h(x)$，那么 $A=\lim\limits_{x\to a}f(x)$。

**单调收敛原理**。若 $f(x)$ 在 $(a, a+\delta)$ 中单调且有界，则 $\lim\limits_{x\to a^+}f(x)$ 存在。

**致密性定理**。设 $f(x)$ 在 $(a, a+\delta)$ 有界，则存在 $(a, a+\delta)$ 中趋于 $a$ 的序列 $\{x_n\}$ 使得 $\{f(x_n)\}$ 收敛。
- 注：对该序列收敛到哪里没有任何要求，例如完全不一定是 $\lim\limits_{x\to a}f(x)$。

定理。设 $f(x)$ 在 $(a, a+\delta)$ **有界**，给定 $A$。若对于任意 $(a, a+\delta)$ 内的趋于 $a$ 的点列 $\{x_n\}$，只要 $\{f(x_n)\}$ 收敛，就有 $\lim\limits_{n\to\infty}f(x_n)=A$，则 $\lim\limits_{x\to a^+}f(x)=A$。
- 证明。假设 $\lim\limits_{x\to a^+}f(x)\neq A$，则存在 $\varepsilon_0>0$ 使得任意 $(a, a+\delta)$ 里面都存在 $x\in(a, a+\delta)\land|f(x)-A|\ge\varepsilon_0$。那么对 $\left( a, a+\dfrac1n \right)$ 取满足这样性质的 $x_n$，由于 $f(x)$ 有界，所以 $\{f(x_n)\}$ 有界，因此存在收敛子列（致密性定理）。不妨设其自身收敛，则根据假设有 $\lim\limits_{n\to\infty}f(x_n)=A$，这与 $|f(x_n)-A|\ge\varepsilon_0$ 矛盾，假设不成立。
### $\it 4.3$ 若干重要极限
$\lim\limits_{n\to\infty}\left( 1+\dfrac1n \right)^n$ 的连续化。一定成立 $\left( 1+\dfrac1{[x]+1} \right)^{[x]}<\left( 1+\dfrac1x \right)^x<\left( 1+\dfrac1{[x]} \right)^{[x]+1}$。其中前后夹逼收敛到 $e$。从而有 $\lim\limits_{x\to\infty}\left( 1+\dfrac1x \right)^x=e$。
- 等价于：$\lim\limits_{x\to 0}(1+x)^{\frac1x}=e$。
- 等价于：$\lim\limits_{x\to 0}\dfrac{\ln(1+x)}{x}=1$。

$\lim\limits_{x\to 0}\dfrac{\sin x}{x}=1$。目前按照几何的定义证明。
### $\it N$ 若干 $\mathbb R$ 的拓扑概念
设 $A\subseteq\mathbb R$。
- 内点。对 $x$ 若存在 $\delta>0$ 使得 $U(x, \delta)\subset A$，则称 $x$ 是 $A$ 的内点。
	- $A$ 的全体内点被称为 $A$ 的**内部**，记作 $A^\circ$。
- 外点。对 $x$ 若存在 $\delta>0$ 使得 $U(x, \delta)\cap A=\varnothing$，则称 $x$ 是 $A$ 的外点。
- 边界。若 $x$ 既不是内点，也不是外点，那么 $x$ 是 $A$ 的边界点，边界点的全体称为 $A$ 的边界，记作 $\partial A$。
- 聚点、导集。若任意 $\delta>0$，均有 $A\cap U(x, \delta)$ 是无限集，则称 $x$ 是 $A$ 的聚点。$A$ 的全体聚点成为 $A$ 的导集，记作 $A'$。
	- 定理：若对任意 $\delta>0$，$U(x, \delta)$ 内均存在 $A$ 中异于 $x$ 的点，则 $x$ 是 $A$ 的聚点。（证明：任取 $\delta_1>0$，存在 $x_1$，取 $\delta_2=|x-x_1|$ 得到 $x_2$ 的存在，以此类推）
- 孤立点。对 $x$ 若存在 $\delta>0$ 使得 $A\cap U(x, \delta)=\{x\}$，则称 $x$ 是 $A$ 的孤立点。
- 开集。若 $A=A^\circ$，则称 $A$ 是开集。
- 闭集。若 $A$ 包含其所有聚点（$A'\subseteq A$），则称 $A$ 是闭集。
	- 闭集的等价定义：$\partial A\subseteq A$、$\mathbb R-A$ 是开集，以及“$A$ 中任意收敛点列的极限均属于 $A$”。
	- 第三条的证明：必要性，若 $A$ 是闭集，则任取 $A$ 内部收敛点列 $\lim\limits_{n\to\infty}x_n=L$，那么 $\{x_n\}\cap U(L, \varepsilon)$ 是无穷集（根据极限定义，存在 $N$ 使得 $\{x_k\mid k\ge N\}\subseteq U(L, \varepsilon)$），故 $L$ 是 $A$ 的聚点，故 $L\in A$。充分性，任取 $A$ 的一个聚点 $a$，那么 $A\cap U(a, \varepsilon)$ 是无穷集，故存在 $A$ 中的收敛点列 $\lim\limits_{n\to\infty}x_n=a$。因此根据假设，$a\in A$。
- 任意个开集的并是开集，任意个闭集的交是闭集。
- 任意有限个开集的交是开集，任意有限个闭集的并是闭集。
- 等价于：$\lim\limits_{x\to 0}(1+x)^{\frac1x}=e$。
- 等价于：$\lim\limits_{x\to 0}\dfrac{\ln(1+x)}{x}=1$。

$\lim\limits_{x\to 0}\dfrac{\sin x}{x}=1$。目前按照几何的定义证明。
