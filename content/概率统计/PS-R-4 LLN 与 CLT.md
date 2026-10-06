---
publish: true
---

# PS-R-4 概率论 笔记 (4) 大数定律与中心极限定理
## $\mathrm I$ 随机变量序列的收敛
为了描述随机变量的统计量在数据规模越来越大的情况下如何“逼近”概率（大数定律、中心极限定理），我们需要先仿照序列极限的方式，引入依概率收敛、依分布收敛等定义。
### $\it 1.1$ 依概率收敛
**依概率收敛**。设 $\{X_n\}$ 一列随机变量，$X_0$ 是某个随机变量，若对任意 $\varepsilon>0$ 成立 $\lim\limits_{n\to\infty}P(|X_n-X_0|\ge\varepsilon)=0$，则称 $\{X_n\}$ **依概率收敛**到 $X_0$。记作 $X_n\xrightarrow PX_0$。
- 完全展开概念：对任意 $\varepsilon_P>0$ 以及 $\varepsilon>0$，存在一个 $N$ 使得 $n\ge N\to P(|X_n-X_0|\ge\varepsilon_P)<\varepsilon$。这与一般 $\lim\limits_{n\to\infty}x_n=A$ 的区别在于其为概率收敛为零（测度），而非偏差必然趋于零。
- 可以看出，这与数列极限是完全同构的定义，因此其具有四则运算性质是不奇怪的。其说明，当 $n$ 充分大时，$X_n$ 与 $X_0$ 的偏差超过任意给定正数的概率可以任意小。也就是 $X_n$ 可以与 $X$ 在测度意义下“视为同一个随机变量”。

**依概率收敛的四则运算**。
- 设 $X_n\xrightarrow Pa$ 且 $Y_n\xrightarrow Pb$，那么 $X_n+Y_n\xrightarrow Pa+b$。
	- 证明。$\{|X_n+Y_n-a-b|\ge\varepsilon\}\subseteq\left\{ |X_n-a|\ge\dfrac\varepsilon2 \right\}\cap\left\{ |Y_n-b|\ge\dfrac\varepsilon2 \right\}$，因此 $P(|X_n+Y_n-a-b|\ge\varepsilon)\le P\left( |X_n-a|\ge\dfrac\varepsilon2 \right)+P\left( |Y_n-b|\ge\dfrac\varepsilon2 \right)\to 0$。
- 设 $X_n\xrightarrow Pa$ 且 $Y_n\xrightarrow Pb$，那么 $X_n-Y_n\to a-b$。
- 那么 $X_nY_n\xrightarrow Pab$。
- 以及 $\dfrac{X_n}{Y_n}\xrightarrow P\dfrac ab$（当 $b\neq 0$）。
### $\it 1.2$ 依分布收敛（弱收敛）
**依分布收敛**（弱收敛）。设 $X$ 是随机变量，其分布函数是 $F(x)$。$\{X_n\}$ 是一系列随机变量，其分布函数是 $F_n(x)$。若对于 $F(x)$ 的任一**连续点**都有 $\lim\limits_{n\to\infty}F_n(x)=F(x)$ 成立，则称 $F_n(x)$ 弱收敛于 $F(x)$，记作 $F_n(x)\xrightarrow WF(x)$。同时称 $X_n$ 依分布收敛到 $X$，记作 $X_n\xrightarrow LX$。
- 点点收敛对于分布函数而言是一个过强的性质：因为分布函数在跳跃点处的不连续性是本质的，若要求在跳跃点也逐点收敛，则会与极限分布函数的跳跃结构产生冲突，导致本来“合理”的收敛被排除，甚至极限函数根本不是分布函数。
	- 例如，随机变量序列满足 $P\left( X_n=\dfrac1n \right)=1$，则其分布函数为$F_n(x)=\begin{cases}0,&x<\dfrac1n,\\1,&x\ge\dfrac1n.\end{cases}$。若要求在每点收敛，则 $F_n(x)$ 在 $x=0$ 处对任意 $n$ 都有 $F_n(0)=0$，从而逐点极限在 $x=0$ 的值为 $0$；但 $X_n\xrightarrow P0$，极限分布应为退化在 $0$ 的分布，其在 $x=0$ 处应为 $1$。因此若坚持点点收敛，就会把这种自然的收敛排除。（甚至 $\lim\limits_{n\to\infty}F_n(x)$ 根本不是合法的分布函数）

