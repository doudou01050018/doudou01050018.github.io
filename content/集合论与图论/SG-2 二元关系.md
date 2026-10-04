---
publish: true
---

# SG-2 二元关系
## $\mathrm I$ 有序对与关系的描述
> 在定义方面，二元关系（以及多元关系）使用集合进行形式化定义，但在具体讨论问题时则几乎不考虑这一点。1.2 节中介绍了诸多有关二元关系的衍生运算，其中不乏一些反直觉的式子，需要着重理解。1.3 给出了以矩阵、图描述有限集合上关系的方法。

有关研究集合的思路：在集合至多可列时，可以采用列表的方法研究集合中的元素，而当集合不可列或者在讨论最一般的集合时，就只能依靠逻辑武器（任意、存在、推理法则）。但离散（尤其有限）情况下的集合性质能够对理解性质提供很好的直觉。
### $\it 1.1$ 有序对与 Descartes 积
**有序对**。定义二元有序对 $(a, b):=\{\{a\}, \{a, b\}\}$。基于二元有序对，递归地定义多元组 $(a_1, a_2, \cdots, a_n)=((a_1, a_2, \cdots, a_{n-1}), a_n)$。
- 细节：二元有序对 $(a, b)$ 中，$a, b$ 可以相同。形如 $(a, a)$ 这样的有序对写成集合就是 $\{\{a\}, \{a\}\}=\{\{a\}\}$。
- **有序对的相等**。这是一个需要证明的定理：$(a, b)=(c, d)$ 当且仅当 $a=c\land b=d$。
	- 引理1：$\{x, a\}=\{x, b\}$ 当且仅当 $a=b$。证明（必要性）：若 $x=a$，则 $\{a\}=\{a, b\}$，即 $a=b$。若 $x\neq a$，则 $a\in\{x, b\}$，只能是 $a=b$。
	- 引理2：设 $\mathscr A, \mathscr B$ 是非空集族，若 $\mathscr A=\mathscr B$，则 $\bigcup\limits\mathscr A=\bigcup\limits\mathscr B$ 且 $\bigcap\limits\mathscr A=\bigcap\limits\mathscr B$。证明（以并集为例）：$\forall x$，若 $x\in\bigcup\limits\mathscr A$，则 $\exists Z(Z\in\mathscr A\land x\in Z)$，即 $\exists Z(Z\in\mathscr B\land x\in Z)$，即 $x\in\bigcup\limits\mathscr B$。
	- 那么 $(a, b)=(c, d)$ 意味着 $\bigcup\limits(a, b)=\bigcup\limits(c, d)$，也就是 $\{a, b\}=\{c, d\}$。同时有 $\bigcap\limits(a, b)=\bigcap\limits(c, d)$，即 $a=c$。根据引理1，$b=d$。
- 有了二元组相等，不难得到 $\mathbf a=(a_1, a_2, \cdots, a_n)$ 与 $\mathbf b=(b_1, b_2, \cdots, b_n)$ 相等的充要条件是 $a_i=b_i$ 对任意 $i\in[1, n]$。

**Descartes 积**（二元）。设 $A, B$ 是集合，则定义 $A\times B:=\{(x, y)\mid x\in A\land y\in B\}$ 为 $A, B$ 的 **Descartes 积**。Descartes 积有如下性质：
- 不满足交换律：$A\times B\neq B\times A$，除非 $A=\varnothing$ 或 $B=\varnothing$ 或 $A=B$。
- 不满足结合律：$(A\times B)\times C\neq A\times(B\times C)$，除非 $A=\varnothing$ 或 $B=\varnothing$ 或 $C=\varnothing$。
- **满足分配率**：$A\times(B\cap C)$、$A\times(B\cup C)$、$(B\cap C)\times A$、$(B\cup C)\times A$ 都可以按照结合律展开。

**Descartes 积**（$n$ 元）。设 $A_1, A_2, \cdots, A_n$ 是集合，则定义 $A_1\times A_2\times\cdots\times A_n$ 为 $\{(a_1, a_2, \cdots, a_n)\mid a_1\in A_1\land a_2\in A_2\land\cdots\land a_n\in A_n\}$。称其为 $n$ 元 Descartes 积。同时将 $A\times A\times\cdots\times A$（$n$ 个）简记为 $A^n$。

