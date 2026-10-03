---
publish: true
---

# ML-2 逻辑回归
## $\mathrm I$ 问题分析
### $\it 1.1$ 问题描述
输入数据 $\mathbf x\in\mathbb R^D$，我们希望使用 ML 将不同的 $\mathbf x$ 分类（二分类），也就希望训练一个由 $\mathbb R^d$ 到 $\{0, 1\}$ 的映射 $y(\mathbf x):\mathbb R^{D}\to\{0, 1\}$。
- 给定大小为 $n$ 的训练集 $(\mathbf x_i, y_i)$，其中 $\mathbf x_i\in\mathbb R^n$，$y_i\in\{0, 1\}$。
- 采用线性模型，即 $f(\mathbf x)=\mathbf w_0^T\mathbf x+b$，于是我们的模型可以描述为 $\mathbf w=\begin{pmatrix} \mathbf w_0 \\ b \end{pmatrix}\in\mathbb R^{D+1}$。
### $\it 1.2$ 概率判别方法 Soft Prediction
考虑采用激活函数 $\sigma(x)=\dfrac1{1+e^{-x}}\in(0, 1)$ 来模拟概率，也就是定义 $P(y(\mathbf x)=1\mid\mathbf w):=\sigma(f(\mathbf x))$。
- 分析：$\sigma(f(\mathbf x))$ 可以想象为对一个超平面 $\Sigma$ 的距离应用 $\sigma$，离得越远那么 $\sigma(f(\mathbf x))$ 的值就越清晰（或者越极端，总之靠近 $0$ 或 $1$），离得越近那么概率表述就越模糊（接近 $0.5$）。

### $\it 1.3$ 应用 MLE（最大似然估计）
在应用 Soft Prediction 的情形下，要训练模型 $\mathbf w=\begin{pmatrix} \mathbf w_0 \\ b \end{pmatrix}$，我们首先应当构建损失函数，即转化为函数优化问题。应用 **MLE 方法**（在只知道模型 $\mathbf w$ 以及给定数据 $\mathbf x_i$ 的情况下，对 $y(\mathbf x_i)$ 进行**预估**，得到的预估被称为**似然**，我们的任务是寻找使得实际情况 $(y_1, y_2, \cdots, y_n)$ 发生概率最大的参数 $\mathbf w$）：
- 我们当前的信念（模型）为 $\mathbf w$。
- 以当前模型为基准，我们要观测的事件描述为 $X:=\bigcap\limits_{i=1}^n\{y(\mathbf x_i)=y_i\}$。
- 根据 MLE，寻找最大化 $P(X\mid\mathbf w)$ 的 $\mathbf w$。
	- 至此我们有了优化目标，接下来就是对这个函数进行优化。

$$
\begin{aligned} P\left( \bigcap\limits_{i=1}^n\{y(\mathbf x_i)=y_i\}\mid\mathbf w \right) &= \prod\limits_{i=1}^nP(y(\mathbf x_i)=y_i\mid\mathbf w) \qquad\text{(默认独立同分布)} \\ &= \prod\limits_{i=1}^n[\sigma(f(\mathbf x_i))]^{y_i}[1-\sigma(f(\mathbf x_i))]^{1-y_i} \end{aligned}
$$

取对数：
$$
\begin{aligned} L(\mathbf  w) &= -\ln P(X\mid\mathbf w) \\&= -\sum\limits_{i=1}^ny_i\ln\sigma(f(\mathbf x_i))+(1-y_i)\ln(1-\sigma(f(\mathbf x_i))) \end{aligned}
$$
（这被称为交叉熵损失 Cross Entropy Loss / CELoss，是信息论的内容）

### $\it 1.4$ 熵
**熵 Entropy**用于描述“混乱程度”，对于随机变量 $Y$，定义其熵为 $H(Y):=-\sum\limits_{i=1}^nP(y_i)\ln P(y_i)$（连续情形改为积分即可）。
- 交叉熵涉及两个分布 $P$ 和 $Q$，其中 $P$ 为实际分布，$Q$ 为预测分布，其度量的是用分布 $Q$ 编码分布 $P$ 所需的平均信息量，写作 $H(P, Q):=-\sum\limits_{i=1}^np(x_i)\ln q(x_i)$。

