---
publish: true
---

# ML-1 线性回归
## $\mathrm{I}$ 线性回归的问题形式
给定一组数据 $D=(\mathbf x_1, y_1), (\mathbf x_2, y_2), \cdots, (\mathbf x_n, y_n)$，其中 $\mathbf x_i\in\mathbb R^d, y_i\in\mathbb R$。
### $\it 1.1$ 问题的基本定义
基本数据：
- $D = \{(\mathbf{x}_i, y_i)\}$ 为训练集，其中 $\mathbf{x}_i \in \mathbb{R}^d, y \in \mathbb{R}$；
- 使用线性模型进行拟合：$f(\mathbf{x}) = \mathbf{w}_0^T\mathbf{x} + b$，其中 $\mathbf{w}\in \mathbb{R}^d, b \in \mathbb{R}$，分别称为权重（weight）和偏置（bias）。
- 模型训练的原则：经验风险最小化（ERM, empirical risk minimization）。在做的事情就是最小化训练数据集上的损失。

定义损失函数（loss function）$L(\mathbf{w}_0, b)$，在线性回归中我们使用平方损失（squared loss），表达式为：$\min_{\mathbf{w}, b} \frac{1}{n}\sum_{i \in [n]}(y_i - f(\mathbf{x}_i))^{2}$。

为了使这个表达式达到最小值，我们对其求梯度（gradient）：
$$
\begin{aligned}
\frac{\partial L(\mathbf{w}, b)}{\partial \mathbf{w}} &= -\sum_{i \in [n]}2(y_i - \mathbf{w}^T\mathbf{x}_i - b)\cdot \frac{\partial (\mathbf{w}^T\mathbf{x}_i)}{\partial \mathbf{w}} \\
&= -\sum_{i \in [n]}2\mathbf{x}_i(y_i - \mathbf{w}^T\mathbf{x}_i - b) \\
\frac{\partial L(\mathbf{w}, b)}{\partial b} &= -\sum_{i \in [n]}2(y_i - \mathbf{w}^T\mathbf{x}_i - b)
\end{aligned}
$$
然后利用**梯度下降法**（gradient descent, GD）对 $\mathbf{w}$ 和 $b$ 的值进行更新，具体地：

$$
\begin{aligned}
\mathbf{w}' &\gets \mathbf{w} - \alpha \cdot \frac{\partial L}{\partial \mathbf{w}} \\
b' &\gets b - \alpha \cdot \frac{\partial L}{\partial b}
\end{aligned}
$$

其中 $\alpha$ 为**学习率**（learning rate, LR），是预先指定的**超参数**（hyperparameter），代表 $\mathbf{w}, b$ 每次往梯度方向走的“步长”。

### $\it 1.2$ 闭式解讨论

以上属于是数值方法，但对于线性回归而言，其是有**闭式解**（closed-form solution）的，我们就没有必要使用数值方法（其具有一定的随机性，且有不可避免的误差，此处不过多讨论）

为了方便，我们令 $\displaystyle X:=\begin{bmatrix} \mathbf{x}_1^T & 1 \\ \vdots  & \vdots \\ \mathbf{x}_n^T & 1 \end{bmatrix} \in \mathbb{R}^{n\times (d+1)}$，$\mathbf{\hat{w}} := \begin{bmatrix} \mathbf{w} \\ b \\\end{bmatrix} \in \mathbb{R}^{d+1}$，$\mathbf{y}:= \begin{bmatrix} y_1 \\ \vdots \\ y_n \\\end{bmatrix} \in \mathbb{R}^n$，这样可以把参数都放进一个 $\mathbf{\hat{w}}$ 里面，我们需要优化的目标就可以变成 $\left\| \mathbf{y} - X \mathbf{\hat{w}} \right\|^2$，为了对其求梯度，我们将其写成如下形式：

$$
\begin{aligned}
L(\mathbf{\hat{w}}) &= (\mathbf{y} - X \mathbf{\hat{w}})^T(\mathbf{y} - X \mathbf{\hat{w}}) \\
&= \mathbf{y}^T\mathbf{y} - \mathbf{y}^T X \mathbf{\hat{w}} - \mathbf{\hat{w}}^T X^T \mathbf{y} + \mathbf{\hat{w}}^T X^T X \mathbf{\hat{w}}\\
&= \mathbf{y}^T\mathbf{y} - 2\mathbf{y}^T X \mathbf{\hat{w}} + \mathbf{\hat{w}}^T X^T X \mathbf{\hat{w}}\\
\frac{\partial L(\mathbf{\hat{w}})}{\partial \mathbf{\hat{w}}} &= -2 X^T \mathbf{y} + 2 X^T X \mathbf{\hat{w}}\\
&= -2 X^T(\mathbf{y} - X \mathbf{\hat{w}})
\end{aligned}
$$

令 $\displaystyle \frac{\partial L}{\partial \mathbf{\hat{w}}} = 0$ 可以知道 $X^T\mathbf{y} = X^T X \mathbf{\hat{w}}$。解的情况需要取决于 $X^T X$ 的可逆性。
- 若 $X^T X$ 可逆，则 $\mathbf{\hat{w}} = (X^T X)^{-1} X^T\mathbf{y}$，这是最简单的情况。
- 若 $X^T X$ 不可逆，则 $X^T X$ 为奇异阵，一般有两种原因：
  - $d+1>n$，直觉上来看就是**数据点太少**，有不等式 $\operatorname{rank}(X^T X) = \operatorname{rank}(X) \le \min(n, d+1) = n < d + 1$。而 $X^T X \in \mathbb{R}^{(d+1)\times (d+1)}$，所以不可逆。这种情况一般比较罕见。
  - $d+1\le n$，直觉上来看是**有多余的特征维度**，$X$ 中有重复的列，导致不满秩。

