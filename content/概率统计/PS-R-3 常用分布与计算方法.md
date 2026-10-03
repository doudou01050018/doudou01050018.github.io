---
publish: true

---

常用分布，所有常用的计算技巧。计算技巧等。
$$
\newcommand{\Beta}{\mathrm{B}}
$$
# PS-R-3 概率论 笔记 (3) 常用分布与计算技巧
## $\mathrm{I}$ 计算技巧梳理
### $\it 1.1$ 随机变量的函数
已知 $X$ 的分布，求 $Y=g(X)$ 的分布。
- 方法是求 $P(Y\le t)$，然后解方程 $Y\le t$ 对应 $X$ 的哪一部分，于是 $P(Y\le t)=\begin{aligned} \int_{g(X)\le t}p(x){\text d}x \end{aligned}$。然后求 $p_Y(t)=\dfrac{{\text d}P(Y\le t)}{{\text d}t}$ 即可。

已知 $Y=g(X)$，求 $E(Y)$。
- 直接求 $E(Y)=\begin{aligned} \int_{-\infty}^\infty g(x)p(x){\text d}x \end{aligned}$ 即可。

**卷积公式**。设 $X$ 和 $Y$ 是独立的连续随机变量，求 $Z=X+Y$ 的分布。
- 推导。换元 $u = x+y, v = x-y$。则 $P(X\le t)=\begin{aligned} \iint\limits_{x+y\le t}p_X(x)p_Y(y){\text d}x{\text d}y \end{aligned}$。求 Jacobi 行列式 $\dfrac{{\text d}u{\text d}v}{{\text d}x{\text d}y}=\begin{vmatrix} u_x & u_y \\ v_x & v_y \end{vmatrix}\Rightarrow2$。于是 $P(X\le t)=\begin{aligned} \int_{u=-\infty}^t{\text d}u\int_{-\infty}^\infty {\text d}v p_X\left( \dfrac{u+v}2 \right)p_Y\left( \dfrac{u-v}2 \right)\cdot\dfrac12 \end{aligned}$。所以 $p_Z(t)=\begin{aligned} \int_{-\infty}^\infty p_X(x)p_Y(t-x)\dfrac12{\text d}(2x-t)=\int_{-\infty}^\infty p_X(x)p_Y(t-x){\text d}x \end{aligned}$。
### $\it 1.2$ 期望与方差算式整理
期望：
- $E(X):=\begin{aligned} \int_{-\infty}^\infty xp(x){\text d}x \end{aligned}$。性质公式：
	- $E(X+Y)=E(X)+E(Y)$ 无条件成立。
	- $E(A\mathbf X)=AE(\mathbf X)$。
	- 若 $X, Y$ 独立，则 $E(XY)=E(X)E(Y)$。

方差：
- $\mathrm{Var}(X):=E(X-E(X))^2=\begin{aligned} \int_{-\infty}^\infty(x-E(X))^2p(x){\text d}x \end{aligned}$。
- $\mathrm{Var}(X)=E(X^2)-E^2(X)$。或 $\mathrm{Cov}(\mathbf X)=E(\mathbf X\mathbf X^T)-E(\mathbf X)E(\mathbf X^T)$。
- $\mathrm{Var}(\mathbf u^T\mathbf X)=\mathbf u^T\mathrm{Cov}(\mathbf X)\mathbf u$。或者更普遍地有 $\mathrm{Cov}(A\mathbf X)=A\mathrm{Cov}(\mathbf X)A^T$。
### $\it 1.3$ Jacobi 换元求分布
**Jacobi 换元求密度函数**。设 $\begin{cases}  u=u(x, y)\\v=v(x, y) \end{cases}$ 与 $(x, y)$ 存在双射关系，则 $p(u, v)=p(x, y)\cdot\begin{vmatrix} u_x & u_y \\ v_x & v_y \end{vmatrix}^{-1}$。
- 这里负一次方的含义是，若 $(x, y)\to(u, v)$ 的面积微分算子 $(u_x, v_x){\text d}x\times(u_y, v_y){\text d}y$ 相比标准面积算子 $(1 ,0){\text d}x\times(0, 1){\text d}y$ 是扩张（倍数放大），那么两处的概率总值相等，因此密度反而减小，所以需要乘 $|J|^{-1}$。

对 $n$ 元的情形完全一样。特别地，对于线性变换的情况，设 $\mathbf y=A\mathbf x$ 且 $A$ 可逆，那么 $p(\mathbf y)=p(\mathbf x)\cdot\begin{vmatrix} \dfrac{\partial\mathbf y}{\partial\mathbf x} \end{vmatrix}^{-1}$。其中 $\dfrac{\partial\mathbf y}{\partial\mathbf x}=A$。
## $\mathrm{II}$ 常用分布：Bernoulli 实验类
接下来将引入一些常用的分布（不分离散与连续，按照生成机制分类），其均有现实意义。需要着重记录其基本性质以及其推导。

Bernoulli 实验指的是，对某一个事件 $A$ 进行重复试验，每次均有成功与失败的可能，其中成功的概率为 $p$。实验重复 $n$ 次。对于 Bernoulli 实验的相关统计量，其产生的分布有：二项分布、Poisson 分布、几何分布、指数分布、Pascal 分布、超几何分布。
### $\it 2.1$ Bernoulli 实验成功次数——二项分布
**二项分布**（$n$ 重 Bernoulli 实验成功次数的分布）。考虑对事件 $A$ 进行 $n$ 重 Bernoulli 实验，其中每次成功的概率为 $p$，失败的概率为 $1-p$。记随机变量 $X$ 为实验成功的次数，则 $X$ 的取值范围是 $0, 1, 2, \cdots, n$。则有结论 $P(X=k)=\dbinom nk p^k(1-p)^{n-k}$。记作 $X\sim\mathit b(n, p)$。
- 证明。列出所有状态空间，即 $\mathbf w=(\omega_1, \omega_2, \cdots, \omega_n)$，其中 $\omega_i\in\{A, \overline A\}$。事件 $[X=k]$ 可以分解为每一种具体 $k$ 个事件成功，有 $\dbinom nk$ 种可能。对于每一种的可能，其概率均为 $p^k(1-p)^{n-k}$。因此总概率为 $P(X=k)=\dbinom nk p^k(1-p)^{n-k}$。

