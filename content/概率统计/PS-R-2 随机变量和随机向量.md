---
publish: true
---

# PS-R-2 概率论 笔记 (2) 随机变量和随机向量
### $\it 0.1$ Intro
随机的数值对研究问题有很大的便利，因此我们将样本空间的主舞台变为 $\mathbb R$，这是研究随机数值的基本。同时，我们将引入线性代数观点，对随机向量进行基本的解析。
## $\mathrm I$ 随机变量的分布函数
### $\it 1.1$ 定义分布函数
**随机变量**。形式化地，定义一个映射 $X:\Omega\to\mathbb R$ 并用大写字母 $X, Y, Z$ 代表随机变量，用小写字母 $x, y, z$ 代表其取值。
- 若 $X$ 的取值可能为至多可列个，则称其为**离散随机变量**，否则其为**连续随机变量**。

**分布函数**。如何描述 $X$ 各处取值的可能性？我们只用定义 $F(x):=P(X\le x)$，称其为随机变量 $X$ 的**分布函数**，就可以知道所有形如 $\{X\in[a, b]\}$ 区间之类的事件有多少概率。其具有一些基本的性质：
- 非严格单调性：$x_1\le x_2\iff F(x_1)\le F(x_2)$。
	- 证明。$F(x_2)-F(x_1)=P(x_1<X\le x_2)\ge 0$。
- 有界性：$F(x)\in[0, 1]$，且 $\lim\limits_{x\to-\infty}F(x)=0$，$\lim\limits_{x\to\infty}F(x)=1$。
	- 证明。$F(x)=P(X\le x)\in[0, 1]$。对于分布函数在无穷处的极限（首先单调有界一定存在极限），我们将 $x\in\mathbb R$ 分解为可列个事件 $[i-1<X\le i]$，于是 $1=P\left( \bigcup\limits_{i=-\infty}^\infty[i-1<X\le i] \right)=\sum\limits_{i=-\infty}^\infty P(i-1<X\le i)$。即 $1=\lim\limits_{n\to\infty}F(n)-\lim\limits_{m\to-\infty}F(m)$。根据介值性，有 $\lim\limits_{x\to\infty}F(x)=1$，$\lim\limits_{x\to-\infty}F(x)=0$。
- **右侧连续性**。$\forall a$ 均有 $\lim\limits_{x\to a^+}F(x)=F(a)$。
	- 证明。任取趋于 $a$ 的单调点列 $x_1>x_2>\cdots>x_n>\cdots>a$（满足 $\lim\limits_{n\to\infty}x_n=a$）。则 $F(x_1)-F(a)=P\left( \bigcup\limits_{k=1}^\infty[x_{k+1}<X\le x_k] \right)=\sum\limits_{k=1}^\infty P(x_{k+1}<X\le x_k)=\sum\limits_{k=1}^\infty F(x_k)-F(x_{k+1})$。
	- 右侧即为 $F(x_1)-\lim\limits_{n\to\infty}F(x_n)$。所以 $F(a)=\lim\limits_{n\to\infty}F(x_n)$。
	- 这里点列 $\{x_n\}$ 是任取的，所以根据 Heine 定理得出 $F(a)=\lim\limits_{x\to a^+}F(x)$。
		- 实际上，Heine 定理要求任意趋于 $a$ 的数列都有这一条件，对于非单调数列改为上下极限即可得到同样的结论。