### $\it 1.2$ 二元关系的描述
#### $\it 1.2.1$ 关系、定义域、值域
**关系**的定义。形式化地，若集合 $F$ 中的全体元素均为有序 $n$ 元组，则称 $F$ 是一个 $n$ 元关系。当 $n=2$ 时，二元关系简称为**关系**。对于二元关系，若 $(x, y)\in F$，则记为 $xFy$。
- 规定 $\varnothing$ 是任意元的关系，简称为**空关系**。
- 当讨论二元关系时，任意一个 $A\times B$ 的子集均为 $A$ **到** $B$ 的二元关系（注意，关系是有顺序的）。称 $A$ 到自身的二元关系为 $A$ 上的二元关系。
- 一些特殊关系：
	- 全域关系 $E_A=A^2$。
	- 恒等关系 $I_A=\{(x, x)\mid x\in A\}$。
	- 以及整除关系、小于关系、小于等于关系、包含关系、真包含关系等。

**关系的定义域与值域**。记 $\mathrm{dom}R:=\{x\mid\exists y(xRy)\}$ 为 $R$ 的**定义域**。记 $\mathrm{ran}R:=\{y\mid\exists x(xRy)\}$ 为 $R$ 的**值域**。称 $\mathrm{fld}R:=\mathrm{dom}R\cup\mathrm{ran}R$ 为 $R$ 的**域**。
- 细节：没有规定 $R$ 一定要写为规整的二元组的集合（注意可能有陷阱，例如 $(F^{-1})^{-1}\subseteq F$，而取等条件为 $F$ 是二元关系）。但是定义域和值域只取二元组的相应元素。
- **逆**。称 $F^{-1}:=\{(y, x)\mid xFy\}$ 为 $F$ 的逆。
- **合成**（**逆序**）。$F\circ G:=\{(x, y)\mid\exists z((x, z)\in G\land (z, y)\in F)\}$。
	- 也就是 $xGzFy$。与函数复合 $f\circ g$ 的理解一致（先在 $x$ 上作用 $G$，再作用 $F$）。
- **限制**。称 $F\restriction A:=\{(x, y)\mid (x, y)\in F\land x\in A\}$。相当于将 $F$ 的定义域限制在 $A$ 上。
- **像**。称 $F[A]:=\mathrm{ran}(F\restriction A)$ 为 $A$ 在 $F$ 下的像。即 $A$ 在关系 $F$ 下被“映射”到了哪些元素。
- **单根**。若对任意 $y\in\mathrm{ran}F$，存在唯一的 $x\in\mathrm{dom}F$ 满足 $xFy$，则称 $F$ 是单根的。
- **单值**。若对任意 $x\in\mathrm{dom}F$，存在唯一的 $y\in\mathrm{ran}F$ 满足 $xFy$，则称 $F$ 是单值的。

#### $\it 1.2.2$ 需要注意的公式
> 尤其需要注意的是**交运算**和**差运算**可能会破坏关系之间（例如定义域、值域）性质的一致性。

**逆关系的定义域和值域**。$\mathrm{dom}(F^{-1})=\mathrm{ran}F$，$\mathrm{ran}(F^{-1})=\mathrm{dom}F$。这是平凡的。

**定义域与值域的交和并**。**注意**：在定义域与值域的交与并中，存在不对称运算：例如 $\mathrm{dom}(F\cup G)=\mathrm{dom}F\cup\mathrm{dom}G$，$\mathrm{ran}(F\cup G)=\mathrm{ran}F\cup\mathrm{ran}G$，**但是** $\mathrm{dom}(F\cap G)\boldsymbol\subseteq\mathrm{dom}F\cap\mathrm{dom}G$，$\mathrm{ran}(F\cap G)\boldsymbol\subseteq\mathrm{ran}F\cap\mathrm{ran}G$。
$$
\begin{aligned} x\in\mathrm{dom}(F\cup G) &\iff \exists y((x, y)\in(F\cup G)) \\ &\iff \exists y((x, y)\in F\lor(x, y)\in G) \\ &\iff \exists y(xFy)\lor\exists y(xGy) \\ &\iff x\in\mathrm{dom}F\lor x\in\mathrm{dom}G \\ &\iff x\in\mathrm{dom}F\cup\mathrm{dom}G \end{aligned}
$$
但是在交的公式中：
$$
\begin{aligned} x\in\mathrm{dom}(F\cap G) &\iff \exists y((x, y)\in(F\cap G)) \\ &\iff\exists y((x, y)\in F\land(x, y)\in G) \\ &\color{Red}\Rightarrow \exists y(xFy)\land\exists y(xGy) \\ &\iff x\in\mathrm{dom}F\land x\in\mathrm{dom}G \\ &\iff x\in\mathrm{dom}F\cap\mathrm{dom}G \end{aligned}
$$
- 在红色的一行中，问题在于前后两个 $y$ 可能不是同一个。本质上，这是在说，当两个关系取交集 $F\cap G$ 时，要求**只有相同的关系**被选入，但 $\mathrm{dom}F\cap\mathrm{dom}G$ 中可能存在某一个 $x$ 在 $F$ 和 $G$ 有**不同的映射值**（但却不同时属于其值域，即 $xFy_1$ 且 $xGy_2$ 但 $y_1, y_2$ 却不能相等）。
	- 但是并集就不存在这个问题。
	- 本质上这是因为 $F\cap G$ 的要求比单纯定义域的交或者单纯值域的交**更高**。
	- 因此取等条件是，任意公共定义域的 $x\in\mathrm{dom}F\cap\mathrm{dom}G$，其至少存在一条同时存在于 $F, G$ 的映射。