**二项分布的期望**。$E(X)=np$。
- 证明1：$E(X)=\sum\limits_{k=0}^nk\dbinom nkp^k(1-p)^{n-k}$。使用恒等式 $k\dbinom nk=n\dbinom{n-1}{k-1}$ 计算得到。
- 证明2：记 $X_i=[\omega_i=A]$，则 $X=\sum\limits_{k=1}^nX_i$。因此 $E(X)=\sum\limits_{k=1}^nE([\omega_i=A])=nE([\omega=A])=np$。

**二项分布的方差**。$\mathrm{Var}(X)=np(1-p)$。
- 证明：$E(X^2)=\sum\limits_{i=1}^nX_i^2+2\sum\limits_{i<j}E(X_iX_j)=np+n(n-1)p^2$。故 $\mathrm{Var}(X)=E(X^2)-E^2(X)=np(1-p)$。

**二项分布的可加性**。若 $X_1\sim\mathit b(n, p)$，$X_2\sim\mathit b(m, p)$，且 $X_1, X_2$ 独立，则 $X=X_1+X_2\sim\mathit b(n+m, p)$。
- 证明。$P(X=k)=\sum\limits_{i=0}^k\dbinom{n}{i}p^i(1-p)^{n-i}\dbinom{m}{k-i}p^{k-i}(1-p)^{m-k+i}$。即 $p^k(1-p)^{n+m-k}\sum\limits_{i=0}^k\dbinom ni\dbinom{m}{k-i}$。由 Vandermonde 恒等式得到右侧组合式结果为 $\dbinom{n+m}k$。证毕。
### $\it 2.2$ 二项分布的极限——Poisson 分布
**Poisson 分布**（连续时间中发生次数的分布）。在一段连续的**单位时间**内，每一个时间点都有可能发生事件 $A$，而单位时间内平均发生 $A$ 的次数是 $\lambda$）。求这段时间内发生事件 $A$ 的次数 $X$ 的分布。结果为 $P(X=k)=\dfrac{\lambda^k}{k!}e^{-\lambda}$。记作 $X\sim\mathit P(\lambda)$。
- 验证其概率之和为 $1$。$\sum\limits_{k=0}^\infty \dfrac{\lambda^k}{k!}e^{-\lambda}=e^{\lambda}e^{-\lambda}=1$。
- 计算 Poisson 分布的期望：$E(X)=\sum\limits_{k=0}^\infty k\dfrac{\lambda^k}{k!}e^{-\lambda}=\sum\limits_{k=1}^\infty \lambda e^{-\lambda}\cdot\dfrac{\lambda^{k-1}}{(k-1)!}=\lambda$。
- 计算 Poisson 分布的方差：$E(X^2)=\lambda^2+\lambda$，因此 $\mathrm{Var}(X)=\lambda$。

**Poisson 是二项分布的极限**（Poisson 定理）。考虑 $n$ 重 Bernoulli 实验，每次实验成功的概率是 $p_n$。若 $n\to\infty$ 而 $np_n\to\lambda$，则有 $\lim\limits_{n\to\infty}\dbinom nk p_n^k(1-p_n)^{n-k}=\dfrac{\lambda^k}{k!}e^{-\lambda}$。
- 证明可以使用 $n!\sim\sqrt{2\pi n}\left( \dfrac ne \right)^n$（Stirling 公式）。
- 总之这提供了一种全新的视角，面对 Poisson 分布不好处理的时候，可以考虑 $n$ 重 Bernoulli 实验，然后取 $n$ 的极限。
	- 例如 Poisson 分布的期望和方差，这正是 $\lim\limits_{n\to\infty}np$ 和 $\lim\limits_{n\to\infty}np(1-p)$。

**Poisson 分布的可加性**。若 $X_1\sim\mathit P(\lambda_1)$，$X_2\sim\mathit P(\lambda_2)$ 且 $X_1, X_2$ 独立，则 $X=X_1+X_2$ 服从 $X\sim\mathit P(\lambda_1+\lambda_2)$。
- 从 Poisson 分布的概念上很好解释（注意：$X_1-X_2$ 不服从 Poisson 分布）。可以按照卷积符号记作 $P(\lambda_1)*P(\lambda_2)=P(\lambda_1+\lambda_2)$。

**二项分布的 Poisson 近似**。当 $n\ge 20$ 且 $p\le 0.05$ 时，可以用 Poisson 分布近似二项分布，取 $\lambda=np$。
### $\it 2.3$ Bernoulli 实验首次成功的消耗次数——几何分布
**几何分布**。在不断的 Bernoulli 实验中，单次成功的概率为 $p$，记 $X$ 为 $A$ 首次成功时所消耗的实验次数，求 $X$ 的分布列。分布列很好写出，即 $P(X=k)=(1-p)^{k-1}p$。记作 $X\sim\mathit{Ge}(p)$。
- 计算几何分布的期望（级数）。$E(X)=\sum\limits_{k=1}^\infty k(1-p)^{k-1}p=p\sum\limits_{k=1}^\infty kq^{k-1}=p\sum\limits_{k=1}^\infty(q^k)'=p\left( \sum\limits_{k=1}^\infty q^k \right)'=\dfrac1p$。这里幂级数允许在收敛范围内任意地逐项积分和求导。
- 计算几何分布的期望（重期望）。$E(X)=1\cdot p+E(X+1)\cdot(1-p)$ 解得 $E(X)=\dfrac1p$。
	- 这体现了几何分布的**无记忆性**，一次实验失败之后所面对的情形与之前完全一致。