- 上述三条是判定函数 $F(x)$ 是否能成为分布函数的**充要条件**。实际上，我们有一些额外的性质：
- **左侧连续性分析**。如果记 $F(a^-):=\lim\limits_{x\to a^-}F(x)$，那么有 $P(X=a)=F(a)-F(a^-)$。
	- 证明。对任意的 $a<b$，设 $x_1, x_2, \cdots, x_n, \cdots$ 为任意一列满足 $x_{i+1}>x_i$ 且 $\lim\limits_{n\to\infty}x_n=a$ 的点列。则根据可列可加性，有 $P(x_1< X\le a)=P(X=a)+\sum\limits_{i=1}^\infty P(x_i<X\le x_{i+1})$。
	- 左侧：$P(x_1<X\le a)=F(a)-F(x_1)$。
	- 右侧：$\mathrm{rhs}=P(X=a)+\sum\limits_{i=1}^\infty[F(x_{i+1})-F(x_i)]=P(X=a)-F(x_1)+\lim\limits_{x\to a^-}F(x)$。
	- 对比两侧，因此 $P(X=a)=F(a)-F(a-0)$。
	- 这是一些奇妙的拓扑性质，对于可列个 $(a, b]$ 这样区间的并，其并区间右侧会变为开。

可以得出，$F(x)$ 不一定左连续。这是因为单点概率测度 $P(X=a)>0$ 导致了 $F(a)\neq F(a^-)$。对于这样的情形，我们可以分为离散与连续两类情形讨论。若 $X$ 为离散随机变量，则可以列出 $X$ 所有可能的取值 $x_1, x_2, \cdots$。其中每一种的概率为 $p_i=P(X=x_i)$。因此可以按照列表列出：
$$
\begin{pmatrix} x_1 & x_2 & x_3 & \cdots \\ p_1 & p_2 & p_3 & \cdots \end{pmatrix}
$$
而如果 $F(x)$ 在 $x=a$ 处连续，则 $P(X=a)=0$ 在测度上“测不出来”，但是我们知道 $a$ 处是有可能性取到的（即：**不可能事件的概率一定为 $0$，但概率为零不等于不可能发生**，也可能是“测不出来”）。如何描述此处的“可能性”到底有多大？我们需要其一阶导信息。
### $\it 1.2$ 概率密度函数
**概率密度函数**。设 $X$ 的分布函数为 $F(x)$，若存在一个非负、可积函数 $p:\mathbb R\to\mathbb R$ 满足 $F(x)=\begin{aligned} \int_{-\infty}^xp(t){\text d}t \end{aligned}$，则称 $p(x)$ 为 $X$ 的概率密度函数，或简称密度。此时 $X$ 为**连续随机变量**，$F(x)$ 为**连续分布函数**。其基本性质如下：
- 在 $F(x)$ 可导时，有 $F'(x)=p(x)$，可以写成 $p(x)=\lim\limits_{h\to 0}\dfrac{F(x+h)-F(x)}{h}$。因此立即推出 $p(x)\ge 0$ 以及 $\begin{aligned} \int_{-\infty}^\infty p(x){\text d}x=1 \end{aligned}$。
- 回到 $P(X=a)=0$“测不出来”的问题，我们用一阶微分来衡量其取值，因此不严谨地说，$P(X=a)$ 可以写作 $P(X=a)={\text d}F(a)=p(a){\text d}x$。
	- 有关 $F(x)$ 以及 $p(x)$ 是否符合 Riemann 积分等规范的问题，这里暂时不作讨论，在概率统计遇到的函数几乎可以默认其性质是良好的。

密度函数与零测集。对于连续随机变量 $X$，若改变其密度函数 $p(x)$ 在某一个零测集上的函数值，则其 Riemann 积分结果与原来的 $p(x)$ 并无不同，因此其在**概率意义**上是相同的密度函数。
- 这再一次体现了概率的本质是测度。此时前后的 $p(x)$“**几乎处处相等**”。
## $\mathrm{II}$ 数学期望和方差
通过分布函数以及密度函数，我们通过一个函数的信息完整描述了 $X$ 在各处的取值可能性（保留了完整的概率分布信息）。基于此我们可以得到 $X$ 的更多信息，比如期望和方差。
### $\it 2.1$ 数学期望
**期望**。对于离散随机变量 $X$，定义其期望为 $E(X):=\sum\limits_{i=1}^\infty x_ip(x_i)$。对于连续随机变量 $X$，定义其期望为 $E(X):=\begin{aligned} \int_{-\infty}^\infty xp(x){\text d}x \end{aligned}$。
- 期望存在的条件：需要满足级数或广义积分**绝对收敛**，即 $\sum\limits_{i=1}^\infty|x_i|p(x_i)<\infty$ 以及 $\begin{aligned} \int_{-\infty}^\infty |x|p(x){\text d}x \end{aligned}<\infty$。这是为了在实际观测 $X$ 的取值时，观测顺序不影响期望，否则不能保证在任意观测顺序下的均值 $\overline x$ 都收敛到期望。
- 如何理解期望。为什么能保证统计值或主观先验收敛到期望？这其实是一个“证明不了”的问题，实际上只要承认概率公理，那么就可以由数学严格推导出这一结论（大数定律）。换言之：只要承认概率公理，那么大量对 $X$ 观测的均值就会收敛到期望。