而不论如何，若 $X^T X$ 不可逆，都意味着有多组 $\mathbf{\hat{w}}$ 的可行解。下面证明解一定存在：

假设其无解，说明 $\operatorname{rank}(X^T X) < \operatorname{rank}(X^T X \mid X^T\mathbf{y})$，但这种情况显然不可能发生，于是线性回归的 $\mathbf{\hat{w}}$ **要么解唯一，要么有无穷解**。*事实上，无穷的情况会比较不好处理*。

省流：这不就是投影吗。要最小化 $\|\mathbf y-X\hat{\mathbf w}\|^2$，相当于 $X\hat{\mathbf w}\in\mathrm{Col}(X)$，然后可以直接用投影公式 $\mathbf y_{\perp}=A(A^TA)^{-1}A^T\mathbf y$。

## $\mathrm{II}$ 正则化讨论
### $\it 2.1$ 岭回归（Ridge Regression）：L2-范数

采用了 L2-正则化的线性回归称为岭回归。L2-正则化的意思是在损失函数后面追加一个正则化项 $\lambda \cdot \left\| \mathbf{\hat{w}} \right\|^2$（L2-范数），以惩罚权重过大的模型。于是现在的损失函数为：$L=$

令其梯度为 $0$ 尝试推导闭式解：

$$
\begin{aligned}
-2X^T(\mathbf{y} - X \mathbf{\hat{w}}) + 2 \lambda \mathbf{\hat{w}} &= 0 \\
X^T\mathbf{y} - X^T X \mathbf{\hat{w}} &= \lambda \mathbf{\hat{w}}\\
(X^T X + \lambda I) \mathbf{\hat{w}} &= X^T\mathbf{y}
\end{aligned}
$$

接下来我们说明 $X^T X + \lambda I$ 一定是非奇异阵。

由于 $X^T X$ 为实对称矩阵，所以对其特征值分解可以得到

$$
X^T X = U \Lambda U^T = U \begin{bmatrix} \lambda_1 &  &  \\  & \ddots &  \\  &  & \lambda_{d+1} \\\end{bmatrix} U^T
$$

且 $X^T X$ 半正定（为什么，考虑 $\forall \mathbf{v} \ne \mathbf{0}$，我们有 $\mathbf{v}^T X^T X \mathbf{v} = \left\| X\mathbf{v} \right\|^2\ge 0$），$\lambda_1 \ge \lambda_2 \ge \cdots \ge \lambda_{d+1} \ge  0$。

那我们对 $X^T X + \lambda I$ 也做同样操作：

$$
X^T X + \lambda I = U (\Lambda + \lambda I) U^T
$$

$\lambda > 0$，$\Lambda + \lambda I$ 是正定阵，于是 $X^T X + \lambda I$ 也是正定阵，一定可逆。

所以加上 L2-Norm 后，$\mathbf{\hat{w}}$ 是一定有唯一解的：$(X^T X + \lambda I)^{-1} X^T\mathbf{y}$。

加上 L2-Norm 的好处还有一方面：考虑 $X^T X$ 的特征值分解 $U \operatorname{diag}\{\lambda_1, \cdots ,\lambda_{d+1}\}U^T$，一般而言有 $\lambda_{d+1}\to 0$。而 $(X^T X)^{-1} = U \Lambda^{-1} U^T = U \operatorname{diag} \{\lambda_1^{-1}, \cdots , \lambda_{d+1}^{-1}\}U^T$，数值稳定性就不太好。不过加上了正则化项之后，求完逆的最后一个特征值即为 $\displaystyle \frac{1}{\lambda_{d+1}+\lambda}$，不容易出现 numerical issues。

### $\it 2.2$ Lasso 回归：L1-范数
L1-Norm：

$$
\min_{\mathbf{\hat{w}}} L(\mathbf{\hat{w}} ) + \lambda \left\| \mathbf{\hat{w}} \right\|_1
$$
- 注意：$\|\mathbf w\|_1:=\sum\limits_{i=1}^d|w_i|$ 为 L1-范数。正常的欧几里得范数（模长）为 L2-范数。

相当于希望 $\mathbf{\hat{w}}$ 的大多数维度为空，即希望一个稀疏的 $\mathbf{\hat{w}}$，可以理解为一种**特征选择**。带上 L1 正则化项的线性回归称为 Lasso 回归（Least Absolute Shrinkage and Selection Operator）。
- 之所以会出现维度为 $0$，是因为此时在 $\|\mathbf w\|_1$ 相同时在轴上的 $\mathbf w$ 会更倾向于有更小的 $L$。

### $\it 2.3$ 最小二乘法
另一种理解线性回归的方式。理想情况下，我们希望 $X \mathbf{\hat{w}} = \mathbf{y}$。但事实是，$\mathbf{y}$ 可能压根不在 $X$ 的列空间里面，所以并不存在这样的 $\mathbf{\hat{w}}$。那么我们自然希望找到一个 $\mathbf{\hat{y}}$，满足 $\mathbf{\hat{y}}$ 在 $\operatorname{Col}(X)$ 里面，并且这个 $\mathbf{\hat{y}}$ 与我们希望的 $\mathbf{y}$“差距最小”。

所以，$\mathbf{y} - \mathbf{\hat{y}} \perp \operatorname{col}(X)$。即 $X^T(\mathbf{y} - \mathbf{\hat{y}}) = \mathbf{0}$，所以

$$
X^T X \mathbf{\hat{w}} = X^T\mathbf{y}
$$

这与我们之前利用 ERM 推导的结果是相符的。