- 计算几何分布的方差。使用级数（类似的技巧）得到 $\mathrm{Var}(X)=\dfrac{1-p}{p^2}$。使用重期望即 $E(X^2)=1\cdot p+E(X+1)^2\cdot(1-p)$ 解得 $E(X^2)=\dfrac{2-p}{p^2}$。
### $\it 2.4$ 几何分布的扩展——Pascal 分布
**Pascal 分布**。在 Bernoulli 实验序列中，单次成功 $A$ 的概率为 $p$，记 $X$ 为 $A$ 第 $r$ 次成功时所消耗的实验次数，求 $X$ 的分布。则 $P(X=n)=\dbinom{n-1}{r-1}p^{r}(1-p)^{n-r}$（因为第 $n$ 次一定为成功）。记作 $X\sim\mathit{Nb}(r, p)$。
- 计算 Pascal 分布的期望。设 $X_i$ 表示“已经成功了 $i-1$ 次，直到 $i$ 次成功需要额外花费的次数”。那么 $X=\sum\limits_{i=1}^nX_i$。所以 $E(X)=\sum\limits_{i=1}^nE(X_i)=nE(X_1)$（无记忆性）。根据几何分布，$E(X_1)=\dfrac 1p$，因此 $E(X)=\dfrac np$。
	- 或者采用全期望递推：$E(X_n)=E(X_{n-1}+1)\cdot p+E(X_n+1)\cdot(1-p)$。
- 计算 Pascal 分布的方差。$E(X^2)=nE(X_1^2)+n(n-1)E(X_1X_2)=n\dfrac{2-p}{p^2}+n(n-1)\dfrac1{p^2}$（这里 $X_1, X_2$ 独立，因此可以直接化为乘积）。因此 $\mathrm{Var}(X)==n\dfrac{1-p}{p^2}$。
	- 或者注意到 $X_i$ 之间是**独立的**，因此 $\mathrm{Var}\left( \sum\limits_{i=1}^nX_i \right)=\sum\limits_{i=1}^n\mathrm{Var}(X_i)=n\mathrm{Var}(X_1)$。
### $\it 2.5$ 不放回抽样——超几何分布
**超几何分布**。从一个有限总体中不放回抽样的结果为超几何分布。考虑 $N$ 件产品中有 $M$ 件不合格，从中抽取 $n$ 件检测，则含有的不合格产品数量 $X$ 服从超几何分布。记作 $X\sim\mathit h(n, N, M)$。
- $P(X=k)=\dfrac{\text{C}_M^k\text{C}_{N-M}^{n-k}}{\text{C}_N^n}$。$k=0, 1, \cdots, \min\{M, n\}$。这是 **Vandermonde 恒等式**的原型。
- 计算超几何分布的期望。设 $X_i$ 表示“第 $i$ 个产品不合格”。则 $E(X)=\sum\limits_{k=1}^nE(X_i)=np$。这里 $p=\dfrac MN$。
- 计算超几何分布的方差。$E(X^2)=\sum\limits_{k=1}^nE(X_i^2)+\sum\limits_{i<j}2E(X_iX_j)$。其中 $E(X_i^2)=p$，$E(X_iX_j)=\dfrac{\text{C}_M^2}{\text{C}_N^2}$。得到 $E(X^2)=np+n(n-1)\cdot p\dfrac{M-1}{N-1}$。因此 $\mathrm{Var}(X)=np(1-p)\dfrac{N-n}{N-1}$。

与二项分布的近似。当 $n\ll N$ 时，不放回抽样对整体概率的改变很小，此时近似二项分布。
### $\it 2.6$ 二项分布的极限——正态分布
**正态分布**。考虑另一类二项分布的极限，令 $n\to\infty$ 而 $p$ 固定，记 $X$ 为实验成功次数，标准化 $X^*=\dfrac{X-np}{\sqrt{np(1-p)}}$。则 $X\sim\mathit N(0, 1)$（de Moivre-Laplace 定理）。
## $\mathrm{III}$ 常用分布：Poisson 过程类
Poisson 过程指的是连续时间尺度下某件事情 $A$ 发生的情形。对于有限时间内发生次数、首次发生等待时间、第 $n$ 次发生等待时间等，我们有 Poisson 分布、指数分布、Gamma 分布等。
### $\it 3.1$ 固定时间的离散计数——Poisson 分布
参考上文。
### $\it 3.2$ 首次等待时间——指数分布
**指数分布**。类似离散实验下的几何分布，对于连续“实验”的第一次成功等待时间，我们有指数分布。与 Poisson 分布完全一致，设 $\lambda$ 为单位时间内发生 $A$ 的次数，那么记 $X$ 为从某一时刻开始首次等到 $A$ 的发生所需要的时间，求 $X$ 的分布。
- 答案为 $p(x)=\lambda e^{-\lambda x}$，其中 $x\ge 0$。
- 计算指数分布的期望。$E(X)=\begin{aligned} \int_0^\infty x\cdot\lambda e^{-\lambda x}{\text d}x \end{aligned}=\dfrac1\lambda$。
- 计算指数分布的方差。$E(X^2)=\dfrac2{\lambda^2}$，因此 $\mathrm{Var}(X)=\dfrac1{\lambda^2}$。
- **无记忆性**。分析这个问题本身的性质，若一段时间内没有等到，那么接下来所面临的情形与之前一样，即 $P(X\ge t_1)=P(X\ge t_1+t_0\mid X\ge t_0)$，而指数分布是唯一能满足此方程的分布。