**期望的性质**。有一些常用的性质，需要熟练掌握。
- 设 $Y=g(X)$ 是有关 $X$ 的随机变量。则其期望 $E(Y)=\begin{aligned} \int_{-\infty}^\infty g(x)p(x){\text d}x \end{aligned}$。
- 若 $a$ 是常数，则 $E(a)=a$。且对任意的随机变量 $X$，有 $E(aX)=aE(X)$。
- **期望的线性可加**。对**任意**随机变量 $X, Y$，均有 $E(X+Y)=E(X)+E(Y)$。
	- 注意：这里对 $X$ 和 $Y$ 的独立性、相关性没有任何要求，其可以通过联合分布推导出。
### $\it 2.2$ 方差与标准差
**方差**。定义 $\mathrm{Var}(X):=E(X-E(X))^2$ 为随机变量 $X$ 的方差。定义 $\sigma=\sqrt{\mathrm{Var}(X)}$ 为标准差。
- 计算方式：直接写出 $\mathrm{Var}(X)=\sum\limits_{i=1}^\infty (x_i-E(X))^2p(x_i)$ 或者 $\mathrm{Var}(X)=\begin{aligned} \int_{-\infty}^\infty(x-E(X))^2p(x){\text d}x \end{aligned}$。
- 这同样要求这个级数或者广义积分**绝对收敛**。原因同上。
	- 因此，若方差存在，则期望一定存在，而反之则不一定。

**方差的性质**探讨。
- 首先是一个至关重要的算式：$\mathrm{Var}(X)=E(X^2)-E^2(X)$。
	- 证明。$\mathrm{Var}(X)=E((X-E(X)))^2=E(X^2-2XE(X)+E^2(X))$。即 $E(X^2)-2E(X)\cdot E(X)+E^2(X)=E(X^2)-E^2(X)$。这在计算中极为常用。
- 若 $a$ 是常数，则 $\mathrm{Var}(a)=0$。若 $a, b$ 是常数，则 $\mathrm{Var}(aX+b)=a^2\mathrm{Var}(X)$。
- 有关**为什么方差的设计是平方**。如果从实用的角度来说，平方是一个“优秀”的选项，例如在梯度下降中的可导性以及“平方惩罚”等说法众说纷纭。但是其理论依据可能是正态分布以及中心极限定理等（？）。
### $\it 2.3$ 基本概率不等式
有了期望和方差的定义，我们就可以引入一些最基本的概率不等式了，其在估计概率的收敛速度中非常有用。