- 同理有 $\mathrm{ran}(F\cup G)=\mathrm{ran}F\cup\mathrm{ran}G$，但 $\mathrm{ran}(F\cap G)\subseteq\mathrm{ran}F\cap\mathrm{ran}G$。

另外需要注意的公式（**差运算**）：$\mathrm{dom}F-\mathrm{dom}G\subseteq\mathrm{dom}(F-G)$，$\mathrm{ran}F-\mathrm{ran}G\subseteq\mathrm{ran}(F-G)$。
- 以 $\mathrm{dom}$ 为例，这是因为可能存在一个**不在** $\mathrm{dom}F-\mathrm{dom}G$ 中的 $x$（例如定义域的交中），其有关系 $xFy$，但 $x, y$ 之间却没有关系 $G$。因此 $(x, y)\in F-G$ 但 $x\notin\mathrm{dom}F-\mathrm{dom}G$。
	- 取等条件为，$\mathrm{dom}F\cap\mathrm{dom}G$ 中的所有 $x$，其所有关于 $F$ 的关系 $xFy$ 一定要出现在 $G$ 中。

**关系合成运算的分配律**。同样需要注意交的特殊情况：
- $R_1\circ(R_2\cup R_3)=R_1\circ R_2\cup R_1\circ R_3$，$(R_1\cup R_2)\circ R_3=R_1\circ R_3\cup R_2\circ R_3$。
- $R_1\circ(R_2\cap R_3)\subseteq R_1\circ R_2\cap R_1\circ R_3$，$(R_1\cap R_2)\circ R_3\subseteq R_1\circ R_3\cap R_2\circ R_3$。
	- 后两个以前面的式子为例，其中 $R_1\circ(R_2\cap R_3)$ 代表 $x\to z\to y$ 必须是**同一条路**，但 $R_1\circ R_2\cap R_1\circ R_3$ 却可以是 $x\to z_1\to y$（$R_2$ 路径）和 $x\to z_2\to y$（$R_3$ 路径），没有要求 $z_1=z_2$。

**合成的逆运算**。设 $F, G$ 是两个**任意**集合，则 $(F\circ G)^{-1}=G^{-1}\circ F^{-1}$。
- 这是恒成立的。

有关**限制**的一组公式。$R\restriction\bigcup\limits\mathscr A=\bigcup\limits\{R\restriction A\mid A\in\mathscr A\}$；$R\restriction\bigcap\limits\mathscr A=\bigcap\limits\{R\restriction A\mid A\in\mathscr A\}$。$(F\circ G)\restriction A=F\circ(G\restriction A)$。
- 其中涉及集族的公式可以简化为形如 $R\restriction(A\cap B)=(R\restriction A)\cap(R\restriction B)$ 等。