**指数分布是几何分布的极限**。将单位时间分为 $n$ 个 Bernoulli 实验，每个实验成功的概率是 $p_0$。那么由几何分布 $P(X=k)=(1-p_0)^{k-1}p_0$。计算 $P(X\le k)=\sum\limits_{i=1}^{k}(1-p_0)^{i-1}p_0=1-(1-p_0)^k$。考虑标准化时间 $t=\dfrac kn$，$X'=\dfrac Xn$，且引入参数 $\lambda=np_0$（固定），则 $P(X'\le t)=1-\left( 1-\dfrac\lambda n \right)^{tn}$。固定参数 $\lambda=np_0$ 求 $\lim\limits_{n\to\infty}1-\left( 1-\dfrac\lambda n \right)^{tn}=1-e^{-\lambda t}$。这正是指数分布的 $F(t)$。
### $\it 3.3$ $n$ 次等待时间——Gamma 分布
**Gamma 函数**。复习一下 Gamma函数：令 $\Gamma(t):=\begin{aligned} \int_0^\infty x^{t-1}e^{-x}{\text d}x \end{aligned}$。这个无穷积分在 $t>0$ 时收敛，具有如下性质：
- $\Gamma(1)=1, \Gamma\left( \dfrac12 \right)=\sqrt\pi$。
- $\Gamma(t+1)=t\Gamma(t)$。$\Gamma(n)=(n-1)!$（$n$ 为正整数）。

**Gamma 分布**。考虑对指数分布作一个最简单的拓展，事件第 $n$ 次发生的等待时间为 $X$，此时 $X$ 的分布为什么？
- 从 Pascal 分布推导 Gamma 分布是可行的，但是比指数分布复杂很多。
- 直接给出结果：对于单位时间发生平均次数为 $\lambda$（与 Poisson 分布、指数分布一致）且等待发生 $r$ 次的 $X$，$p(x)=\dfrac{\lambda^r}{\Gamma(r)}x^{r-1}e^{-\lambda x}$（$x\ge 0$）。
	- 注意：此分布可以拓展为 $r\in\mathbb R_+$，没有要求 $r$ 为正整数。
- 计算 Gamma 分布的期望。若 $r$ 为正整数意义，则 $E(X)=\sum\limits_{i=1}^nE(X_i)=nE(X_1)=\dfrac r\lambda$。若 $r$ 不要求为正整数，则只能按照定义：$E(X)=\begin{aligned} \int_{0}^\infty x\cdot\dfrac{\lambda^r}{\Gamma(r)}x^{r-1}e^{-\lambda x}{\text d}x \end{aligned}=\dfrac{\Gamma(r+1)}{\Gamma(r)\lambda}=\dfrac r\lambda$。
- 计算 Gamma 分布的方差。$E(X^2)$ 按照类似方法计算得到 $\dfrac{\Gamma(r+2)}{\Gamma(r)\lambda^2}=\dfrac{r(r+1)}{\lambda^2}$。所以 $\mathrm{Var}(X)=\dfrac r{\lambda^2}$。

**Gamma 分布的可加性**。若 $X\sim\mathit{Ga}(r_1, \lambda)$ 且与 $Y\sim\mathit{Ga}(r_2, \lambda)$ 独立，则 $Z=X+Y\sim\mathit{Ga}(r_1+r_2, \lambda)$。
- 这是一条很重要的推论，在指数分布以及 $\chi^2$ 分布中都非常重要。
- 以及另一条结论：$X\sim\mathit{Ga}(r, \lambda)$，则 $Y=kX\sim\mathit{Ga}\left( r, \dfrac\lambda k \right)$。

**Gamma 分布的特例**。
- 称 $\mathit{Ga}(1, \lambda)$ 为**指数分布**。
- 称 $\mathit{Ga}\left( \dfrac n2, \dfrac12 \right)$ 为 $\boldsymbol{\chi^2}$ **分布**。这里 $n$ 一般为正整数。其意义将在正态分布一节中体现。

**Gamma 分布与 Poisson 的对偶性**。设 $N\sim\mathit P(\lambda)$ 且 $T\sim\mathit{Ga}(r, \lambda)$，则 $P(N\ge r)=P(T\le 1)$。
- 这在意义上很好理解：考虑参数为 $\lambda$ 的 Poisson 过程，单位时间内发生次数 $\ge r$ 当且仅当“等待 $r$ 次发生所需时间 $\le 1$”。
- 或者进一步改写为更一般的：$N\sim\mathit P(\lambda t)$ 且 $T\sim\mathit{Ga}(r, \lambda)$，则 $P(N\ge r)=P(T\le t)$（$t$ 个单位时间内）。
## $\mathrm{IV}$ 常用分布：正态分布类
### $\it 4.1$ 正态分布
**正态分布**。正态分布起源于“大量微小误差”的总扰动估计。其方程为 $p(x)=\dfrac1{\sqrt{2\pi}\sigma}\exp\left( -\dfrac{(x-\mu)^2}{2\sigma^2} \right)$。其中 $E(X)=\mu, \mathrm{Var}(X)=\sigma^2$。记作 $X\sim\mathit N(\mu, \sigma^2)$。通常将其标准化为 $U:=\dfrac{X-\mu}{\sigma}$（**标准正态分布**），其服从 $U\sim\mathit N(0, 1)$。
- 计算正态分布的期望。$E(U)=\begin{aligned} \int_{-\infty}^\infty x p(x){\text d}x \end{aligned}$，注意这是一个奇函数，因此 $E(U)=0$。所以 $E(X)=E(\sigma U+\mu)=\mu$。
- 计算正态分布的方差。$\mathrm{Var}(U)=E(U^2)=\begin{aligned} \int_{-\infty}^\infty u^2\dfrac1{\sqrt{2\pi}}\exp\left({-\frac{u^2}2}\right){\text d}u=1 \end{aligned}$（分部积分法）。所以 $\mathrm{Var}(X)=\mathrm{Var}(\sigma X+\mu)=\sigma^2$。
	- Gauss 积分：$\begin{aligned} \int_{-\infty}^\infty\exp(-x^2){\text d}x=\sqrt\pi \end{aligned}$。
- 标准正态函数 $\Phi(u):=\begin{aligned} \int_{-\infty}^u\dfrac1{\sqrt{2\pi}}\exp\left( -\dfrac{x^2}2 \right){\text d}x \end{aligned}$。