**Markov 不等式**。设 $X$ 是**非负**随机变量，则对任意的 $a>0$ 有 $P(X\ge a)\le\dfrac{E(X)}{a}$。
- 证明并没有用到复杂的结论，此处假定其为连续随机变量（离散情形类似）。设 $p(x)$ 为其密度函数，则 $P(X\ge a)=\begin{aligned} \int_a^\infty p(x){\text d}x \end{aligned}$。而 $E(X)=\begin{aligned} \int_0^\infty xp(x){\text d}x \end{aligned}$。因此 $\dfrac1a\begin{aligned} \int_0^\infty xp(x){\text d}x \end{aligned}\ge\dfrac1a\begin{aligned} \int_a^\infty xp(x){\text d}x\ge \dfrac1a\int_a^\infty ap(x){\text d}x \end{aligned}=P(X\ge a)$。证毕。
- 课本上给出了另一种巧妙的证明。令随机变量 $Y_a:=\begin{cases} 0\quad X<a \\ a\quad X\ge a \end{cases}$。则 $0\le Y_a\le X$。所以 $aP(X\ge a)=E(Y_a)\le E(X)$。

**Chebyshev 不等式**。设 $X$ 为**任意**随机变量，其期望和方差都存在，则对任意 $\varepsilon>0$，有 $P(|X-E(X)|\ge\varepsilon)\le\dfrac{\mathrm{Var}(X)}{\varepsilon^2}$。证明依旧只用到了最基本的积分不等式（令 $a=E(X)$）：
$$
\begin{aligned} \dfrac1{\varepsilon^2}\mathrm{Var}(X)&=\dfrac1{\varepsilon^2}\int_{-\infty}^\infty(x-a)^2p(x){\text d}x \\ &\ge \int_{|x-a|\ge\varepsilon}\left( \dfrac{x-a}{\varepsilon} \right)^2p(x){\text d}x \\ &\ge \int_{|x-a|\ge\varepsilon}p(x){\text d}x \\ &= P(|X-a|\ge\varepsilon) \end{aligned}
$$
Markov 不等式与 Chebyshev 不等式为估计概率的“收敛”速度提供了重要参考，其主要在于说明，趋于无穷处的概率会被压制。根据其推导过程我们发现，其只用到了概率公理以及基本的积分不等式，因此其可以视作概率公理的**直接推论**。也说明了无穷处的概率函数一定趋于零。
- 例如，证明 $\mathrm{Var}(X)=0$ 的充要条件是 $P(X=a)=1$。可以直接根据 Chebyshev 不等式得到 $P(|X-E(X)|\ge\varepsilon)=0$ 对任意 $\varepsilon$ 成立。然后由一些可列可加分析即可得到。
## $\mathrm{III}$ 随机向量与联合分布
进入多元随机变量，我们自然要引入线性代数对高维空间进行规范化的处理。
### $\it 3.1$ 定义随机向量
**随机向量**。设 $X_1(\omega), X_2(\omega), \cdots, X_n(\omega)$ 是定义在同一个样本空间 $\Omega$ 上的 $n$ 个随机变量，则称向量 $\mathbf X(\omega)=(X_1(\omega_1), X_2(\omega_2), \cdots, X_n(\omega_n))^T$ 为 $n$ 维**随机向量**。大多数情况中，我们直接讨论 $X_i\in\mathbb R$。
- 符号：约定 $\mathbf x=(x_1, x_2, \cdots, x_n)^T$ 为标量向量，$\mathbf X$ 为随机向量。

**联合分布函数**。对于事件 $\bigcap\limits_{i=1}^n[X_i\le x_i]$，定义联合分布函数 $F(\mathbf x):=P\left( \bigcap\limits_{i=1}^n[X_i\le x_i] \right)$。对于 $n=2$，即 $F(x, y):=P(X\le x\land Y\le y)$。接下来我们着重讨论二元 $F(x, y)$ 的性质，对于 $n$ 维可以进行自然拓展。
- 单调性。$F(x, y)$ 对 $x$ 和 $y$ 分布单调。
- 有界性。$\lim\limits_{x\to-\infty}F(x, y)=\lim\limits_{y\to-\infty}F(x, y)=0$。$\lim\limits_{(x, y)\to(\infty, \infty)}F(x, y)=1$。
- **右侧连续性**。$\lim\limits_{x\to x_0^+}F(x, y_0)=F(x_0, y_0), \lim\limits_{y\to y_0^+}F(x_0, y)=F(x_0, y_0)$。
- 非负性（矩形差分）。$P(X\in(a, b]\land Y\in(c, d])=F(b, d)-F(a, d)-F(b, c)+F(a, c)\ge 0$。