有关**像**的一组公式：
- $F\left[ \bigcup\limits\mathscr A \right]=\bigcup\limits\{F[A]\mid A\in\mathscr A\}$。这没问题。
- $F\left[ \bigcap\limits\mathscr A \right]\subseteq\bigcap\limits\{F[A]\mid A\in\mathscr A\}$。这个问题与上面所述同理，即可能存在一个值域中的 $y$，其到 $\mathscr A$ 的各个部分都存在关系，但不存在一条同时存在于各个部分的关系。
- $F[A]-F[B]\subseteq F[A-B]$。
- $(F\circ G)[A]=F[G[A]]$。
### $\it 1.3$ 关系矩阵与关系图
**关系矩阵**。对有限集合 $A=\{x_1, x_2, \cdots, x_n\}$ 上的二元关系 $R$ 定义关系矩阵 $M(R)=(r_{ij})_{n\times n}$。其中每一个元 $r_{ij}=\begin{cases} 1 \quad x_iRx_j \\ 0 \quad \mathrm{otherwise} \end{cases}$。关系矩阵 $M(R)$ 是集合 $R$ 的另一种**完全等价**的写法。
- 基本性质：$M(R^{-1})=M^T(R)$。
- 矩阵合成：$M(F\circ G)=^* M(G)\cdot M(F)$。
	- 注 $*$：在这里如果按照矩阵乘法计算 $M(G)\cdot M(F)$，那么得到的结果将会是“路径条数”，严格来说与只关注可达性的 $0, 1$ 矩阵不相等。若将矩阵乘法规则中相加的步骤改为**逻辑或**，则可以写为等号。

**关系图**。设 $A=\{x_1, x_2, \cdots, x_n\}$，$R\subseteq A^2$，以 $A$ 中所有元素为顶点，依照 $R$ 在顶点之间连边：$(x_i, x_j)$ 之间存在**有向边**当且仅当 $x_iRx_j$。这是 $R$ 内容的另一种等价写法。
## $\mathrm{II}$ 关系特殊性质的讨论
主要分析自反、对称、传递这三类特殊的关系。
### $\it 2.1$ 自反、对称、传递
对于 $R\subseteq A^2$，有一系列特殊的性质可以探讨：
- 自反。若 $\forall x(x\in A\to xRx)$，则称 $R$ 是自反的。
	- 反自反。若 $\forall x(x\in A\to\neg(xRx))$。
- 对称。若 $\forall x\forall y(x\in A\land y\in A\to(xRy\leftrightarrow yRx))$。
	- 反对称。$xRy\land yRx\to x=y$。
- **传递**。对于任意的 $x, y, z\in A$，若 $xRy\land yRz$，则有 $xRz$。

**有关特殊关系性质与交并集讨论**：
- 若 $R_1, R_2$ 是自反的，则 $R_1^{-1}, R_1\cup R_2, R_1\cap R_2, R_2\circ R_1$ 都是自反的。
- 若 $R_1, R_2$ 是反自反的，则 $R_1^{-1}, R_1\cup R_2, R_1\cap R_2, R_1-R_2$ 都是反自反的。
- 若 $R_1, R_2$ 是对称的，则 $R_1^{-1}, R_1\cup R_2, R_1\cap R_2, R_1-R_2, \sim R_1$ 都是对称的。
- 若 $R_1, R_2$ 是反对称的，则 $R_1^{-1}, R_1\cap R_2, R_1-R_2$ 也是反对称的。
- 若 $R_1, R_2$ 是传递的，则 $R_1^{-1}, R_1\cap R_2$ 也是传递的。
### $\it 2.2$ 二元关系的幂运算
**二元关系幂运算的定义**。设 $R\subseteq A^2$，递归定义 $R^0=I_A, R^{n+1}=R^n\circ R$。
- （组合分析思路）设 $|A|=n$，$R\subseteq A^2$，则存在自然数 $0\le s<t\le 2^{n^2}$，使得 $R^s=R^t$。
	- 证明：考虑反证并对结构施加约束，考虑如果 $0\le i\le 2^{n^2}$ 的 $R^i$ 各不相同会发生什么。因为 $R^i\in P(A^2)$，而由于 $|A|=n$，因此 $|P(A^2)|=2^{n^2}$。但 $0$ 到 $2^{n^2}$ 共有 $2^{n^2}+1$ 个元素，因此根据抽屉原理一定存在 $R^s=R^t$。
	- 可以看出，在 $2^{n^2}$ 的范围内一定存在循环节，虽然 $2^{n^2}$ 的上界其实极其宽松。
