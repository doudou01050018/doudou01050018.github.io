---
publish: true
---

矩阵微分是推导各种机器学习公式的基础。在 ML 语境下，应当对矩阵微分作一些特化和简化处理。

# ML-ST-1：机器学习中的矩阵微分
> 符号约定：使用 $a, b, x$ 表示标量，使用 `\mathbf` 样式的粗体 $\mathbf x, \mathbf y, \mathbf z$ 表示向量，使用大写字母 $A, B, C$ 表示矩阵（未加粗）。
> 
> 如未说明，默认 $\mathbf x=(x_1, x_2, \cdots, x_n)$，$A=(a_{ij})$。
## $\mathrm I$ 矩阵微分入门
### $\it 1.1$ 矩阵微分定义(1)
通常需要掌握的是标量对向量、标量对矩阵的求导。设 $L$ 是损失函数（标量），我们需要求其对向量 $\mathbf x$ 的梯度。

Def. 定义**标量对向量**的导数：$\dfrac{\partial L}{\partial\mathbf x}=L'(\mathbf x):=\mathrm{grad}L=\left( \dfrac{\partial L}{\partial x_1}, \dfrac{\partial L}{\partial x_2}, \cdots, \dfrac{\partial L}{\partial x_n} \right)^T$，这正是 $L$ 对 $\mathbf x$ 的**梯度**。
- 注意：有关矩阵求导的格式，在机器学习中，默认使用**分母布局**，即**所得结果与分母的布局一致**（如上式中 $\partial\mathbf x$ 与 $\mathrm{grad}L$ 都是 $n\times 1$ 的列向量）。

Def. 定义**标量对矩阵**的导数：
$$\dfrac{\partial L}{\partial A}:=\begin{pmatrix} \dfrac{\partial L}{a_{11}} & \dfrac{\partial L}{a_{12}} & \cdots & \dfrac{\partial L}{\partial a_{1m}} \\ \dfrac{\partial L}{a_{21}} & \dfrac{\partial L}{a_{22}} & \cdots & \dfrac{\partial L}{\partial a_{2m}} \\ \vdots & \vdots & \ddots & \vdots \\ \dfrac{\partial L}{a_{n1}} & \dfrac{\partial L}{a_{n2}} & \cdots & \dfrac{\partial L}{\partial a_{nm}} \end{pmatrix}$$
其表示对每一个位置的偏导写为同布局矩阵的结果。其与标量对矩阵的导数是一致的，都是 $L$ 对每一个元素求导后写在一起（或者将矩阵展开为一个 $n\times m$ 长度的向量亦可）。