**联合密度函数**。对连续随机变量定义。若存在函数 $p(x, y)$ 使得 $F(x, y)=\begin{aligned} \int_{-\infty}^x\int_{-\infty}^yp(u, v){\text d}u{\text d}v \end{aligned}$，则称 $p(x, y)$ 为 $(X, Y)$ 的**联合密度函数**。重要结论：若 $F(x, y)$ 偏导存在，则 $p(x, y)=\dfrac{\partial^2F(x, y)}{\partial x\partial y}$。
- 证明：按照定义式对 $F$ 依次求 $x$ 和 $y$ 的偏导即可。一个重要的理解方式就是，将 $p(x, y){\text d}x{\text d}y$ 可以按照一元情形一样，理解为 $P(X=x, Y=y)=p(x, y){\text d}x{\text d}y$。
	- 更具体地可以写出矩形差分然后对 ${\text d}x, {\text d}y$ 使用二阶泰勒展开。
### $\it 3.2$ 边际分布与独立性分析
**边际分布**。考虑一个二元分布中某一个随机变量的分布情况，即忽视另一个随机变量时分布函数的情形。若在 $(X, Y)$ 中只想研究 $X$ 的分布，则可以令 $F_X(x):=F(x, \infty)=\lim\limits_{y\to\infty}F(x, y)$。称其为 $X$ 的边际分布。同理有 $F_Y(y):=F(\infty, y)=\lim\limits_{x\to\infty}F(x, y)$。
- 在三维或更高维，可以得到更多不同的边际分布。

**边际密度函数**。令 $p_X(x)$ 表示只关注 $X$ 时的密度函数，即 $p_X(x)=\dfrac{{\text d}F(x, \infty)}{{\text d}x}$。得到 $p_X(x)=\begin{aligned} \int_{-\infty}^\infty p(x, y){\text d}y \end{aligned}$。同理 $p_Y(y)=\begin{aligned} \int_{-\infty}^\infty p(x, y){\text d}x \end{aligned}$。

**独立性分析**。考虑随机变量 $X_1, X_2, \cdots, X_n$，若 $F(x_1, x_2, \cdots, x_n)=\prod\limits_{i=1}^n F(x_i)$ 对任意 $\mathbf x=(x_1, x_2, \cdots, x_n)$，则称 $X_1, X_2, \cdots, X_n$ 相互独立。
- 注意：在连续情形下，这等价于 $p(\mathbf x)=\prod\limits_{i=1}^n p_{X_i}(x_i)$。
### $\it 3.3$ 随机向量函数及其期望与方差
列举常见的随机向量函数性质，这些均为极其常见的运算性质。
- 设 $Y=g(\mathbf X)$ 为向量函数，则 $E(Y)=\begin{aligned} \int_{\Omega}g(\mathbf v)p(\mathbf v){\text d}\mathbf v \end{aligned}$。
- 若 $(X, Y)$ 是随机向量，则 $E(X+Y)=E(X)+E(Y)$。
	- $E(X+Y)=\begin{aligned} \int_{-\infty}^\infty\int_{-\infty}^\infty(x+y)p(x, y){\text d}x{\text d}y=\int_{-\infty}^\infty\int_{-\infty}^\infty xp(x, y){\text d}x{\text d}y+\int_{-\infty}^\infty\int_{-\infty}^\infty yp(x, y){\text d}x{\text d}y \end{aligned}$。因此等于 $\begin{aligned} \int_{-\infty}^\infty xp_X(x){\text d}x+\int_{-\infty}^\infty yp_Y(y){\text d}y \end{aligned}=E(X)+E(Y)$。
	- 注：不要求 $X, Y$ 有任何其他性质。可以写成 $E\left( \sum\limits_{i=1}^nX_i \right)=\sum\limits_{i=1}^nE(X_i)$。