### $\it 2.3$ 关系的闭包
#### $\it 2.3.1$ 闭包的定义
直观来说，闭包的含义是对于 $R$，寻找一个 $R'\supseteq R$ 且满足某种性质的最小的 $R'$。形式化定义如下：设 $R\subseteq A^2$，$R$ 的自反（对称、传递）闭包 $R'$ 满足如下条件：
1. $R'$ 是自反（对称、传递）的。
2. $R\subseteq R'$。
3. $A$ 上任意自反（对称、传递）的关系 $R''$，若 $R\subseteq R''$，则有 $R'\subseteq R''$。
- 按照生成元的视角，群也是其最小生成元的“闭包”。
- 常用 $r(R), s(R), t(R)$ 表示 $R$ 的自反闭包、对称闭包、传递闭包。

**闭包的性质**：
- $R_1\subseteq R_2\subseteq A^2$，则 $r(R_1)\subseteq r(R_2)$，$s(R_1)\subseteq s(R_2)$，$t(R_1)\subseteq t(R_2)$。
	- 证明：$R_1\subseteq R_2\subseteq r(R_2)$，且 $r(R_2)$ 同时满足自反与 $R_1\subseteq r(R_2)$，所以由定义 $r(R_1)\subseteq r(R_2)$。对 $s, t$ 同理。
- $F, G\subseteq A^2$，则 $r(F\cup G)=r(F)\cup r(G)$，$s(F\cup G)=s(F)\cup s(G)$，但 $t(F\cup G)\boldsymbol\supseteq t(F)\cup t(G)$。
	- 对传递闭包的不等取反例很容易，令 $F:1\to 2$ 与 $G:2\to 3$ 即可。
#### $\it 2.3.2$ 闭包的性质
**闭包的简便表示**：
- $r(R)=R\cup I_A$。
	- 证明：首先 $R\cup I_A$ 是自反的。因为 $R\subseteq R\cup I_A$，所以 $r(R)\subseteq R\cup I_A$。另一方面，$R\subseteq r(R)\land I_A\subseteq r(R)\Rightarrow(R\cup I_A\subseteq r(R))$。得到 $r(R)=R\cup I_A$。
- $s(R)=R\cup R^{-1}$。
	- 证明：首先证明 $R\cup R^{-1}$ 是对称的。任意 $x(R\cup R^{-1})y$，则有 $xRy$ 或 $xR^{-1}y$。若 $xRy$，则 $yR^{-1}x$，因此其并集里 $x, y$ 是对称的。对 $xR^{-1}y$ 同理。
	- 然后证明 $R\cup R^{-1}$ 是最小的。首先 $s(R)\subseteq R\cup R^{-1}$。然后 $R\subseteq s(R)$。关键在于说明 $R^{-1}\subseteq s(R)$。因为 $xRy\to xs(R)y\land ys(R)x$，因此 $xR^{-1}y\iff yRx\to ys(R)x\iff xs(R)y$。所以 $R^{-1}\subseteq s(R)$。所以 $R\subseteq s(R)\land R^{-1}\subseteq s(R)\to R\cup R^{-1}\subseteq s(R)$。
- $t(R)=\bigcup\limits_{k=1}^\infty R^k$。
	- 证明：首先要证明 $\bigcup\limits_{k=1}^\infty R^k$ 是传递的。$x\left( \bigcup\limits_{k=1}^\infty R^k \right)y\land y\left( \bigcup\limits_{k=1}^\infty R^k \right)z\iff\exists n(xR^ny)\land\exists m(yR^mz)$。所以 $R^{n+m}=R^m\circ R^n$ 中，$x(R^m\circ R^n)z$。因此 $x\left( \bigcup\limits_{k=1}^\infty R^k \right)z$。所以 $t(R)\subseteq\left( \bigcup\limits_{k=1}^\infty R^k \right)$。
	- 然后我们想要证明 $\left( \bigcup\limits_{k=1}^\infty R^k \right)\subseteq t(R)$。对此证明 $\forall n(R^n\subseteq t(R))$。归纳，已知 $R\subseteq t(R)$，假设对 $1\le k\le n$ 有 $R^k\subseteq t(R)$，则 $xR^{n+1}y\iff x(R^n\circ R)y\iff\exists z(xRz\land zR^ny)$。因此 $\exists z(xt(R)z\land zt(R)y)$。根据传递性，$xt(R)y$。因此 $R^{n+1}\subseteq t(R)$。所以 $\left( \bigcup\limits_{k=1}^\infty R^k \right)\subseteq t(R)$。至此两者相等。
- 疑问1：如果 $R$ 不可数，其传递链也不可数，那可数并岂不是不能覆盖？
- 疑问2：是否有更 intuitive 的定义可以使用？