**依概率收敛是强于依分布收敛**的。即：$X_n\xrightarrow PX\Rightarrow X_n\xrightarrow L X$，但逆命题不成立。
- 暂时不给出其证明，这是因为依分布收敛只要求其分布函数的形状长得一样，而不对随机变量之间的相关性有任何要求。而依概率收敛就是强得多的性质，其要求两者的差距为在概率测度下几乎一定为零。
	- 例如 $X_1=-X_2$ 是一对有约束的随机变量，只要 $X_1$ 的分布函数是对称的，那么 $F_1(x)=F_2(x)$。于是 $X_2\xrightarrow L X_1$，即依分布收敛成立。但实际上 $|X_2-X_1|$ 完全不趋于 $0$。或者即便同分布的 $X_1, X_2$ 独立，其 $|X_1-X_2|$ 也没有任何保障。
- 特例：$X_n\xrightarrow Pc\iff X_n\xrightarrow Lc$。
## $\mathrm{II}$ 大数定律：概率是频率的极限
回到第一章的笔记，彼时我们只基于测度公理规定了概率的运算规则，而并没有赋予其任何实际意义（并没有强制任何人以任何形式怎样理解概率）。实际上，在概率的数学化定义中，也从来就不涉及这一点，但是概率公理可以导出一些“理所应当”而非常符合实际认知的结论。

接下来我们来通过严格的数学推导，看看大数定律是如何被概率公理导出的，以及如此被规范定义的概率为何能被用于指导实际决策。
### $\it 3.1$ Bernoulli 大数定律
大数定律有很多种，其中 Bernoulli 大数定律是较为简单的。