- 若 $X, Y$ 是独立的随机变量，则 $E(XY)=E(X)E(Y)$。
	- 证明。$E(XY)=\begin{aligned} \int_{-\infty}^\infty xyp(x, y){\text d}x{\text d}y \end{aligned}=\begin{aligned} \int_{-\infty}^\infty\int_{-\infty}^\infty xp_X(x){\text d}x\cdot yp_Y(y){\text d}y \end{aligned}$。即 $\begin{aligned} \left(\int_{-\infty}^\infty xp_X(x){\text d}x\right)\left(\int_{-\infty}^\infty yp_Y(y){\text d}y\right) \end{aligned}=E(X)E(Y)$。
	- 利用了 $p(x, y)=p_X(x)p_Y(y)$。
- 若 $X, Y$ 是独立随机变量，则 $\mathrm{Var}(X+Y)=\mathrm{Var}(X)+\mathrm{Var}(Y)$。
	- 按协方差性质，$\mathrm{Var}(\mathbf 1^T\mathbf X)=\mathbf 1^T\mathrm{Cov}(\mathbf X)\mathbf 1$。这里 $X,Y$ 独立，结果即为 $\mathrm{Var}(X)+\mathrm{Var}(Y)$。
### $\it 3.4$ 协方差与相关系数
**协方差**。定义 $\mathrm{Cov}(X, Y):=E[(X-E(X))(Y-E(Y))]$ 为随机变量 $X, Y$ 之间的协方差，其意义为平面上 $(X, Y)$ 到 $(E(X), E(Y))$ 的“矩形”的期望面积，描述了 $X, Y$ 的“正向关联程度”。
- $\mathrm{Cov}(X, Y)>0$ 称 $X, Y$ **正相关**。$\mathrm{Cov}(X, Y)=0$ 称 $X, Y$ **不相关**。$\mathrm{Cov}(X, Y)<0$ 称 $X, Y$ **负相关**。注意：**不相关**是一种**弱于**变量独立的条件，$X, Y$ 相互独立一定能得出 $\mathrm{Cov}(X, Y)=0$，而反之则不行（可能是复杂的非线性关系）。【反之 $\mathrm{Cov}(X, Y)\neq 0$ 则代表 $X, Y$ 不可能独立】。

**协方差的运算性质**。
- $\mathrm{Cov}(X, Y)=E(XY)-E(X)E(Y)$。
	- 据此看出，$E(XY)=E(X)E(Y)$ 的充要条件是 $X, Y$ 不相关，而独立性是比其更强的。
- $\mathrm{Cov}(X, Y)=\mathrm{Cov}(Y, X)$。
- $\mathrm{Cov}(X, a)=0$，其中 $a$ 是常数。
- $\mathrm{Cov}(aX, bY)=ab\mathrm{Cov}(X, Y)$。
- $\mathrm{Cov}(X+Y, Z)=\mathrm{Cov}(X, Z)+\mathrm{Cov}(Y, Z)$。这里 $X, Y, Z$ 是任意的，无需特殊要求。

**相关系数**。将随机变量 $X$ 标准化为 $X^*:=\dfrac{X-\mu}{\sigma}$。则其满足 $E(X^*)=0$，$\mathrm{Var}(X^*)=1$。定义 $X, Y$ 的相关系数为 $\mathrm{Corr}(X, Y):=\mathrm{Cov}(X^*, Y^*)=\dfrac{\mathrm{Cov}(X, Y)}{\sqrt{\mathrm{Var}(X)\mathrm{Var}(Y)}}$。
- **Schwartz 不等式**。$[\mathrm{Cov}(X, Y)]^2\le\mathrm{Var}(X)\mathrm{Var}(Y)$。
	- 证明的思路类似 Cauchy 不等式。构造 $0\le E(tX-Y)^2=t^2E(X^2)-2tE(XY)+E(Y^2)$。写出其关于 $t$ 的判别式即得 $E^2(XY)\le E(X^2)E(Y^2)$。
	- 立即得出 $\mathrm{Corr}(X, Y)\in[-1, 1]$。