**$3\sigma$ 准则**。$x=\pm\mu$ 是 $p(x)$ 的拐点。$P(|X-\mu|<\sigma)=0.6826$，$P(|X-\mu|<2\sigma)=0.9545$，$P(|X-\mu|<3\sigma)=0.9973$。

**正态分布的可加性**。若 $X\sim\mathit N(\mu_1, \sigma_1^2)$，$Y\sim\mathit N(\mu_2, \sigma_2^2)$ 且 $X, Y$ 独立。则 $Z=X+Y\sim\mathit N(\mu_1+\mu_2, \sigma_1^2+\sigma_2^2)$。

有关正态分布的更多性质将涉及 CLT（中心极限定理）、Fourier 变换等。
### $\it 4.2$ $n$ 元正态分布
**$n$ 元正态分布**是极为重要的一个分布，在许多地方都有非常重要的应用（如 ML）。

设 $\mathbf X=(X_1, X_2, \cdots, X_n)$ 是 $n$ 维随机向量，其期望为 $\mu=E(\mathbf X)$，协方差矩阵为 $\Sigma=\mathrm{Cov}(\mathbf X)$（必须正定）。那么称如下密度函数为 $\boldsymbol n$ **元正态分布**：
$$p(\mathbf x)=\dfrac1{(2\pi)^{\frac n2}|\Sigma|^{\frac12}}\exp\left( -\dfrac12(\mathbf x-\mu)^T\Sigma^{-1}(\mathbf x-\mu) \right)$$
根据协方差矩阵的性质，$E(A\mathbf X)=AE(\mathbf X)$，$\mathrm{Cov}(A\mathbf X)=A\mathrm{Cov}(\mathbf X)A^T$。我们知道若 $\mathbf X$ 服从正态分布，那么对 $X$ 施加线性变换之后仍然服从正态分布，因此据线性变换的期望和方差公式可以复现变换前后的正态分布表达式的关系。

有关研究 $n$ 元正态分布的思路，应当首先从向量和矩阵的整体出发，而非一开始就拘泥于分量（正如定义式里没有任何一个显式的 $X_i$ 分量）。不妨假设 $\mu=0$。由于 $\Sigma$ 正定，所以可以找到正交矩阵 $Q$ 使得 $\Sigma=Q\Lambda Q^T$，其中 $\Lambda$ 为对角矩阵。那么换元 $\mathbf Z=Q^T\mathbf X$，此时 $\mathbf Z$ 仍然服从正态分布。其满足 $\mathrm{Cov}(\mathbf Z)=\Lambda$。这说明 $\mathbf X$ 经过一个旋转，其主轴变为了**不相关**的随机变量分量（在正态分布中，不相关等价于独立性）。若 $\Lambda=\mathrm{diag}(\lambda_1^2, \lambda_2^2, \cdots, \lambda_n^2)$，则 $Z_i\sim\mathit N(0, \lambda_i^2)$。

所以我们知道了任意一个多元正态分布的本质就是一个经过了“旋转”的 $n$ 个正态分布的复合。考虑先研究标准正态分布：$\mathbf Z=(Z_1, Z_2, \cdots, Z_n)$，其中 $Z_i\sim\mathit N(0, 1)$ 且 $Z_i$ 与 $Z_j$ 两两独立。因此可以直接写出其联合分布函数 $p(\mathbf z)=\prod\limits_{i=1}^n\dfrac1{\sqrt{2\pi}}\exp\left( -\dfrac{z_i^2}2 \right)=\dfrac1{(2\pi)^{\frac n2}}\exp\left( -\dfrac12\mathbf z^T\mathbf z \right)$。这被称为标准正态分布，即 $\mathbf Z\sim\mathit N(\mathbf 0, I)$。考虑一般正态分布的密度函数是如何得来的：
- 现在对 $\mathbf Z$ 施加一个线性变换 $A=Q\Lambda^{\frac12}$（这里 $\Lambda^{\frac12}=\mathrm{diag}(\lambda_1, \lambda_2, \cdots, \lambda_n)$）得到 $\mathbf X=A\mathbf Z$。此时 $\mathrm{Cov}(\mathbf X)=A\mathrm{Cov}(\mathbf Z)A^T=AA^T=\Sigma$。根据 Jacobi 换元，有 $p(\mathbf x)=p(\mathbf z)\cdot |A|^{-1}=p(\mathbf z)\cdot\dfrac1{|\Sigma|^{\frac 12}}$。所以 $p(\mathbf x)=\dfrac1{|\Sigma|^{\frac12}}\cdot\dfrac1{(2\pi)^{\frac n2}}\cdot\exp\left( -\dfrac12\mathbf z^T\mathbf z \right)$。然后 $\mathbf x=A\mathbf z$ 因此 $\mathbf z=A^{-1}\mathbf x$。所以写为 $p(\mathbf x)=\dfrac1{|\Sigma|^{\frac12}}\cdot\dfrac1{(2\pi)^{\frac n2}}\cdot\exp\left( -\dfrac12\mathbf x^T\Sigma^{-1}\mathbf x \right)$。加入平移 $\mathbf x'=\mathbf x+\mu$ 之后就得到了最标准的形式。
- 同时可以看出为什么要求 $\Sigma$ 正定。如果 $\Sigma$ 不正定，则存在“零”方向，也就是实际有效的主轴数量不足 $n$ 个，起不到 $n$ 元正态分布的效果。

**$n$ 正态分布的关键性质（边缘分布）**。若 $\begin{pmatrix} \mathbf X_1 \\ \mathbf X_2 \end{pmatrix}\sim\mathit N\left(\begin{pmatrix} \mu_1 \\ \mu_2 \end{pmatrix}, \begin{pmatrix} \Sigma_{11} & \Sigma_{12} \\ \Sigma_{21} & \Sigma_{22} \end{pmatrix}\right)$，那么 $\mathbf X_1\sim\mathit N(\mu_1, \Sigma_{11})$。
- 证明。考虑 $A=\begin{pmatrix} I & 0 \end{pmatrix}$。令 $\mathbf X_1=A\mathbf X$。那么 $\mathbf X_1\sim\mathit N(A\mu, A\Sigma A^T)$。这相当于 $\mathbf X_1\sim\mathit N(\mu_1, \Sigma_{11})$。