**Bernoulli 大数定律**。考虑 $n$ 重 Bernoulli 实验，即 $S_n$ 为这 $n$ 次实验中 $A$ 发生的次数，$p=P(A)$，则对任意 $\varepsilon>0$，有 $\lim\limits_{n\to\infty}P\left(\left|\dfrac{S_n}{n}-p\right|<\varepsilon\right)=1$。
- 也可写作 $\dfrac{S_n}{n}\xrightarrow Pp$，即频率依概率收敛于概率。
- 证明。$E\left( \dfrac{S_n}{n} \right)=p$，$\mathrm{Var}\left( \dfrac{S_n}{n} \right)=\dfrac{p(1-p)}{n}$。于是根据 Chebyshev 不等式，任取 $\varepsilon>0$，有 $P\left(\left|\dfrac{S_n}{n}-p\right|\ge\varepsilon\right)\le \dfrac{D\left(\dfrac{S_n}{n}\right)}{\varepsilon^2}=\dfrac{p(1-p)}{n\varepsilon^2}\to 0$。
- 再次提醒：$P(A)=1$ 不代表 $A$ 是必然事件。
### $\it 3.2$ 大数定律的一般形式
扩展到一般形式，有一列随机变量 $\{X_n\}$，若满足对任意 $\varepsilon>0$，有
$$
\lim\limits_{n\to\infty}P\left(\left|\dfrac1n\sum\limits_{i=1}^nX_i-\dfrac1n\sum\limits_{i=1}^nE(X_i)\right|<\varepsilon\right)=1$$
则称其（随机变量序列 $\{X_n\}$）**服从大数定律**。给定不同的约束条件，可以得到不同版本的大数定律。
- 下称 $\mathrm{bias}:=\dfrac1n\sum\limits_{i=1}^n X_i-\dfrac1n\sum\limits_{i=1}^n E(X_i)$。
#### $\it 3.2.1$ Chebyshev 大数定律
Chebyshev 大数定律。设 $\{X_n\}$ 为一列两两不相关的随机变量序列，若 $\mathrm{Var}(X_i)$ 均存在，且存在 $c\in\mathbb R$ 使得对任意 $i$ 有 $\mathrm{Var}(X_i)\le c$，则 $\{X_n\}$ 服从大数定律。
- 证明。$\mathrm{Var}\left( \dfrac1n\sum\limits_{i=1}^nX_i \right)=\dfrac1{n^2}\sum\limits_{i=1}^n\mathrm{Var}(X_i)\le\dfrac cn$。所以 $P(\mathrm{bias}<\varepsilon)\ge1-\dfrac{\mathrm{Var}\left( \dfrac1n\sum\limits_{i=1}^nX_i \right)}{\varepsilon^2}\ge1-\dfrac c{n\varepsilon^2}$（Chebyshev 不等式）。所以 $n\to\infty$ 时，$\lim\limits_{n\to\infty}P(\mathrm{bias}<\varepsilon)=1$。
- Chebyshev 大数定律只要求 $X_i$ 之间彼此独立，不要求 $X_i$ 的分布列有任何形式的相同。
#### $\it 3.2.2$ Markov 大数定律
Markov 大数定律。在 Chebyshev 大数定律的证明中，$\mathrm{Var}\left( \dfrac1n\sum\limits_{i=1}^nX_i \right)\to 0$ 是右侧放缩成立的根本条件，在 Chebyshev 大数定律中由 $\mathrm{Var}(X_i)\le c$ 保证。
- 称 **Markov 条件** 为 $\lim\limits_{n\to\infty}\mathrm{Var}\left( \dfrac1n\sum\limits_{i=1}^nX_i \right)=0$。若 Markov 条件成立，则 Chebyshev 大数定律自动成立，此时称其为 **Markov 大数定律**。
- Markov 大数定律是一种更普遍的情形，要求 $X_i$ 的方差都存在，对于 $X_i$ 的同分布、独立性、不相关性等没有要求。
#### $\it 3.3$ Khinchin 大数定律
Khinchin 大数定律。设 $\{X_n\}$ 是独立、同分布的随机变量序列，若 $E(X_i)$ 存在，则 $\{X_n\}$ 服从大数定律。
## $\mathrm{III}$ 中心极限定理 CLT：必然的正态分布
一句话总结，若有一系列大量、微小的随机扰动（满足某条件），则其总影响即为正态分布。
### $\it 4.1$ Lindeberg-Levy CLT
**Lindeberg-Levy CLT**。设 $\{X_n\}$ 是独立、同分布的随机变量序列，$E(X_i)=\mu$，$\mathrm{Var}(X_i)=\sigma^2>0$，则 $\dfrac{\sum\limits_{i=1}^nX_i-n\mu}{\sqrt n\sigma}\xrightarrow LN(0,1)$。
- 证明采用了特征函数。设 $Y_n:=\sum\limits_{i=1}^nX_i$，$Y_n^*:=\dfrac{Y_n-E(Y_n)}{\sqrt{\mathrm{Var}(Y_n)}}$，且单个 $X_i-\mu$ 的特征函数为 $\varphi(t)$。那么 $\varphi_{Y_n^*}=\left[\varphi\left( \dfrac t{\sigma\sqrt n} \right)\right]^n$。因为 $E(X_i-\mu)=0$，$\mathrm{Var}(X_i-\mu)=\sigma^2$，所以 $\varphi'(0)=0$，$\varphi''(0)=-\sigma^2$。所以 Taylor 展开得到 $\varphi(t)=1-\dfrac12\sigma^2t^2+o(t^2)$。故 $\lim\limits_{n\to\infty}\varphi_{Y_n^*}(t)=\lim\limits_{n\to\infty}\left[1-\dfrac{t^2}{2n}+o\left( \dfrac{t^2}n \right)\right]^n=\exp\left( -\dfrac{t^2}2 \right)$。这正是 $\mathit N(0, 1)$ 的特征函数。
### $\it 4.2$ 二项分布的正态近似
**de Moivre-Laplace CLT**（二项分布的正态近似）。在 $n$ 重 Bernoulli 实验中，若 $S_n$ 为 $A$ 发生的次数，$p=P(A)$，则 $\dfrac{S_n-np}{\sqrt{np(1-p)}}\xrightarrow LN(0,1)$。
- 即：设 $Y_n^*=\dfrac{S_n-np}{\sqrt{np(1-p)}}$，那么 $\lim\limits_{n\to\infty}P(Y_n^*\le\lambda)=\Phi(\lambda)=\begin{aligned} \int_{-\infty}^\lambda\exp\left( -\dfrac{t^2}2 \right){\text d}t \end{aligned}$。