### $\it 3.5$ 协方差矩阵
**协方差矩阵**。定义 $n$ 维随机向量 $\mathbf X=(X_1, X_2, \cdots, X_n)^T$。若每个分量的数学期望 $E(X_i)$ 均存在，则称 $E(\mathbf X):=(E(X_1), E(X_2), \cdots, E(X_n))^T$ 为随机向量 $\mathbf X$ 的数学期望向量。基于此定义协方差矩阵 $\mathrm{Cov}(\mathbf X):=E[(\mathbf X-E(\mathbf X))(\mathbf X-E(\mathbf X))^T]$。即：

$$\mathrm{Cov}(\mathbf X)=\begin{pmatrix} \mathrm{Var}(X_1) & \mathrm{Cov}(X_1, X_2) & \cdots & \mathrm{Cov}(X_1, X_n) \\ \mathrm{Cov}(X_2, X_1) & \mathrm{Var}(X_2) & \cdots & \mathrm{Cov}(X_2, X_n) \\ \vdots & \vdots & \ddots & \vdots \\ \mathrm{Cov}(X_n, X_1) & \mathrm{Cov}(X_n, X_2) & \cdots & \mathrm{Var}(X_n) \end{pmatrix}$$ 
这显然是一个实对称矩阵，因为 $\mathrm{Cov}(X_i, X_j)=\mathrm{Cov}(X_j, X_i)$。

**协方差矩阵的运算性质**。协方差的一个重要用途就是求线性期望加权变量 $Z=\mathbf u^T\mathbf X$ 的方差。我们指出：对于一组给定的系数 $\mathbf u=(u_1, u_2, \cdots, u_n)^T$，有 $\mathrm{Var}(\mathbf u^T\mathbf X)=\mathbf u^T\mathrm{Cov}(\mathbf X)\mathbf u$。我们在证明此式的时候将一并证明 $\mathrm{Cov}(\mathbf X)$ 的**半正定性**。
$$
\begin{aligned} \mathbf u^T\mathrm{Cov}(\mathbf X)\mathbf u &= \mathbf u^TE[(\mathbf  X-E(\mathbf  X))(\mathbf X-E(\mathbf X))^T]\mathbf u \\ &=E[\mathbf u^T(\mathbf X-E(\mathbf X))]^2 \\ &=E[Z-E(Z)]^2 = \mathrm{Var}(Z)\ge0 \end{aligned}
$$
- 协方差矩阵的额外性质：$\mathrm{Cov}(\mathbf X)=E(\mathbf X\mathbf X^T)-E(\mathbf X)E(\mathbf X^T)$。

**期望与线性代数运算**。期望是一个线性算子，满足 $E(X+Y)=E(X)+E(Y)$，$E(aX)=aE(X)$。这启发我们，$E$ 可能是某种线性“泛函”。不过泛函以及 Hilbert 空间超出了我们的讨论范围，我们只给出一些最基本的“运算升级”。
- $E(A\mathbf X)=AE(\mathbf X)$，其中 $A$ 是常数矩阵。
- 考察 $\mathrm{Cov}(\mathbf X)$ 何时正定。其不正定，当且仅当存在 $\mathbf u^T\mathbf X$ 几乎处处为常数，这是一组线性约束关系。因此，如果不存在这样的线性约束关系（$\mathbf u^T\mathbf X=c$，几乎处处），那么 $\mathrm{Cov}(X)$ 正定。
- 根据 $E(A\mathbf X)=AE(\mathbf X)$ 推出 $\mathrm{Cov}(A\mathbf X)=A\mathrm{Cov}(\mathbf X)A^T$。
	- 重要：按照上述思路，存在正交矩阵 $Q$ 使得 $\mathrm{Cov}(\mathbf X)=Q\Lambda Q^T$。换元 $\mathbf Z=Q^T\mathbf X$，则 $\mathbf Z$ 的协方差矩阵 $\mathrm{Cov}(\mathbf Z)=\Lambda$，即任意的 $Z_i, Z_j$ 不相关。