**$n$ 正态分布的关键性质（条件分布）**。若 $\begin{pmatrix} \mathbf X_1 \\ \mathbf X_2 \end{pmatrix}\sim\mathit N\left(\begin{pmatrix} \mu_1 \\ \mu_2 \end{pmatrix}, \begin{pmatrix} \Sigma_{11} & \Sigma_{12} \\ \Sigma_{21} & \Sigma_{22} \end{pmatrix}\right)$，那么条件分布 $\mathbf X_1\mid\mathbf X_2=\mathbf x_2\sim\mathit N\left(\mu_{1\mid 2}, \Sigma_{1\mid 2}\right)$。其中 $\mu_{1\mid 2}=\mu_1+\Sigma_{12}\Sigma_{22}^{-1}(\mathbf x_2-\mu_2)$，$\Sigma_{1\mid 2}=\Sigma_{11}-\Sigma_{12}\Sigma_{22}^{-1}\Sigma_{21}$（Schur 补）。
- 推导暂时跳过。
### $\it 4.3$ $\chi^2$ 分布
接下来将记录三个基于正态分布构造的统计分布（三大抽样分布）。

**$\chi^2$ 分布**。设 $\mathbf X=(X_1, X_2, \cdots, X_n)$ 是取自 $\mathit N(\mathbf 0, I)$ 的随机变向量，记 $\chi^2=\mathbf X^T\mathbf X$，求 $\chi^2$ 的分布。
- 推导。先指出一个极为常用的结论，若 $X\sim\mathit N(0, 1)$，则 $X^2\sim\mathit{Ga}\left( \dfrac12, \dfrac12 \right)$。证明计算 $P(X^2\le t)$ 然后对 $t$ 求导即得到 $\mathit{Ga}\left( \dfrac12, \dfrac12 \right)$ 的密度函数。
- 然后根据 Gamma 分布的可加性，$\chi^2=\sum\limits_{i=1}^nX_i^2$，那么 $\chi^2\sim\mathit{Ga}\left( \dfrac n2, \dfrac12 \right)$。这个分布被特殊地记作 $\mathbf X^T\mathbf X\sim\chi^2(n)$，称其为**自由度为 $n$ 的 $\chi^2$ 分布**，即 $p(x)=\dfrac{\left( \frac12 \right)^{\frac n2}}{\Gamma\left( \frac n2 \right)}x^{\frac n2-1}e^{-\frac x2}$。
	- 更一般地，若 $\mathbf X\sim\mathit N(\mathbf 0, \sigma^2I)$，那么单个变量有 $X_i^2\sim\mathit{Ga}\left( \dfrac12, \dfrac 1{2\sigma^2} \right)$，得到 $\mathbf X^T\mathbf X\sim\mathit{Ga}\left( \dfrac n2, \dfrac1{2\sigma^2} \right)$。

**$\chi^2$ 分布的重要性质**。设 $X_1, X_2, \cdots, X_n$ 是来自 $\mathit N(\mu, \sigma^2)$ 的独立同分布的样本，其样本均值为 $\overline x$，样本方差为 $s^2$。则：
1. $\overline x$ 与 $s^2$ 独立。
2. $\overline x\sim\mathit N\left( \mu, \dfrac{\sigma^2}n \right)$。
3. $\dfrac{(n-1)s^2}{\sigma^2}\sim\chi^2(n-1)$。

注意到，$(n-1)s^2$ 可以写成 $\mathbf X^TP\mathbf X$ 的二次型形式，其中 $P=I-\dfrac1n J$（$J$ 为全 $1$ 矩阵）。为了对 $\mathbf X^TP\mathbf X$ 应用可加性结论，需要找到矩阵 $P$ 的“主轴”。分析 $P$ 的性质，其为对称幂等矩阵（一定有 $n$ 个特征方向），即 $P^T=P$ 和 $P^2=P$，因此根据幂等矩阵的结论，$P$ 的特征值只能为 $0$ 或 $1$。解方程 $\left( I-\dfrac1n J \right)\mathbf x=\mathbf 0$，得到 $\mathbf x=\lambda\mathbf 1_n$，也就是说只有 $\mathbf 1_n$ 一个方向是零空间，因此剩余的 $n-1$ 个方向（与 $\mathbf 1_n$ 正交的空间）的特征值全部为 $1$。

考虑 $P=Q\Lambda Q^T$ 这样的分解，其中 $\Lambda$ 为包含 $n-1$ 个 $1$ 与一个 $0$ 的对角矩阵。不妨设其第一行的特征值为 $0$，那么 $Q$ 的第一列为对应的单位特征向量 $\dfrac1{\sqrt n}\mathbf 1_n$，其余的列为其它特征值为 $1$ 的特征向量（与 $\mathbf 1_n$ 正交）。接下来就可以对 $\mathbf X$ 作线性变换了，令 $\mathbf Z=Q^T\mathbf X$，那么 $(n-1)s^2=\mathbf Z^T\Lambda\mathbf Z=\sum\limits_{i=2}^nZ_i^2$。因为 $\mathbf X\sim\mathit N(\mu\mathbf 1_n, \sigma^2I)$，那么对 $X$ 作线性变换之后的向量依然服从正态分布，即 $\mathbf Z\sim\mathit N(\mu Q^T\mathbf 1_n, \sigma^2(Q^TIQ))=\mathit N(\mu Q^T\mathbf 1_n, \sigma^2I)$。其中 $\mu Q^T\mathbf 1_n=(\mu\sqrt n, 0, 0, \cdots, 0)$（因为 $Q$ 除了第一列其余的列向量都与 $\mathbf 1_n$ 正交）。因此对 $i\ge 2$ 有 $Z_i\sim\mathit N(0, \sigma^2)$，所以根据 Gamma 分布的可加性，有 $\sum\limits_{i=2}^nZ_i^2\sim\mathit{Ga}\left( \dfrac n2, \dfrac1{2\sigma^2} \right)$。即 $\dfrac{(n-1)s^2}{\sigma^2}\sim\chi^2(n-1)$。
- 这直接证明了（3）。对于（1），有 $(n-1)s^2=\sum\limits_{i=2}^nZ_i^2$ 而 $\overline x\sim Z_1$，因此独立性成立。（2）根据正态分布的可加性直接得到。
- 注：这个做法基于二次型分析，没有像课本那样显式构造 $Q$，省去了不必要的麻烦。这个方法是 Cochran 定理的核心，以后处理 $\mathbf X^TA\mathbf X$ 的分布问题（$\mathbf X$ 为正态），都可以像这样转化为特征分析问题（将二次型分解到主轴方向，再应用 Gamma 分布的可加性）。