## $\mathrm{II}$ 线性可分数据与过拟合现象
### $\it 2.1$ 对 MLE 化简并求导
为了统一表示参数，令 $\mathbf x_i':=\begin{pmatrix} \mathbf x_i \\ 1 \end{pmatrix}$，则 $f(\mathbf x_i)=\mathbf w^T\mathbf x_i'$。

化简：
$$
\begin{aligned} L(\mathbf w) &= -\sum\limits_{i=1}^ny_i\ln\dfrac1{1+e^{-\mathbf w^T\mathbf x_i'}}+(1-y_i)\ln\dfrac1{1+e^{\mathbf w^T\mathbf x_i'}} \\ &= -\sum\limits_{i=1}^n\left(-y_i\ln(1+e^{-\mathbf w\mathbf x_i'})+(y_i-1)\ln(1+e^{\mathbf w^T\mathbf x_i'})\right) \\ &=-\sum\limits_{i=1}^n\left[y_i\mathbf w^T\mathbf x_i'-\ln(1+e^{\mathbf w^T\mathbf x_i'})\right] \end{aligned}
$$
将 $L$ 对 $\mathbf w$ 求导：
$$
\begin{aligned} \dfrac{\partial L}{\partial\mathbf w} &= -\sum\limits_{i=1}^n\left( y_i\mathbf x_i'-\dfrac{e^{\mathbf w^T\mathbf x_i'}\cdot\mathbf x_i'}{1+e^{\mathbf w^T\mathbf x_i'}} \right) \\ &= -\sum\limits_{i=1}^n(y_i-\sigma(f(\mathbf x_i)))\mathbf x_i' \\ &= -\sum\limits_{i=1}^n(y_i-P(y(\mathbf x_i)=1))\mathbf x_i' \end{aligned}
$$

### $\it 2.2$ 线性可分性分析与过拟合
如果我们希望 $\displaystyle \frac{\partial L}{\partial\mathbf w} = 0$，则说明 $i=1, 2, \cdots, n$ 有 $y_i - \sigma(f(\mathbf x_i)) = 0$。但是这种情况通常是不可能出现的。

除非训练数据**线性可分**（linearly separatable）。事实上线性可分的情况通常并不是我们希望的。因为对于分隔超平面 $\mathbf{w}^T\mathbf{x} + b = 0$，如果同时给 $\mathbf{w}$ 和 $b$ 乘上系数 $k > 0$，则超平面在几何上是不变的（而且数据点到超平面的距离也是不变的），但是这会影响模型的预测值——因为 $\mathbf{w}^T\mathbf{x} + b$ 处在 $e$ 的指数位上，所以这会使得 $\sigma(\mathbf{w}^T\mathbf{x}+b)\to 0$ 或 $1$，即往使损失函数减小的方向上持续更新，进而导致 $k \to \infty$，模型越来越 sharp，造成**过拟合**（overfitting）。
- 在这种情况下（线性可分），就像 $e^x$ 在 $x\to-\infty$ 的时候，会造成“永远下降”但无法结束的结果。

如何避免这种情况？加一个 L2-Norm 就好了（凸且保证有最小值点）。
- 疑问：为什么线性不可分就可以保证存在最小值点？因为此时不存在一个超平面将点完全分开，任何模型一定会存在与预测不一致的点，这会导致在 $L$ 中出现“阻力”，阻止 $\|\mathbf w\|$无限变大。
### $\it 2.3$ CELoss 的凹凸性
分析 $L$ 关于 $\mathbf w$ 的 **Hesse 矩阵**：
$$
\begin{aligned} H&=\dfrac{\partial}{\partial\mathbf w}\left( \dfrac{\partial L}{\partial\mathbf w} \right) \\ &= \sum\limits_{i=1}^nP(y(\mathbf x_i)=1)P(y(\mathbf x_i)=0)\mathbf x_i'(\mathbf x_i')^T \end{aligned}
$$
- 其中 $\mathbf x\mathbf x^T$ 半正定，因此 Hesse 矩阵半正定，这是一个凸函数。
	- 证明：$\forall\mathbf v$ 有 $\mathbf v^T\mathbf x\mathbf x^T\mathbf v=(\mathbf x^T\mathbf v)^T\mathbf x^T\mathbf v=\|\mathbf x^T\mathbf v\|\ge0$。
	- **但是**：函数为凸不代表存在最小值点，最小值点需要一阶导数为零加上 Hesse 矩阵为正定才能判定。