这说明，正态分布（以及形如 $\exp(-x^2)$ 类的函数）是组合数的连续化版本。在 $n$ 较大时（$np>5$ 或 $n(1-p)>5$），通常采用正态分布近似二项分布（$p$ 较小时也可以采用 Poisson 分布近似）。
### $\it 4.3$ 独立不同分布情形的中心极限定理
Lindeberg-Levy 定理要求诸 $X_i$ 是同分布的，而即便是不同分布的 $X_i$ 在满足一定条件时也可以适用中心极限定理。

Lindeberg 中心极限定理。设 $\{X_n\}$ 是相互独立的随机变量序列，具有 $E(X_i)=\mu_i, \mathrm{Var}(X_i)=\sigma_i^2$。讨论 $Y_n=\sum\limits_{i=1}^nX_i$ 的分布情况。不难得出 $E(Y_n)=\sum\limits_{i=1}^n\mu_i$，$B_n:=\sigma(Y_n)=\sqrt{\sum\limits_{i=1}^n\sigma_i^2}$。标准化 $Y_n$，令 $Y_n^*:=\dfrac{Y_n-E(Y_n)}{B_n}=\sum\limits_{i=1}^n\dfrac{X_i-\mu_i}{B_n}$。
- 考虑如何使每一个 $\dfrac{X_i-\mu_i}{B_n}$“均匀地小”，也就是对任意 $\tau>0$，事件 $\left\{ \dfrac{|X_i-\mu_i|}{B_n}>\tau \right\}$ 发生的概率尽可能小。
- 我们要求 $\lim\limits_{n\to\infty}P(\sup\limits_{1\le i\le n}|X_i-\mu_i|>\tau B_n)=0$。
- **Lindeberg 条件**：$\lim\limits_{n\to\infty}\dfrac{1}{\tau^2B_n^2}\sum\limits_{i=1}^n\begin{aligned} \int_{|x-\mu_i|>\tau B_n}(x-\mu_i)^2p_i(x){\text d}x=0 \end{aligned}$ 对任意 $\tau>0$成立，其中 $p_i(x)$ 是 $X_i$ 的密度函数（连续变量情形）。
- **Lindeberg 中心极限定理**：设独立随机变量序列 $\{X_n\}$ 满足 Lindeberg 条件，则对任意 $x$ 都满足 $\lim\limits_{n\to\infty}P\left( \dfrac1{B_n}\sum\limits_{i=1}^n(X_i-\mu_i)\le x \right)=\dfrac1{\sqrt{2\pi}}\begin{aligned} \int_{-\infty}^xe^{-\frac{t^2}2}{\text d}t \end{aligned}$。

Lyapnov 中心极限定理。设 $\{X_n\}$ 是独立随机变量序列，若存在 $\delta>0$ 使得 $\lim\limits_{n\to\infty}\dfrac1{B_n^{2+\delta}}\sum\limits_{i=1}^nE(|X_i-\mu_i|^{2+\delta})=0$，则对任意 $x$，满足 $\lim\limits_{n\to\infty}P\left( \dfrac1{B_n}\sum\limits_{i=1}^n(X_i-\mu_i)\le x \right)=\dfrac1{\sqrt{2\pi}}\begin{aligned} \int_{-\infty}^xe^{-\frac{t^2}2}{\text d}t \end{aligned}$。