**$\chi^2$ 分布的期望和方差**。根据 Gamma 分布，若 $X\sim\chi^2(n)$，则 $E(X)=n$，$\mathrm{Var}(X)=2n$。
### $\it 4.4$ $F$ 分布
承接 $\chi^2$ 分布，若 $X_1, X_2, \cdots, X_m, Y_1, Y_2, \cdots, Y_n\sim\mathit N(0, 1)$，则 $F=\dfrac{\dfrac1m\sum\limits_{i=1}^mX_i^2}{\dfrac1n\sum\limits_{i=1}^nY_i^2}$ 的分布情况是怎样的？换言之，此时 $X\sim\chi^2(m)$，$Y\sim\chi^2(n)$，求 $F=\dfrac{\dfrac1mX}{\dfrac 1nY}$ 的分布。这个问题的答案称为 $F$ 分布。

有了 $\chi^2$ 分布的铺垫，我们只用计算已知分布的随机变量的比值是什么分布。先计算 $Z=\dfrac XY$ 的分布，根据随机变量之比的分布公式（Jacobi 换元）以及 Gamma 函数的定义，有 $p_Z(z)=\begin{aligned} \int_{0}^\infty x_2p_X(zx_2)p_Y(x_2){\text d}x_2 \end{aligned}$。然后计算 $F=\dfrac nmZ$ 的分布，得到： 
$$p_F(x)=\dfrac{\Gamma\left( \dfrac{m+n}2 \right)\left( \dfrac mn \right)^\frac m2}{\Gamma\left( \dfrac m2 \right)\Gamma\left( \dfrac n2 \right)}x^{\frac m2-1}\left( 1+\dfrac mnx \right)^{-\frac{m+n}2}$$
$F$ 分布的作用是计算正态数据方差比值的分布。根据上述推导不难有如下结论：若 $\mathbf x=(x_1, x_2, \cdots, x_m)$ 是来自 $\mathit N(\mu_1, \sigma_1^2)$ 的一组样本，$\mathbf y=(y_1, y_2, \cdots, y_n)$ 是来自 $\mathit N(\mu_2, \sigma_2^2)$ 的样本，两组样本独立，则 $F=\dfrac{s_x^2 / \sigma_1^2}{s_y^2 / \sigma_2^2}\sim\mathit F(m-1, n-1)$。
### $\it 4.5$ $t$ 分布
设 $M\sim\mathit N(0, 1)$，$X\sim\chi^2(n)$，则称 $t=\dfrac{M}{\sqrt{X / n}}$ 的分布为自由度为 $n$ 的 $t$ 分布，记作 $t\sim \mathit t(n)$。
- 根据正态分布的对称性，有 $P(0<t<y)=P(-y<t<0)$，于是 $P(0<t<y)=\dfrac12P(t^2<y^2)$。由 $F$ 变量的构造，$t^2=\dfrac{M^2}{X / n}\sim \mathit F(1, n)$。将上式两侧按 $y$ 求导得到 $p_t(y)=\dfrac{\Gamma\left( \dfrac{n+1}2 \right)}{\sqrt{n\pi}\Gamma\left( \dfrac n2 \right)}\left( 1+\dfrac{y^2}n \right)^{-\frac{n+1}2}$。

$F$ 分布和 $t$ 分布在统计上有很多应用（假设检验类）。其意义等待补充。

结论：$t(n)\xrightarrow L \mathit N(0, 1)$ 当 $n\to\infty$。
## $\mathrm{V}$ 常用分布：Bayes 共轭类
### $\it 5.0$ Intro
如果先验的参数本身是随机变量，那么其服从什么分布？考虑一个 Bayes 中的计算问题：我们想要根据先验计算后验，假设我们的先验 $\theta$ 有一个分布 $P(\theta)$，似然为 $P(X\mid\theta)$，想要求解后验 $P(\theta\mid X)$ 的具体分布。据 Bayes 公式有 $P(\theta\mid X)=\dfrac{P(X\mid\theta)P(\theta)}{P(X)}$，其中分母为 $P(X)=\begin{aligned} \int_{-\infty}^\infty P(X\mid\theta)P(\theta){\text d}\theta \end{aligned}$。这个积分通常没有闭式解，需要进行数值积分或采样，计算成本很高。但是有一类特殊的分布：若 $P(X\mid\theta)$ 的分布与 $P(\theta)$ 的分布是一对**共轭的分布**，那么后验 $P(\theta\mid X)$ 将自动与先验 $P(\theta)$ 属于**同一个分布族**。于是，不用再计算 $P(X)$，因为我们知道了 $P(\theta\mid X)$ 的形状，那么归一化常数可以直接由先验和似然得到。且后验的参数可以直接由先验和似然的参数得出。
### $\it 5.1$ Beta 分布
**Beta 函数**。复习一下 Beta 函数：$\Beta(a, b):=\begin{aligned} \int_0^1x^{a-1}(1-x)^{b-1}{\text d}x \end{aligned}$。其中参数 $a>0, b>0$ 时该瑕积分收敛。其基本性质为 $\Beta(a, b)=\Beta(b, a)$ 且 $\Beta(a, b)=\dfrac{\Gamma(a)\Gamma(b)}{\Gamma(a+b)}$。记作 $X\sim\mathit{Be}(a, b)$。
- 从 $\Gamma(t)$ 是阶乘的连续化可以看出，Beta 函数是组合数的连续化。