### $\it 2.4$ 为什么不能用平方损失函数
为什么不能用 $L=\sum\limits_{i=1}^n(y_i-f(\mathbf x_i))^2$？显然不行：
- 分类标签的 $y_i$ **没有数值上的意义**，$\{0,1\}$ 换成 $\{1,-1\}$ 是一样的。比如说 $y_i = 1$，$f(\mathbf{x}_i) = 0.8$ 或 $1.2$，损失函数值都是一样的，失去了概率意义。
- $f(\mathbf{x}_i) \in \mathbb{R}$，值域与 $\{0,1\}$ 是不匹配的。
- 当 $y_i = 0$ 且 $f(\mathbf{x}_i) = 1$ 的时候，损失函数值仅仅为 $1$，反观如果使用 CELoss，$\sigma(f(\mathbf{x}_i)) \to 1$ 的时候，造成的损失为 $-(1 - y_i) \log \sigma(-f(\mathbf{x}_i)) \to +\infty$。
- 对离群值不健壮（not robust to **outlier**）：对于一个和分隔超平面很远的 outlier，用平方损失的话会造成把分隔平面往 outlier 方向拉的情况。

> 其实说了半天，最终道理还是因为分类标签本身是不具有数值意义的，所以不能用平方损失。如果使用平方损失的话会造成很多问题。

## $\mathrm{III}$ 多分类问题
### $\it 3.1$ Softmax 模拟概率
该模型用于处理多分类问题。给定 $(\mathbf x_i, y_i)$，其中 $y_i\in\{1, 2, \cdots, K\}$。希望训练一个能将输入的 $\mathbf x$ 归类到 $1, 2, \cdots, K$ 中（不可以使用平方损失来做）。
- 有 K 个分类函数，$f_k(\mathbf{x}) = \mathbf{w}_{k, 0}^T\mathbf{x} + b_k=\mathbf w_k^T\mathbf x$，$k\in [K]$。

每个分类函数输出的是对相应类别的一个打分，如何归约到概率上？我们有 Softmax：

$$
P(y=k\mid \mathbf{x}) = \frac{\exp(\mathbf{w}_k^T\mathbf{x})}{\sum_{i=1}^K\exp(\mathbf  w_i^T\mathbf x)}
$$

性质：
1. 这是一个合法的概率分布，因为 $\sum\limits_{k=1}^KP(\mathbf y=k\mid\mathbf x)=1$，且 $P(y=k\mid \mathbf{x})\ge 0$。
2. 若 $f_k(\mathbf x)\gg f_j(\mathbf x),\forall j\neq k$，则 $P(y=k\mid \mathbf{x}) \approx 1$，且 $P(y=j\mid \mathbf{x}) \approx 0$  
   这是指数函数的**放大效应**。

考虑使用 MLE 来优化，写出 log-likelihood：

$$
\sum_{i \in [n]} \log \frac{\exp(\mathbf{w}_{y_i}^T\mathbf{x}_i + b_{y_i})}{\sum_{j \in [K]}\exp(\mathbf{w}_j^T \mathbf{x}_i + b_j)}
$$

实际上，不一定要 $k$ 个线性分类器，其实可以用一整个神经网络，然后最后使用 softmax。

问题：$\color{Red}\textbf{为什么是 softmax？？}$ 
### $\it 3.2$ 与逻辑回归的等价性
$K=2$ 时，Softmax 与逻辑回归等价。

$K=2$ 的时候，假设 $1$ 为正类，$2$ 为负类，则
 $$
 \begin{aligned}
 P(y=1\mid \mathbf{x}) &= \frac{\exp(\mathbf{w}_1^T\mathbf{x} + b_1)}{\exp(\mathbf{w}_1^T\mathbf{x} + b_1) + \exp(\mathbf{w}_2^T\mathbf{x} + b_2)} \\
 &= \frac{1}{1 + \exp((\mathbf{w}_2-\mathbf{w}_1)^T\mathbf{x} + (b_2-b_1))}\\
 &\text{let } b = b_1-b_2, \mathbf{w}= \mathbf{w}_1-\mathbf{w}_2,\\
 &= \frac{1}{1+\exp(\mathbf{w}^T\mathbf{x}+b)} = \sigma(\mathbf{w}^T\mathbf{x}+b)
 \end{aligned}
 $$
 这等价于逻辑回归。