#### $\it 2.3.3$ Floyd-Warshell 算法
Algo. **Floyd-Warshell 算法**求传递闭包。
```cpp
int f[N][N];
void floyd_warshell(int n) {
	for(int k = 1; k <= n; ++k) 
		for(int i = 1; i <= n; ++i) 
			for(int j = 1; j <= n; ++j) 
				f[i][j] |= f[i][k] & f[k][j];
}
```
算法分析：
- 采用动态规划，令 $f_k(i, j)$（$1\le i, j\le k$）表示只能以 $1, 2, \cdots, k$ 为中转点时，$(i, j)$ 的连通性。
- 考虑转移到 $f_{k+1}(i, j)$。若存在不经过 $k+1$ 的路径，则其已经被包含在 $f_k(i, j)$ 中。
- 否则，若存在一条必须经过 $k+1$ 的路径，则**当且仅当**存在 $f_k(i, k+1)$ 与 $f_k(k+1, j)$（充分性显然，必要性：在可达性分析中，可以假设 $k+1$ 在路径中只出现了一次，则 $i\to k+1$ 以及 $k+1\to j$ 都只应以 $1, 2, \cdots, k$ 为中转点）。
- 因此算法成立。而因为可达性不存在覆盖问题，因此可以省去数组的 $k$ 维度。复杂度 $O(n^3)$。
#### $\it 2.3.4$ 闭包的组合关系
若 $R$ 本身满足自反或对称或传递，则 $r(R), s(R), t(R)$ 是否满足自反、传递、对称？

|        | 自反性 | 对称性 | 传递性 |
| ------ | --- | --- | --- |
| $r(R)$ |     |     |     |
| $s(R)$ |     |     | 不一定 |
| $t(R)$ |     |     |     |
只有【$R$ 满足传递性，那么 $s(R)$ 满足传递性】是**错误**的，其余都是满足的。反例很简单，设 $R=\{(1, 2), (1, 3)\}$ 即可。

**闭包关系的复合**。定理：$rs(R)=sr(R)$，$rt(R)=tr(R)$，但 $st(R)\subseteq ts(R)$。
- 先证明两个基础命题：$(F\cup G)^{-1}=F^{-1}\cup G^{-1}$，以及 $(R\cup I_A)^n=\bigcup\limits_{k=0}^n R^k$。证明简单。
- $\begin{aligned}sr(R)&=s(R\cup I_A)=(R\cup I_A)\cup(R\cup I_A)^{-1}=(R\cup I_A)\cup(R^{-1}\cup I_A)\\ &=R\cup R^{-1}\cup I_A=s(R)\cup I_A=rs(R)\end{aligned}$。
- 后面几个证明思路是类似的，都是利用 $r(R), s(R), t(R)$ 的已知表达式进行运算。
- **这说明** $r$ 操作是可以与 $s, t$ 交换的，而 $s, t$ 不可交换。
	- 第三个命题的反例仍然是 $R=\{(1, 2), (1, 3)\}$。
## $\mathrm{III}$ 等价关系、划分与序
### $\it 3.1$ 什么是等价关系；第二类 Stirling 数
**等价关系**的定义。设 $R\subseteq A^2$ 且 $R$ 是**自反的、对称的和传递的**，则称 $R$ 是 $A$ 上的**等价关系**。

**等价类**。设 $R\subseteq A^2$ 是等价关系，任取 $x\in A$，则定义 $[x]_R:=\{y\in A\mid xRy\}$ 为 $x$ 的**关于 $R$ 的等价类**。简称 $x$ 的等价类，或者直接记作 $[x]$。有一些基本性质：
- 若 $xRy$，则 $[x]=[y]$。这是因为传递性。
- 若 $\neg(xRy)$，则 $[x]\cap[y]=\varnothing$。使用反证法。
- $\bigcup\limits\{[x]\mid x\in A\}=A$。显然 $x\in[x]$。

**商集与划分**。设 $A$ 是集合，$R$ 是等价关系，则定义 $A / R:=\{[x]\mid x\in A\}$。即 $A$ 中所有不同的等价类组成的集合。同时形式化定义**划分**为一个集族 $\mathscr A\subseteq P(A)$，满足 $\varnothing\notin A$，$\forall x, y\in\mathscr A(x\neq y\to x\cap y=\varnothing)$ 且 $\bigcup\limits\mathscr A=A$，称 $\mathscr A$ 为 $A$ 的一个**划分**，$\mathscr A$ 中的元素称为**划分块**。
- **划分与等价关系的等价性**。$A$ 上的等价关系 $R$ 与划分 $\mathscr A$ 存在**双射关系**。