**Beta 分布**。Beta 分布正是为了解决“概率的概率”而生。设想一个情景，需要估计抛一枚硬币其正面朝上的概率 $p$。我们不知道 $p$ 的具体值，但是我们有一些先验，想通过实操抛硬币来更新我们的信念。而 Beta 分布正是有关“概率的分布”。Beta 分布被描述为 $p(x)=\dfrac1{\Beta(a, b)}x^{a-1}(1-x)^{b-1}$（$x\in[0, 1]$）。
- 有关其参数的意义：$a$ 表示先验中正面的次数，$b$ 表示先验中反面的次数，$a+b$ 越大代表先验越强。

**Beta 分布与更新先验**。设我们的先验为 $p\sim\mathit{Be}(a, b)$，观测到在 $n$ 次抛硬币中有 $k$ 次正面，$n-k$ 次反面，那么我们的后验为：$p\mid(k, n)\sim\mathit{Be}(a+k, b+n-k)$。
- 这是因为 Beta 分布（先验）与二项分布（似然）为共轭分布。

**Beta 分布的期望和方差**。根据 Beta 函数的性质不难得到 $E(X)=\dfrac a{a+b}$ 与 $\mathrm{Var}(X)=\dfrac{ab}{(a+b)^2(a+b+1)}$。
### $\it 5.N$ 指数族

## $\mathrm{VI}$ Fourier 变换与特征函数
概率密度函数是 Fourier 变换的良好样本（因为 $\begin{aligned} \int_{-\infty}^\infty p(x){\text d}x=1 \end{aligned}$）。对密度函数进行 Fourier 变换可以得到许多良好的性质，简化计算。
### $\it 6.1$ Fourier 变换简介
**Fourier 变换**。对函数 $f(x)$ 作如下变换：$\varphi_f(t):=\begin{aligned} \int_{-\infty}^\infty e^{{\text i}tx}f(x){\text d}x \end{aligned}$。得到的 $\varphi_f(t)$ 函数称为 $f(x)$ 的 Fourier 变换。
- 根据 Fourier 惊人的注意力，这个变换相当于**提取了 $f(x)$ 的所有频率信息**（$e^{{\text i}tx}=\cos(tx)+{\text i}\sin(tx)$，相当于将 $f(x)$ 与不同频率的三角函数相乘）。那么对任意的频率 $t\in\mathbb R$ 作提取，是否可以得到完整的 $f(x)$ 的所有信息？答案是肯定的，事实是，$\varphi_f(x)$ 与 $f(x)$ 为**一一对应**关系，即知道其一可以复现出唯一的另一半。

应用于概率密度函数 $p(x)$ 时，定义 $\varphi(t):=\begin{aligned} \int_{-\infty}^\infty e^{{\text i}tx}p(x){\text d}x \end{aligned}$ 为 $p(x)$ 的**特征函数**。即 $\varphi(t)=E(e^{{\text i}tX})$。因为 $|e^{{\text i}tX}|=1$，所以其期望总是存在。由于随机变量 $X$ 可以看做其对应的 $p(x)$ 函数，所以通常称其 Fourier 变换为 $X$ 的分布特征函数 $\varphi_X(t)$。

在此记录其双射的具体转化方法：假设已知特征函数 $\varphi(t)$，则相应的分布函数满足对任意的 $x_1<x_2$，有 $F(x_2)-F(x_1)=\lim\limits_{T\to\infty}\dfrac1{2\pi}\begin{aligned} \int_{-T}^T\dfrac{e^{-{\text i}tx_1}-e^{-{\text i}tx_2}}{{\text i}t}\varphi(t){\text d}t \end{aligned}$。若 $X$ 为连续随机变量，则有更强的结果 $p(x)=\dfrac1{2\pi}\begin{aligned} \int_{-\infty}^\infty e^{-{\text i}tx}\varphi(t){\text d}t \end{aligned}$。
- 证明涉及复杂的复分析，不作要求。
### $\it 6.2$ 特征函数的性质
在此列出特征函数的若干重要性质。
1. $|\varphi(t)|\le\varphi(0)=1$。
2. $\varphi(-t)=\overline{\varphi(t)}$（表示复数的共轭）。
3. $Y=aX+b$，则 $\varphi_Y(t)=e^{{\text i}bt}\varphi_X(at)$。
4. （重要）**特征函数的乘积等价于原函数的卷积**。即：若 $X, Y$ 独立，那么 $\varphi_{X+Y}(t)=\varphi_X(t)\varphi_Y(t)$。
	- 而对于同一类分布而言，其特征函数也呈现出同一形式，所以根据这一特性可以方便地判定独立变量之和是什么分布。
5. 若 $E(X^r)$ 存在，则 $X$ 的特征函数 $\varphi(t)$ 可 $r$ 次求导，且对 $1\le k\le r$ 有 $\varphi^{(k)}(0)={\text i}^kE(X^k)$。
### $\it 6.3$ 常用分布的特征函数
暂时只记一种：正态分布 $\mathit N(\mu, \sigma^2)$ 的特征函数为 $\exp\left( {\text i}\mu t-\dfrac{\sigma^2t^2}2 \right)$。
- 可以直接看出，对正态分布变量作卷积时，其特征函数直接相乘，因此直接地展现了正态分布的可加性。
## $\mathrm{VII}$ 其他分布
### $\it 7.1$ Cauchy 分布
### $\it 7.2$ 对数正态分布