- 协方差矩阵的局限性在于，其只能检测线性约束关系，而面对 $X_1^2+X_2=0$ 这样的非线性约束则不能提供有效信息。
- 最后是关于 $\mathrm{Cov}(\mathbf X)$ 的几何直观。想象 $p(\mathbf x)$ 描述了 $\mathbb R^n$ 中的一个“概率云”（就跟电子云一样），如果 $\mathrm{Cov}(X_i, X_j)\neq 0$，说明点 $(X_i, X_j)$ 距离 $(E(X_i), E(X_j))$ 的“期望面积”不为零，因此这团云在概率上就会发生“偏移”，尤其表现为“长轴”的倾斜。如果 $\mathrm{Cov}(X_i, X_j)=0$，就说明 $X_i, X_j$ 不相关，呈现在概率云上就是长轴与坐标轴平行。
## $\mathrm{IV}$ 条件分布与条件期望
### $\it 4.1$ 条件分布
**条件密度函数**。设随机向量 $(X, Y)$ 的密度函数为 $p(x, y)$，其中边际分布函数为 $p_X(x), p_Y(y)$。则定义 $P(X\le x\mid Y=y):=\lim\limits_{h\to 0}P(X\le x\mid Y\in[y, y+h])$。
- 推导：$\begin{aligned} \mathrm{LHS}&=\lim\limits_{h\to 0}\dfrac{\int_{-\infty}^x\int_{y}^{y+h}p(u, v){\text d}v{\text d}u}{\int_{y}^{y+h}p_Y(v){\text d}v} \end{aligned}$ 由积分中值定理保证 $\begin{aligned} \lim\limits_{h\to 0}\int_y^{y+h}p_Y(v){\text d}v \end{aligned}=p_Y(y)$ 且 $\begin{aligned} \lim\limits_{h\to 0}\dfrac1h\int_y^{y+h}p(u, v){\text d}v=p(u, y) \end{aligned}$。所以：$P(X\le x\mid Y=y)=\begin{aligned} \int_{-\infty}^x\dfrac{p(u, y){\text d}u}{p_Y(y)} \end{aligned}$。记为 $F(x\mid y)$。
- 定义：$F(x\mid y):=\begin{aligned} \int_{-\infty}^x\dfrac{p(u, y){\text d}u}{p_Y(y)} \end{aligned}$。$p(x\mid y):=\dfrac{p(x, y)}{p_Y(y)}$。

连续情形下的全概率公式与 Bayes 公式。
- 全概率：$p_Y(y)=\begin{aligned} \int_{-\infty}^\infty p(y\mid x)p_X(x){\text d}x \end{aligned}$。
- Bayes：$p(x\mid y)=\begin{aligned} \dfrac{p_X(x)p(y\mid x)}{p_Y(y)} \end{aligned}=\begin{aligned} \dfrac{p_X(x)p(y\mid x)}{\int_{-\infty}^\infty p_X(x)p(y\mid x){\text d}x} \end{aligned}$。
### $\it 4.2$ 条件期望
**条件期望**。条件分布的数学期望（如果存在）称为条件期望。对于连续变量，$E(X\mid Y=y)=\begin{aligned} \int_{-\infty}^\infty xp(x\mid y){\text d}x \end{aligned}$。

**重期望公式**。设 $(X, Y)$ 是随机向量，且 $E(X)$ 存在，则有重要的结果：$E(X)=E[E(X\mid Y)]$。
- 条件方差公式：$\mathrm{Var}(X)=E(\mathrm{Var}(X\mid Y))+\mathrm{Var}(E(X\mid Y))$。
- 重期望公式是很重要的考点。