划分的计数（第二类 Stiring 数）。设 $|A|=n$，求 $A$ 的不同划分数量（或等价关系数量）。一种等效的说法是，给定 $n$ 个不同的球，将其放入 $r$ 个相同的盒子，且要求无空盒，求总放置方法。这是一个组合（计数）问题，引入第二类 Stirling 数：$\begin{Bmatrix} n \\ k \end{Bmatrix}$ 表示 $1, 2, \cdots, n$ 划分为 $k$ 个集合的个数。
- 递推关系 $\begin{Bmatrix} n \\ m \end{Bmatrix}=\begin{Bmatrix} n-1 \\ m-1 \end{Bmatrix}+m\begin{Bmatrix} n-1 \\ m \end{Bmatrix}$。含义是对 $n-1$ 大小的子问题考虑 $n$ 的放置位置，如果 $n$ 单独是一个集合，那么方案数是 $\begin{Bmatrix} n-1 \\ m-1 \end{Bmatrix}$；如果 $n$ 不是单独的集合，那么，考虑 $\begin{Bmatrix} n-1 \\ m \end{Bmatrix}$，将 $n$ 加入任意一个集合即可得到新方案，即 $m\begin{Bmatrix} n-1 \\ m \end{Bmatrix}$。

**划分的加细**。若 $\mathscr B$ 与 $\mathscr A$ 都是 $A$ 的划分，且 $\mathscr B$ 的每一个划分块都包含于 $\mathscr A$ 的某个划分块中，则称 $\mathscr B$ 是 $\mathscr A$ 的**加细**。
- $\mathscr B$ 是 $\mathscr A$ 的加细，当且仅当 $R_{\mathscr B}\subseteq R_{\mathscr A}$。
## $\mathrm{IV}$ 序关系
### $\it 4.1$ 偏序的定义
**偏序**。设 $R\subseteq A^2$。若 $R$ 是**自反、反对称且传递的**，则称 $R$ 是 $A$ 上的**偏序关系**。此时 $xRy$ 记作 $x\preceq y$。

**偏序集**。将一个非空集合 $A$ 与其上的偏序关系写成有序二元组 $(A, \preceq)$，记作一个偏序集。
- 在偏序集中，对于给定的偏序，若 $x\preceq y\lor y\preceq x$，则称 $x, y$ 是**可比的**。
- 若 $x\prec y$ 且不存在 $z\in A$ 使得 $x\prec z\prec y$，则称 $y$ **覆盖** $x$。
- 偏序集的 Hasse 图。若 $y$ 覆盖 $x$，则连由 $x$ 到 $y$ 的有向边（或者无向边，将 $y$ 画在 $x$ 的上方，指在纸上画图）。实际上就是根据“最小”比较关系生成对应的 DAG。
- **全序关系、全序集**。若 $(A, \preceq)$ 中任意两个元素 $x, y$ 都是可比的，则称 $\preceq$ 为 $A$ 上的**全序关系**，此时 $(A, \preceq)$ 为全序集。
- **拟序关系**。若 $\prec{} \subseteq A^2$ 是反自反、反对称和传递的，则称 $(A, \prec)$ 为拟序集。拟序与偏序可以互相转化。
### $\it 4.2$ 偏序中的极值
设 $(A, \le)$ 为偏序集，$B\subseteq A$。定义如下元素：
- **最小元**。若存在 $y\in B$ 使得 $\forall x(x\in B\to y\le x)$，则称 $y$ 是 $B$ 的**最小元**。
- **最大元**。若存在 $y\in B$ 使得 $\forall x(x\in B\to x\le y)$，则称 $y$ 是 $B$ 的**最大元**。
- **极小元**。若存在 $y\in B$ 使得 $\forall x(x\in B\land x\le y\to x=y)$，则称 $y$ 是 $B$ 的**极小元**。
- **极大元**。若存在 $y\in B$ 使得 $\forall x(x\in B\land y\le x\to x=y)$，则称 $y$ 是 $B$ 的**极大元**。