### $\it 1.2$ 用一阶微分理解矩阵微分
以 $L$ 对 $\mathbf x$ 的导数为例，核心思想是将式子写作 ${\text d}L=(\cdots){\text d}\mathbf x$ 的形式，则 $(\cdots)$ 的**转置**就是 $\dfrac{\partial L}{\partial\mathbf x}$。
- ${\text d}L=(L'(\mathbf x))^T{\text d}\mathbf x$，则 $\dfrac{\partial L}{\partial\mathbf x}=L'(\mathbf x)$；

（**运算法则**）矩阵微分的运算法则与单元函数在形式上一致：
- ${\text d}(A+B)={\text d}A+{\text d}B$；
- ${\text d}(AB)={\text d}A\cdot B+A\cdot{\text d}B$。

 这里 $A, B$ 均为任意大小的矩阵（只要满足运算规律即可）。

### $\it 1.3$ 矩阵微分定义(2)
Def. 定义**向量对向量**的导数：一般地，设 $\mathbf f(\mathbf x)$ 表示一个 $\mathbb R^n\to\mathbb R^m$ 的映射（即 $\mathbf x\in\mathbb R^n, \mathbf f\in\mathbb R^m$），具体可以写成 $\mathbf f(\mathbf x)=(f_1(\mathbf x), f_2(\mathbf x), \cdots, f_m(\mathbf x))^T$。那么 $\mathbf f$ 对 $\mathbf x$ 的导数定义为：
$$
\dfrac{\partial\mathbf f}{\partial\mathbf x}:=\begin{pmatrix} \dfrac{\partial f_1}{\partial\mathbf x^T} \\ \dfrac{\partial f_2}{\partial\mathbf x^T} \\ \vdots \\ \dfrac{\partial f_m}{\partial\mathbf x^T} \end{pmatrix}^T=\begin{pmatrix} \dfrac{\partial f_1}{\partial x_1} & \dfrac{\partial f_1}{\partial x_2} & \cdots & \dfrac{\partial f_1}{\partial x_n} \\ \dfrac{\partial f_2}{\partial x_1} & \dfrac{\partial f_2}{\partial x_2} & \cdots & \dfrac{\partial f_2}{\partial x_n} \\ \vdots & \vdots & \ddots & \vdots \\ \dfrac{\partial f_m}{\partial x_1} & \dfrac{\partial f_m}{\partial x_2} & \cdots & \dfrac{\partial f_m}{\partial x_n} \end{pmatrix}^T
$$
这被称为 **Jacobi 矩阵**，在换元中有重要作用。我们也将 $\dfrac{\partial\mathbf f}{\partial\mathbf x}$ 记作 $J\mathbf f(\mathbf x)$，其满足 ${\text d}\mathbf f=J\mathbf f^T\cdot{\text d}\mathbf x$（从这里看出 $J\mathbf f$ 是 $m\times n$ 矩阵）。
- 注：标准数学分析中 $J\mathbf f$ 是的分子布局（不加转置），这里加上 $(\cdots)^T$ 是为了转化为分母布局。

## $\mathrm{II}$ 矩阵微分的计算方法
### $\it 2.1$ 逐个展开
一个自然的思想就是，若矩阵形式的求导看起来陌生，那么就将向量和矩阵具体展开为单元变量，然后对每一个单元变量求偏导，这是我们熟悉的范围。
- 例如求 $\dfrac{\partial L}{\partial\mathbf x}$ 只需对 $\mathbf x$ 的每个分量 $x_i$ 求偏导，即求 $\dfrac{\partial L}{\partial x_1}, \dfrac{\partial L}{\partial x_2}, \cdots, \dfrac{\partial L}{\partial x_n}$，最后将其拼起来即可。
- 如对 $f=\mathbf x^T\mathbf x$ 求 $\dfrac{\partial f}{\partial\mathbf x}$，朴素做法即为写出 $f=\mathbf x^T\mathbf x=\sum\limits_{i=1}^nx_i^2$，这对每一个分量求导的结果为 $\dfrac{\partial f}{\partial x_i}=2x_i$，将求导结果拼到一起写为向量即得到 $\dfrac{\partial f}{\partial\mathbf x}=(2x_1, 2x_2, \cdots, 2x_n)^T=2\mathbf x$。
- 对 $f=\mathbf x^TA\mathbf x$ 这样的二次型求 $\dfrac{\partial f}{\partial\mathbf x}$ 使用朴素做法也是可行的，结果是 $(A+A^T)\mathbf x$（需要牢记！）

### $\it 2.2$ 微分方法
对于多元函数 $f$，若能将其写为 ${\text d}f=(\cdots){\text d}\mathbf x$，则括号里式子的**转置**即为所求 $\dfrac{{\text d}f}{{\text d}\mathbf x}$。于是只需要遵循微分法则运算即可。我们允许诸如 ${\text d}x, {\text d}\mathbf u$ 这样的微分算子独立存在，而之所以允许直接使用微分法则，是利用了**一阶微分的形式不变性**（不可用于更高阶的微分）。
- 例如线性函数 $f=\mathbf a^T\mathbf x$，其 $\dfrac{\partial f}{\partial\mathbf x}=\mathbf a$。
- 例如二次型 $f=\mathbf x^TA\mathbf x$，对两侧取微分得到 ${\text d}f={\text d}(\mathbf x^TA\mathbf x)={\text d}\mathbf x^TA\mathbf x+\mathbf x^T{\text d}(A\mathbf x)={\text d}\mathbf x^TA\mathbf x+\mathbf x^TA{\text d}\mathbf x$。考虑到 ${\text d}\mathbf x^TA\mathbf x$ 为标量，可以将其取转置，于是 ${\text d}f=({\text d}\mathbf x^TA\mathbf x)+\mathbf x^TA{\text d}\mathbf x=\mathbf x^T(A+A^T){\text d}\mathbf x$。所求即为 $(A+A^T)\mathbf x$（注意转置）。
	- 二次型中 $A$ 通常为实对称矩阵，因此所求可以化简为 $2A\mathbf x$。

### $\it 2.3$ 矩阵内积与 Trace 方法
先复习 trace 的相关性质：
- Def. **定义方阵的迹 trace** 为 $\mathrm{tr}(A):=\sum\limits_{i=1}^na_{ii}$。也就是对角线元素之和。
- 基本性质：
	- $\mathrm{tr}(A+B)=\mathrm{tr}(A)+\mathrm{tr}(B)$；
	- $\mathrm{tr}(\lambda A)=\lambda\mathrm{tr}(A)$；
	- $\mathrm{tr}(A)=\mathrm{tr}(A^T)$。
- 重要性质：$\mathrm{tr}(AB)=\mathrm{tr}(BA)$，也就是 trace 是一个可交换的量，这一点在 ML 中通常写作 $\mathrm{tr}(ABC)=\mathrm{tr}(BCA)=\mathrm{tr}(CAB)$（**循环置换**）。
	- 本质上 trace 对应特征多项式里 $\lambda^{n-1}$ 的系数。

Def. **定义矩阵的内积为** $\langle A, B\rangle:=\sum\limits_{i=1}^n\sum\limits_{j=1}^ma_{ij}b_{ij}=\mathrm{tr}(A^TB)=\mathrm{tr}(B^TA)$。
- 实际上矩阵的内积就相当于两个矩阵对应位置元素的乘积之和，这一点跟向量内积是一致的。
- 关键性质：在 $L=L(A)$ 这类矩阵函数中，${\text d}L=\sum\limits_{i=1}^n\sum\limits_{j=1}^m\dfrac{\partial L}{\partial a_{ij}}{\text d}a_{ij}=\left\langle\dfrac{\partial L}{\partial A}, {\text d}A\right\rangle=\mathrm{tr}\left( \left(\dfrac{\partial L}{\partial A}\right)^T{\text d}A \right)$。也就是，如果能将 ${\text d}L$ 写成 $\mathrm{tr}(B^T{\text d}A)$ 的形式，那么 $B$ 就是所求的 $\dfrac{\partial L}{\partial A}$。

关键性质：
- ${\text d}(\mathrm{tr}A)=\mathrm{tr}({\text d}A)$。微分算符可以穿透 $\mathrm{tr}$。
	- 证明：${\text d}(\mathrm{tr}A)={\text d}\left( \sum\limits_{i=1}^na_{ii} \right)=\sum\limits_{i=1}^n{\text d}a_{ii}=\mathrm{tr}({\text d}A)$。

### $\it 2.4$  换元方法与链式法则
在二元微积分中，复合函数求导形如：
- $f(u, v)$ 是二元函数，其中 $u=u(x, y), v=v(x, y)$，求 $\dfrac{\partial f}{\partial x}$。结果是 $\dfrac{\partial f}{\partial x}=\dfrac{\partial f}{\partial u}\dfrac{\partial u}{\partial x}+\dfrac{\partial f}{\partial v}\dfrac{\partial v}{\partial x}$。
- 或者写成矩阵的形式：$\begin{pmatrix} \dfrac{\partial f}{\partial x} & \dfrac{\partial f}{\partial y} \end{pmatrix}=\begin{pmatrix} \dfrac{\partial f}{\partial u} & \dfrac{\partial f}{\partial v} \end{pmatrix}\begin{pmatrix} \dfrac{\partial u}{\partial x} & \dfrac{\partial u}{\partial y} \\ \dfrac{\partial v}{\partial x} & \dfrac{\partial v}{\partial y} \end{pmatrix}$。

这一形式完全可以扩展到 $n$ 元。对于复合函数 $\mathbf y=\mathbf f(\mathbf x), \mathbf z=\mathbf g(\mathbf y)$（注意这里都是向量函数），定义复合运算 $\mathbf h(\mathbf x)=\mathbf g\circ\mathbf f(\mathbf x)=\mathbf g(\mathbf f(\mathbf x))$，那么其导数（Jacobi 矩阵）满足：$J\mathbf h(\mathbf x)=J\mathbf g(\mathbf y)\cdot J\mathbf f(\mathbf x)$。
- 特别的，若 $\mathbf z$ 退化为标量（例如损失函数 $L$），此时要求 $L(\mathbf y)=L(\mathbf f(\mathbf x))$ 对 $\mathbf x$ 的导数（梯度），那么有 $\left(\dfrac{\partial L}{\partial\mathbf x}\right)^T=\left(\dfrac{\partial L}{\partial\mathbf y}\right)^T\left( \dfrac{\partial \mathbf y}{\partial\mathbf x} \right)^T$（注意这里依旧是分母布局，加上转置变为分子布局更符合自然表述 $\dfrac{{\text d}z}{{\text d}x}=\dfrac{{\text d}z}{{\text d}y}\dfrac{{\text d}y}{{\text d}x}$）。

### $\it 2.5$ 逐元素作用与 Hadamard 积
Def. **定义逐元素乘积** $A, B\in\mathbf M_{n\times m}$，则 $A\odot B:=\begin{pmatrix} a_{11}b_{11} & a_{12}b_{12} & \cdots & a_{1m}b_{1m} \\ a_{21}b_{21} & a_{22}b_{22} & \cdots & a_{2m}b_{2m} \\ \vdots & \vdots & \ddots & \vdots \\ a_{n1}b_{n1} & a_{n2}b_{n2} & \cdots & a_{nm}b_{nm} \end{pmatrix}$。
- 也就是每一个位置的元素独立相乘。注意 $A, B$ 都是 $n\times m$ 矩阵，逐元素乘积又称 Hadamard 积，其要求矩阵布局是**一致的**，而非矩阵乘法要求的 $n\times m, m\times p$ 才能相乘。

（**逐元素作用**）在 ML 中经常遇到一类逐元素作用的函数，如 $\sigma(\mathbf x):=(\sigma(x_1), \sigma(x_2), \cdots, \sigma(x_n))^T$。这类函数在参与微分的时候有一定的特殊形式：${\text d}\sigma(A)=\sigma'(A)\odot{\text d}A$。
- 特别的，若 $A$ 退化为向量 $\mathbf x$，有一类特殊的技巧：$\sigma'(\mathbf x)=\mathrm{diag}(\sigma'(x_1), \sigma'(x_2), \cdots, \sigma'(x_n))$。因此 $\sigma'(\mathbf x)\odot{\text d}\mathbf x=\mathrm{diag}(\sigma'(\mathbf x)){\text d}\mathbf x$。
- 逐元素作用的一类微分式子：$\mathrm{tr}(A^T(B\odot C))=\langle A, B\odot C\rangle=\langle A\odot B, C\rangle=\mathrm{tr}((A\odot B)^TC)$。可以发现，逐元素乘积跟矩阵内积（以及向量内积）的逻辑是一致的。