## $\mathrm{IV}$ 几个统计估计方法的应用
### $\it 4.1$ MLE 方法解释线性回归
即用 MLE 方法推导为什么线性回归的损失是平方：
- 考虑线性模型 $f(\mathbf x)=\mathbf w_0^T\mathbf x+b=\mathbf w^T\mathbf x'$，这里 $\mathbf w=\begin{pmatrix} \mathbf w_0 \\ b \end{pmatrix}, \mathbf x'=\begin{pmatrix} \mathbf x \\ 1 \end{pmatrix}$。
- 根据**中心极限定理**，我们当前要用模型 $\mathbf w$ 估计 $y_i$，假设 $\hat y_i=f(\mathbf x_i)+\varepsilon_i$（这里 $\hat y_i$ 表示估计值），其中 $\varepsilon_i$ 是**大量微小扰动**的反应，因此认为 $\varepsilon_i\sim\mathcal N(0, \sigma^2)$。而实际的偏差记作 $y_i-f(\mathbf x_i)=\delta_i$。
- 考虑使用概率密度函数代替概率密度（实际上就是少了 ${\text d}x^n$），写出 MLE：$\prod\limits_{i=1}^n\dfrac{{\text d}P(\varepsilon_i=\delta_i)}{{\text d}\varepsilon_i}$。
- 引入标准正态分布函数 $\phi(x)=\dfrac1{\sqrt{2\pi}}e^{-\frac12x^2}$ 改写式子为 $\prod\limits_{i=1}^n\phi\left( \dfrac{\delta_i}{\sigma} \right)$。这个式子是 $\mathbf w$ 的函数，我们希望这个似然最大化。
- （经过化简）这等价于求 $\min\sum\limits_{i=1}^n\delta_i^2$。这就是最小二乘法中平方损失的 MLE 来源。
### $\it 4.2$ MAP 最大后验方法推导 L2-Norm
问题描述与线性回归一致，我们希望用 MAP 方法，通过 $\mathbf w$ 的先验（正态）推出损失函数 $L=\sum\limits_{i=1}^n(y_i-\mathbf w^T\mathbf x_i)^2+\lambda\|\mathbf w\|$。
- 我们的先验是正态，即 $\mathbf w\sim\mathrm N(\mathbf 0, \sigma_w^2I)$。这里 $\sigma_w^2I$ 是协方差矩阵（对角矩阵，代表各个分量独立）。根据 MAP 方法，我们希望求 $\arg\max\limits_{\theta}P(\theta\mid X)$，这里 $\theta$ 即为我们的信念（模型）$\mathbf w$，事件 $X$ 代表“模型预测准确”。即最大化 $P(X\mid\theta)P(\theta)$。
- $P(X\mid\theta)$ 部分：设模型为 $\hat y_i=\mathbf w^T\mathbf x+\varepsilon_i$，其中 $\varepsilon_i\sim\mathrm N(0, \sigma^2)$ 为未知量（各个 $\varepsilon_i$ 独立）。那么预测准确（$y_i=\hat y_i$）的概率密度为 $\prod\limits_{i=1}^n\phi\left( \dfrac{y_i-\mathbf w^T\mathbf x_i}{\sigma} \right)$。
- $P(\theta)$ 部分：依旧按照概率密度计算，$\prod\limits_{i=1}^n\phi\left( \dfrac{w_i}{\sigma_w} \right)$。
- 于是优化目标是最大化 $\prod\limits_{i=1}^n\phi\left( \dfrac{y_i-\mathbf w^T\mathbf x_i}{\sigma} \right)\phi\left( \dfrac{w_i}{\sigma_w} \right)$。取对数得到 $L=\dfrac1{\sigma^2}\sum\limits_{i=1}^n(y_i-\mathbf w^T\mathbf x_i)^2+\dfrac1{\sigma_w^2}\|\mathbf w\|^2$（最小化的损失函数）。这正是 L2-正则化。
	- **注**：若使用 Laplace 先验代替正态先验，则得到的是 L1-正则化。
	- **注**：MLE 可以看做是均匀先验情况下（各个 $P(\theta)$ 相等）的 MAP。