分析。极小元的意思是，在 $B$ 中所有能与其比较大小的元素中，极小元是最小的。而最小元要求其能与所有其他元素比较大小并其本身是最小的。按照 DAG 来模拟有限情形，则极小元相当于入度为零的点，而最小元要求入度为零的点有且仅有其一个。

**上界、上确界**。设 $(A, \le)$ 为偏序集，$B\subseteq A$。若存在 $y\in A$ 使得 $\forall x(x\in B\to x\le y)$，则称 $y$ 是 $B$ 的一个上界。若 $C:=\{y\in A\mid y\text{ is an upper bound of }B\}$ 存在**最小元**，则称其为 $B$ 的**上确界**。类似可以定义下界以及下确界。
- $B$ 很可能不存在任何上界（拿 DAG 理解，不同元素没有公共的下游），即便存在，也不保证有上确界（例子，即便像 $\mathbb Q$ 这种全序，其上确界可能为 $\sqrt 2$ 这种无理数）。
### $\it 4.3$ 偏序中的链结构
**链（Chain）、反链**。设 $(A, \le)$ 为偏序集，$B\subseteq A$。若对于 $\forall x, y\in B$，$x, y$ 均可比，则称 $B$ 为 $A$ 中的一条**链**。称 $B$ 中的元素个数为链的长度（如果存在）。反过来，若 $\forall x, y\in B$ 且 $x\neq y$，$x, y$ 均不可比，则称 $B$ 是 $A$ 的一条反链。称 $B$ 中的元素个数为反链的长度（如果存在）。

定理。设 $(A, \le)$ 为偏序集，若 $A$ 中最长链的长度为 $n$，则 $A$ 中存在极大元，且可以将 $A$ 划分为 $n$ 个反链。
- 证明。考虑任意一条长度为 $n$ 的链，则其最大元 $x_n$ 即为 $A$ 的极大元（反证，若存在 $x_n\le y$，则形成了一条长度为 $n+1$ 的链，矛盾）。然后考虑如何划分为 $n$ 个反链。想到对所有极大元下手，可以将其提取出来形成一个反链并将其从 $A$ 中删掉（操作合法，是因为首先各个极大元之间一定不可比）。此后剩下的集合 $A'$ 的最长链长度为 $n-1$（依旧反证，若存在长度为 $n$ 的链则说明要么原图存在长度为 $n+1$ 的链，要么其极大元根本没有被删掉）。因此可以反复操作使得最长链长度变为 $1$。证毕。
- 推论。设 $(A, \le)$ 为偏序集，若 $|A|=mn+1$，则 $A$ 中或者存在长度为 $m+1$ 的反链，或者存在长度为 $n+1$ 的链。
	- 分析。这是组合存在性证明。常见处理手法是反证，并给目标结构施加限制使其崩溃进而导出矛盾。反证，假设 $A$ 中至多存在长度为 $n$ 的链与长度为 $m$ 的反链。按照上述定理的思路，至多存在 $m$ 个极大元，将其删掉之后最长链长度变为 $n-1$，反复操作 $n$ 次得到 $|A|$ 至多为 $nm$，矛盾。

**良序关系**。设 $(A, <)$ 为拟全序集，若对 $A$ 的任何非空子集 $B$ 均存在最小元，则称 $<$ 为良序关系，$(A, <)$ 为良序集。
- 感觉跟实数完备之类的有关。
### $\it 4.N$ （P-complete）偏序计数问题、序问题常见处理手法
问题：给定 $x_1, x_2, \cdots, x_n$，其中有多少种不同的偏序？
- 这是一个 P-complete 问题，我发现一旦涉及偏序以及这种“数一数有多少种合法选择”的问题，就很容易变为 \#P 或者 complete。或者说，问题在于一个偏序所包含的信息量太大了。

一个值得讨论的点在于，序问题的分析难度远大于划分。划分是一种规整的结构（并查集），元素之间只有等价与不等价的关系，而序问题即便是有限元的集合，其等价于分析一个 DAG 的结构，而一般 DAG 的结构特征较为有限，因此很多问题都是 complete 级别的。面对这种问题，应当从特殊结构（如链、反链、极大元）、一般想法以及基本性质（DAG 的基本性质、拓扑排序转化为全序、传递关系等）分析起。
### $\it 4.M$ Zorn's Lemma
Thm. **Zorn's Lemma**. 设 $(P, \le)$ 为偏序集，若任意的链 $B\subseteq P$ 都在 $P$ 中存在上界，则 $P$ 存在极大元。