### $\it 2.6$ 其他运算方法
有关**行列式** $\det A$ 的微分：
- ${\text d}(\det A)={\text d}\left( \sum\limits_{j=1}^na_{1j}A_{1j} \right)$，因此 $\dfrac{\partial(\det A)}{\partial a_{11}}=A_{11}$。这里 $A_{ij}$ 为**代数余子式**。类似的有 $\dfrac{\partial(\det A)}{\partial a_{ij}}=A_{ij}$。因此整体有 $\dfrac{\partial(\det A)}{\partial A}=(A^*)^T=\det A\cdot(A^{-1})^T$（若逆矩阵存在），指**伴随矩阵**的**转置**。
	- 因此我们在已知 $\dfrac{\partial(\det A)}{\partial A}=\det A\cdot(A^{-1})^T$ 的情况下，代入 ${\text d}(\det A)=\mathrm{tr}(G^T{\text d}A)$，得到 ${\text d}(\det A)=\mathrm{tr}((\det A\cdot(A^{-1})^T)^T{\text d}A=\det A\cdot\mathrm{tr}(A^{-1}{\text d}A)$。这是一个有关 ${\text d}(\det A)$ 的重要化简式子。

有关**矩阵逆** $A^{-1}$ 的微分：
- 从 $AA^{-1}=I$ 出发，两侧取微分得 ${\text d}A\cdot A^{-1}+A\cdot{\text d}(A^{-1})=0$，即 ${\text d}(A^{-1})=-A^{-1}({\text d}A)A^{-1}$。

### $\it 2.7$ 矩阵指数
暂时碰不到，先记一个定义：$e^A:=\sum\limits_{k=0}^\infty\dfrac{A^k}{k!}$，与一般实数或者复数的指数定义一致。
## $\mathrm{III}$ 用矩阵微分分析多元函数
### $\it 3.1$ 梯度为零是否等价于最小值点
在一元函数中，$f'(x)=0$ 通常意味着极大值或极小值（或者鞍点），但在高维函数中，$f'(\mathbf x)=0$ 在绝大部分情形下都是**鞍点**（即部分方向为正，部分方向为负）。只有极小部分情况，各特征方向都为正或都为负，此时才为极值点。
- 一维情况下，若 $f(x)$ 在 $x_0$ 处取到极小值，且 $f(x)$ 在 $x_0$ 处可微，那么一定有 $f'(x_0)=0$。这个结论完全可以拓展到高维：若 $f(\mathbf x)$ 在 $\mathbf x_0$ 处取到极小值，且 $f(\mathbf x)$ 在 $\mathbf x_0$ **可微**（指存在切平面，以及能够写成线性主部），则一定有 $f'(\mathbf x_0)=\mathbf 0$。
- 在已知梯度的情况下，可以求任意的方向导数：若 $\mathbf n$ 是单位向量，则方向导数定义为 $\dfrac{\partial f}{\partial\mathbf n}:=\lim\limits_{h\to 0}\dfrac{f(\mathbf x_0+h\mathbf n)-f(\mathbf x_0)}{h}$。其值为 $[f'(\mathbf x_0)]^T\mathbf n$。
### $\it 3.2$ Hesse 矩阵与多元函数的凹凸性
为了判定驻点是否为极值点，引入二阶导判定凹凸性。凸性在几何上相当于“碗底”或者“山峰”，在这样的函数中作梯度下降往往能够顺利达到“碗底”。

Def. **定义 Hesse 矩阵**如下：
$$
H_f(\mathbf  x):=\begin{pmatrix} \dfrac{\partial^2f}{\partial x_1\partial x_1} & \dfrac{\partial^2f}{\partial x_1\partial x_2} & \cdots & \dfrac{\partial^2f}{\partial x_1\partial x_n} \\ \dfrac{\partial^2f}{\partial x_2\partial x_1} & \dfrac{\partial^2f}{\partial x_2\partial x_2} & \cdots & \dfrac{\partial^2f}{\partial x_2\partial x_n} \\ \vdots & \vdots & \ddots & \vdots \\ \dfrac{\partial^2f}{\partial x_n\partial x_1} & \dfrac{\partial^2f}{\partial x_n\partial x_2} & \cdots & \dfrac{\partial^2f}{\partial x_n\partial x_n} \end{pmatrix}
$$

若 $f(\mathbf x)$ 足够连续，那么多次求导的顺序不影响结果（求导次序可交换性）。即为 $f_{xy}=f_{yx}$，此时 $H_f$ 为**实对称矩阵**。

给出任意方向的二阶导的公式：$\dfrac{\partial^2f}{\partial\mathbf n^2}=\mathbf n^TH_f\mathbf n$。其中 $\mathbf n$ 为代指方向的单位向量。
- 可以看出，单位向量 $\mathbf n$ 方向的二阶导数就是 $H_f$ 的二次型。若 $H_f$ 本身是正定的，也就是说**任意方向的二阶导都为正**，此时 $\mathbf x_0$ 为极小值点（在邻域内最小）。若 $H_f$ 负定，则 $\mathbf x_0$ 为极大值点。
	- 注意：若 $H_f$ 不定，那么仍然可以分析。注意 $H_f$ 是实对称矩阵，那么 $H_f$ 可以分解为 $n$ 个独立的方向，也就是存在正交矩阵 $Q$ 使得 $Q^TH_fQ=\mathrm{diag}(\lambda_1, \lambda_2, \cdots, \lambda_n)$。此时有 $p$ 个方向的二阶导为正，$q$ 个方向的二阶导为负。为正的方向与为负的方向构成了整体空间的一个线性划分。

### $\it 3.3$ 凸函数是否存在全局最小
### $\it 3.4$ 多元 Taylor 展开
$f(\mathbf x+{\text d}\mathbf x)=\sum\limits_{k=0}^\infty\dfrac1{k!}\left( \dfrac{\partial}{\partial x_1}{\text d}x_1+\dfrac{\partial}{\partial x_2}{\text d}x_2+\cdots+\dfrac{\partial}{\partial x_n}{\text d}x_n \right)^kf$。
- 其中零阶项为 $f(\mathbf x)$，一阶项为 $[f'(\mathbf x)]^T{\text d}\mathbf x$，二阶项为 $\dfrac12{\text d}\mathbf x^TH_f{\text d}\mathbf x$。更高阶的项需要写作更高阶的张量。
### $\it 3.5$ 一阶与二阶的优化方法

## $\mathrm{IV}$ 常用矩阵微分公式例子
