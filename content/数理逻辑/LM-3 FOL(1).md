---
publish: true
---

# LM-3 FOL(1)
### $\it 0.1$ Intro
在命题逻辑中，语法和语义的界限是十分模糊的，这是因为命题逻辑的结构太简单了，而在一阶逻辑中，这两者将出现明显的分化。

总体地图：
$$ \boxed{ \begin{array}{ccccc} \text{Language }L &\to& \text{Terms/Formulas} &\to& \text{Structure }\mathcal M \\ &&\downarrow&&\downarrow\\ &&\text{Syntax} &&\text{Semantics} \\ &&&\searrow&\\ &&&&\mathcal M\models\varphi[s] \end{array}}
$$

一阶逻辑是描述数学结构的逻辑。
## $\mathrm I$ Definitions of FOL
### $\it 1.1$ Syntax of FOL
一阶逻辑的语法可以看做程序的“源代码”，其**不提供任何释义**，只规定了其中符号的嵌套规则，正如“$1$”可以释义为实数的一，或者群的单位元，其释义会随着“编译器”的不同而不同。但是在针对特定模型 $M$ 进行分析的时候，我们的思维却也常常依赖符号表达。

Def. **Languages and formulas**. A FOL language use following symbols(Basic characters of every FOL):
- parentheses(brackets): `(` and `)`.
- connectives: `\neg, \land, \lor, \to`.
- A **countably infinite** collection of variables $V=\{v_i\mid i\in\mathbb N\}$. (Usually written as $x, y, z$).
- equation: `=`.
- **Quantifiers**: `\forall` and `\exists`.
- Non-logical symbols: 
	1. A collection of function symbols $f$, each with a fixed arity.
	2. A collection of relation symbols $R$, each with a fixed arity.
	3. A collection of constant symbols $c$.

Then $\mathcal L=\{f_i\mid i\in I\}\cup\{R_j\mid j\in J\}\cup\{c_k\mid k\in K\}$ formally describes the language.
- 注意：这里的 $f, R, c$ 仅停留在符号的层面，$f(x, y)$ 这样的表达仅仅是写法，负责提供含义（其具体映射到什么值）是语义负责的部分。
- Also: $\{v_i\mid i\in\mathbb N\}$ such variables need **NO** preliminary note. We write $x, y, z, \cdots$ for them.
- Note: Language $\mathcal L$ is the border of FOL syntax. It defines what you can describe with such symbols.
- Note: $I, J, K$ need not to be countable. Their cardinalities can be arbitrary.

Def. **Terms**. Let $\mathcal L$ be a First-Order-Language. the $\mathcal L$-terms are defined recursively:
1. Every variable and every constant symbol is a term.
2. If $t_1, t_2, \cdots, t_n$ are terms and $f$ is an $n$-ary function symbol, then $f(t_1, t_2, \cdots, t_n)$ is a term.
3. Only strings obtained by finitely steps are terms.

Def. **Atomic Formulas**. An atomic formula of $\mathcal L$ is an expression of one of the following forms:
- $t_1=t_2$, where $t_1$ and $t_2$ are terms.
- $R(t_1, t_2, \cdots, t_n)$, where $R$ is an $n$-ary relation symbol and $t_1, t_2, \cdots, t_n$ are terms.
- Note: This is where things become "boolean", though there is no such semantic word "boolean". But learning FOL requires one to understand syntaxes in some semantic way.

Def. **$\mathcal L$-Formulas**. The $\mathcal L$-formulas are define recursively:
1. Every atomic $L$-formula is a formula.
2. If $\varphi$ and $\psi$ are formulas, then $(\neg\varphi), (\varphi\land\psi), (\varphi\lor\psi), (\varphi\to\psi)$ are  formulas.
3. If $\varphi$ is a formula and $x$ is a variable, then $\forall x\varphi$ and $\exists x\varphi$ are formulas.
- 到此为止我们仍然在规范化语法规则，其层次可以分为**项**与公式。其中项对应的是由原子变量（一开始的 $v_i$）经过一些 $f$ 得到的，代表仍然是变量的部分；而公式是即将被解释为布尔值的部分（其定义也是递归的）。
	- And, formulas can also be regarded as ${\mathrm{variable}}^n\to\mathrm{bool}$ functions where the input are exactly free variables(precisely, in a structure).

Def. **Free Variables**. Let $\varphi$ be an $\mathcal L$-formula and $x$ a variable. We define what is means for $x$ to **occur freely** in $\varphi$ recursively:
1. If $\varphi$ is atomic, then $x$ occurs freely in $\varphi$ **iff** $x$ appears in $\varphi$.
2. If $\varphi=(\neg\psi)$, then $x$ occurs freely in $\varphi$ **iff** it occurs freely in $\psi$.
3. If $\varphi=(\psi\otimes\theta)$ where $\otimes\in\{\land, \lor, \to\}$, then $x$ occurs freely in $\varphi$ **iff** $x$ occurs freely in $\psi$ **or** in $\theta$(or both).
4. If $\varphi=\forall y\psi$ or $\varphi=\exists y\psi$, then $x$ occurs freely in $\varphi$ **iff** $x$ occurs freely in $\psi$ and $x\neq y$.
- We write $F(\varphi)$ as the set of variables free in $\varphi$.
- 自由变量是在释义中“起决定性”的部分，因为相对的被 `\forall` 和 `\exists` 限制的 $x$ 就是“工具人”，其作用只是在释义的时候跑遍其论域 $A$ 检查句子的合法性。

Def. **Sentence**. An $\mathcal L$-formula is a sentence if it has no free variables.
- 根据句子的定义，其没有自由变量，因此一旦确定 $M$（主要是确定 $f$ 和 $R$），那么其真假就固定了，与赋值 $s$ 无关。

可以看到，语法的分层结构是从变量（$x_i$）到项（$x_i$ 以及 $f$ 变换后），然后组成原子公式（Atomic Formula，基础等式以及布尔函数 $R$），以及通过逻辑联结词进行逻辑的复合（$\neg, \land, \lor, \to$），还有**量词**进行扩展。
### $\it 1.2$ Models and Satisfaction
Let $\mathcal L=\{f_i\mid i\in I\}\cup\{R_j\mid j\in J\}\cup\{c_k\mid k\in K\}$ as a language.

Def. **$\mathcal L$-Structure**. An $\mathcal L$-structure, also called an $\mathcal L$-model, is a tuple $M=(A; (f_i^M)_{i\in I}, (R_j^M)_{j\in J}, (c_k^M)_{k\in K})$ consisting of a non-empty set $A$, called the **universe**, and an interpretation of each non-logical symbol:
1. For each $n$-ary function symbol $f_i$, a function $f_i^M:A^n\to A$.
2. For each $n$-ary relation symbol $R_j$, a relation $R_j^M\subseteq A^n$. (Also, $A^n\to\{\mathrm T, \mathrm F\}$ from my own perspective)
3. For each constant symbol $c_k$, an element $c_k^M\in A$.
- 从这里开始，进入语义的解释部分。具体来说，语义描述了一种针对 $\mathcal L$-语言的“编译”方案（也就是编译器），其规定世界的元素的集合是 $A$，并定义了 $A$ 元素之间的转移关系（$f$ 函数以及 $R$ 关系判定）。同一语言不同的模型就好比 C++ 不同版本的编译器一样。

Def. **Assignments and Interpretations of terms**. Let $M$ be an $\mathcal L$-structure. An **assignment** is a map $s:V\to A$(to assign every variable a "thing" in the universe $A$). It extends to a map $\overline s:\{\mathcal L\text{-terms}\}\to A$ as a recursive rule:
1. If $x$ is a variable, then $\overline s(x)=s(x)$.
2. If $c$ is a constant symbol, then $\overline s(c)=c^M$.
3. If $f$ is an $n$-ary function symbol and $t_1, t_2, \cdots, t_n$ are terms, then $\overline s(f(t_1, t_2, \cdots, t_n))=f^M(\overline s(t_1), \overline s(t_2), \cdots, \overline s(t_n))$.
- Also, for $x\in V$ and $d\in A$, write $s(x\mid d)$ as an assignment which agrees with $s$ except $x$:
	- $s(x\mid d)(t)=\begin{cases} d\quad t=x\\s(t)\quad t\neq x \end{cases}$.
- Assignment 赋值就是负责“链接”源代码与模型程序的，其负责给每一个 $v_i$ 的符号赋值为各个具体的 $a_i$，然后由 $M$ 来“运行程序”判定真假，这才是整套完整的流程。

Def. **Satisfaction**. Let $M$ be an $\mathcal L$-structure and $s:V\to A$ an assignment. We define $M\models\varphi[s]$ ($M$ satisfies $\varphi$ under $s$) recursively:
1. $M\models(t_1=t_2)[s]\iff\overline s(t_1)=\overline s(t_2)$.
2. $M\models R(t_1, t_2, \cdots, t_n)[s]\iff(\overline s(t_1), \overline s(t_2), \cdots, \overline s(t_n))\in R^M$.
3. $M\models(\neg\varphi)[s]\iff M\not\models\varphi[s]$.
4. $M\models(\varphi\land\theta)[s]\iff (M\models\varphi[s])\land(M\models\theta[s])$.
5. $M\models(\varphi\lor\theta)[s]\iff(M\models\varphi[s])\lor(M\models\theta[s])$.
6. $M\models(\varphi\to\theta)[s]\iff(M\not\models\varphi[s])\lor(M\models\theta[s])$.
7. $M\models(\forall x\varphi)[s]$ **iff** for every $d\in A$, $M\models\varphi[s(x\mid d)]$.
	- Translation: Modify $x$ in $\varphi$ to arbitrary $d\in A$ and it is valid for every $d$.
8. $M\models(\exists x\varphi)[s]$ **iff** there exists $d\in A$ s.t. $M\models\varphi[s(x\mid d)]$.
- In general, $M\models\varphi[s]$ is also a thing defined recursively by the structure of composition.
- 注意：这是结构归纳的形式来源，可以将其分为三类：原子公式类、布尔复合类（四种）、量词类。
- 其是具有层次的：起初，符号 $x_i$ 被翻译为有意义的 $a_i\in A$，然后还将经过 $f$ 函数的变换（实际上是 $f^M$）。中间至关重要的一步是 $R$ 将其由 $A$ 变为布尔值，然后经过逻辑运算符仍然为布尔值，得到最终的结果。

Prop. Let $M$ be an $\mathcal L$-structure and $s_1, s_2:V\to A$ be assignments, and $\varphi$ as a formula. If $s_1\mid_{F(\varphi)}=s_2\mid_{F(\varphi)}$, then $M\models\varphi[s_1]\iff M\models\varphi[s_2]$.
- **Proof**. Lemma. If $s_1, s_2$ agrees on every variable in a term $t$, then $\overline s_1(t)=\overline s_2(t)$.
	- Induction. If $t=x$ is a variable, then $s_1(x)=s_2(x)$. If $t=c$ is a constant, then both of them are $c^M$. If $t=f(t_1, t_2, \cdots, t_n)$, then $\overline s_1(t)=f(\overline s_1(t_1), \overline s_1(t_2), \cdots, \overline s_1(t_n))=f(\overline s_2(t_1), \overline s_2(t_2), \cdots, \overline s_2(t_n))=\overline s_2(t)$.
	- Then, apply structural induction. For atomic formula $\varphi=(t_1=t_2)$, $M\models\varphi[s_1]\iff\overline s_1(t_1)=\overline s_1(t_2)$. Since $t_1, t_2$ are free variables, $\overline s_1(t_i)=\overline s_2(t_i)$ for $i=1, 2$. Thus $M\models\varphi[s_2]$. For a relation $\varphi=R(\mathbf t)$, $s_1, s_2$ are exactly the same since all variables are free.
	- Then, for boolean connectives. If $\varphi=\neg\psi$, then $F(\varphi)=F(\psi)$ and by induction $M\models\psi[s_1]\iff M\models\psi[s_2]$, so $M\not\models\varphi[s_i]\iff M\models\psi[s_i]$. For $\varphi=\psi\land\theta$, $F(\psi), F(\theta)\subseteq F(\varphi)$ so if $F(\varphi)$ is same in $s_1$ and $s_2$, then $F(\psi), F(\theta)$ are also same. So $M\models\psi[s_1]\iff M\models\psi[s_2]$ are others. Then $M\models\varphi[s_1]\iff M\models\varphi[s_2]$. For $\varphi=\psi\lor\theta, \varphi=\psi\to\theta$, the ideas are the same.
	- Then, for quantifiers(the only non-trivial part). If $\varphi=\forall x\theta$, then $F(\varphi)=F(\theta)-\{x\}$. Now $s_1$ and $s_2$ are the same in $F(\theta)$ except $x$. Consider $s_1(x\mid d)$ and $s_2(x\mid d)$ for any $d\in A$, all assignments are the same(since $x$ is redirected to $d$), so $M\models\theta[s_1(x\mid d)]$ for every $d\in A$ **iff** $M\models\theta[s_2(x\mid d)]$ for every $d\in A$. (For $\exists$, the same).
- **Corollary**. Let $\varphi$ be an $\mathcal L$-sentence and $M$ a structure. Then exactly one of the following holds:
	1. $M\models\varphi[s]$ for every assignment $s:V\to A$.
	2. $M\models\varphi[s]$ for no assignment $s:V\to A$.
	- **Proof**. Since $F(\varphi)=\varnothing$, any two assignments agree on $F(\varphi)$. (In a sentence, $s$ is not important at all since there are no free variables)

Def. If $\varphi$ is an $\mathcal L$-sentence and $M$ is an $\mathcal L$-structure, we write $M\models\varphi$ if $M\models\varphi[s]$ for some assignment $s$(or equivalently, every $s$).

Def. **Definable Set**. Let $M$ be an $\mathcal L$-structure and $D\subseteq A^n$, we say that $D$ is **definable**(without parameters) if there is an $\mathcal L$-formula $\varphi(x_1, x_2, \cdots, x_n)$ suth that $\mathbf a\in D\iff M\models\varphi(\mathbf a)$.
- In this definition, $M\models\varphi(\mathbf a)$ is the simplified form of $M\models\varphi[s(x_i)=a_i]$.
### $\it 1.3$ FOL Induction Tricks
This is very important since the definition is very complex. We divide the induction into 3(or 4) levels.
1. Terms. Assigments are working. Atomic variables, constants, function symbols. **Through assigment**, $x_i\to a_i$(if let $a_i:=s(x_i)$) and $c\to c^M$. $f(t_1, t_2, \cdots, t_n)=f^M(\overline s(t_1), \overline s(t_2), \cdots, \overline s(t_n))$.
2. Atomic formulas. $R(t_1, t_2, \cdots, t_n)\to R^M(\overline s(t_1), \overline s(t_2), \cdots, \overline s(t_n))$. And $[t_1=t_2]\to[\overline s(t_1)=\overline s(t_2)]$.
3. Boolean rules. $\varphi\otimes\theta\to\varphi^M\otimes\theta^M$.
4. **Quantifiers**. $\forall x\varphi\to\forall x\varphi^M$.

To help understanding:
- Let $\mathcal L=\{\le; 3\}$. Let $M=(\mathbb N; \le^{\mathbb N}, 3^{\mathbb N})$. Then we can have a sentence $\varphi(x)=[x\le 3]$ where $x$ is a free variable. Thus $D=\{0, 1, 2, 3\}$ is definable.
## $\mathrm{II}$ Isomorphism
### $\it 2.1$ Definition
Def. **Isomorphism**. Let $M_1=(A_1; \cdots)$ and $M_2=(A_2; \cdots)$ be $\mathcal L$-structures. An isomorphism from $M_1$ to $M_2$ is a bijection $G:A_1\to A_2$ satisfying:
1. For each $n$-ary function-symbol $f$ and each $\mathbf a=(a_1, a_2, \cdots, a_n)\in A_1^n$, we have $G(f^{M_1}(\mathbf a))=f^{M_2}(G(\mathbf a))$.
2. For each $n$-ary relation symbol $R$ and each $\mathbf a\in A_1^n$, we have $\mathbf a\in R^{M_1}\iff G(\mathbf a)\in R^{M_2}$.
3. For each constant symbol $c$, we have $G(c^{M_1})=c^{M_2}$.
- If such $G$ exists, we say that $M_1\cong M_2$ denoting that $M_1$ and $M_2$ are isomorphic.
	- We write $G:M_1\to M_2$ denoting such $G:A_1\to A_2$.

Def. **Elementarily Equivalant**. Two $\mathcal L$-structures $M_1$ and $M_2$ are equivalent, written $M_1\equiv M_2$ if, for every $\mathcal L$-sentence $\varphi$, we have $M_1\models\varphi\iff M_2\models\varphi$.
- Prop. $M_1\cong M_2\implies M_1\equiv M_2$.
- **Proof**. Structural induction. Let $s'$ denote an assignment that $s'(x)=G(s(x))$. We want to show $M_1\models\varphi[s]\iff M_2\models\varphi[s']$.
	- For $\varphi=(t_1=t_2)$, $M_1\models\varphi[s]$ means that $\overline s(t_1)=\overline s(t_2)$, so $\overline s'(t_1)=\overline s'(t_2)$, and $M_2\models\varphi[s']$. For $\varphi=R(t_1, t_2, \cdots, t_n)$, $M_1\models\varphi[s]$ denote $\overline s(t_1, t_2, \cdots, t_n)\in R^{M_1}$, and $G\circ\overline s(t_1, t_2, \cdots, t_n)\in R^{M_2}$, so $M_2\models\varphi[s']$.
	- For $\varphi=\psi\land\theta$, $M_1\models\varphi[s]\iff M_1\models\psi[s]\land M_1\models\theta[s]$. So by induction $M_2\models\psi[s']\land M_2\models\theta[s']$, so the conclusion holds. For $\neg, \lor, \to$, the idea is the same.
	- For $\varphi=\forall x\theta$, $M_1\models\varphi[s]$ **iff** for every $d\in A_1$ we have $M_1\models\theta[s(x\mid d)]$. Notice that $G(s(x\mid d)(t))=\begin{cases} G(t)\quad t\neq x \\ G(d)\quad t=x \end{cases}=s'(x\mid G(d))(t)$, so by induction $M_1\models\theta[s(x\mid d)]\iff M_2\models\theta[s'(x\mid G(d))]$ (for every $d\in A_1$, the conclusion holds). For $\exists$, it is the same.
- eg. $(\mathbb R; \le)$ and $(\mathbb Q; \le)$ are not isomorphic, since they have different cardinalities. But they are elementarily equivalent. 
### $\it 2.2$ Automorphism
Def. **Automorphism**. Let $M=(A; \cdots)$ be an $\mathcal L$-structure. An automorphism of $M$ is an isomorphism $G:M\to M$.
- Corollary. $G\circ f(\mathbf a)=f\circ G(\mathbf a)$, indicating that $G$ and any $f^M$ are exchanging-valid.
- $R(\mathbf a) = R(G(\mathbf a))$. They share the same boolean value.
- 直觉。在自同构中，$G$ 给 $A$ 作变换之后，所运行的“解释器”是同一套 $f^M$ 和 $R^M$，因此其只能替换那些在 $f^M$ 和 $R^M$ 中“等价”的元素，一旦其“生态位”不同，那么自同构就失效了。

Prop. Let $G:M\to M$ be an automorphism of an $\mathcal L$-structure $M=(A; \cdots)$, and let $D\subseteq A^n$ be **definable without parameters**. Then for every $\mathbf a=(a_1, a_2, \cdots, a_n)\in A^n$, $\mathbf a\in D\iff G(\mathbf a)\in D$.
- **Analysis**. Since we need to analyze $\varphi$'s structure, induction is necessary. Consider $\mathbf a\in D\iff M\models\varphi(\mathbf a)$. Then in every step of $\varphi$'s formation, replace $s(x_i)$ with $G(s(x_i))$ would not change anything because $f^M$ and $R^M$ are the same.
- **Proof**. Consider the existence of a formula $\varphi(\mathbf a)$ which could exactly define $D$($\mathbf a\in D\iff M\models\varphi(\mathbf a)$). Explicitly write down the process of combination of $\varphi(\mathbf a)$ (Induction).
	- First, about terms. Let $s':=G\circ s$, first prove that $\overline s'(t)=G\circ\overline s(t)$ for any term $t$.
		- Variables. $\overline s'(x_i)=s'(x_i)=G\circ s(x_i)=G\circ\overline s(x_i)$.
		- Constants. $\overline s'(c)=c^M=\overline s(c)$ (constants have nothing to do with $s$).
		- Functions. Assume $\overline s'(t_i)=G\circ\overline s(t_i)$. Then $\overline s'(t)=\overline s'\circ f(t_1, t_2, \cdots, t_n)=f^M(\overline s'(t_1), \overline s'(t_2), \cdots, \overline s'(t_n))$. This is $f^M(G\circ\overline s(t_1), \cdots, G\circ\overline s(t_n))$. By automorphism, it's $G\circ f^M(\overline s(t_1), \cdots, \overline s(t_n))=G\circ\overline s(t)$.
	- Then, about atomic formulas. 
		- Equations. $M\models(t_1=t_2)[s]\iff\overline s(t_1)=\overline s(t_2)\iff\overline s'(t_1)=\overline s'(t_2)\iff M\models(t_1=t_2)[s']$.
		- Relation. $M\models R(t_1, t_2, \cdots, t_n)[s]\iff(\overline s(t_1), \cdots, \overline s(t_n))\in R^M$. This is exactly $(\overline s'(t_1), \cdots, \overline s'(t_n))\in R^M$, so $M\models R(t_1, t_2, \cdots, t_n)[s']$.
	- Then, about logic connectives.
		- `\neg`. Assume $M\models\varphi[s]\iff M\models\varphi[s']$. Then $M\not\models\neg\varphi[s]\iff M\models\varphi[s]$, the same as $s'$. This is trivial.
		- `\land`. Assume $M\models\varphi[s]\iff M\models\varphi[s']$ and $M\models\theta[s]\iff M\models\theta[s']$. This is exactly $M\models(\varphi\land\theta)[s]\iff M\models(\varphi\land\theta)[s']$.
		- ...
	- Finally, about the quantifiers.
		- `\forall`. We know $M\models\forall x\varphi[s]$. This means for every $d\in A$, we have $M\models\varphi[s(x\mid d)]$. By induction this holds **iff** $M\models\varphi[G\circ s(x\mid d)]$, where: $G\circ\overline s(x\mid d) \ (t)=\begin{cases} G\circ\overline s(t)\quad t\neq x \\ G(d)\quad t=x \end{cases}$. This is the same as $(G\circ s)(x\mid G(d))\ (t)=\begin{cases} (G\circ \overline s)(t)\quad t\neq x \\ G(d)\quad t=x \end{cases}$. Then $M\models\varphi[(G\circ s)(x\mid G(d))]$. Although $d\neq G(d)$ for most cases, they are actually equivalent when considering "for every $d\in A$" since $G$ is a bijection and equally enumerates through $A$. Hence this could be "for every $d\in A$, $M\models\varphi[(G\circ s)(x\mid d)]$". $\iff M\models\forall x\varphi[s']$. 
		- `\exists`. The same thing.
### $\it 2.3$ Real Examples of Isomorphisms
与命题逻辑不同的是，一阶逻辑是**真正处理数学结构的语言**。因此一定有必要掌握一些实际的例子，体会一阶逻辑是如何用于处理真正的数学问题的。

Example(**Definability and Automorphism**). Consider a language $(\mathbb Z; f)$ where $f$ is writte as $f(n)=n+1$. Then we apply a model $M=(\mathbb Z; f)$ which explain the language to its usual meaning. The map $\sigma:\mathbb Z\to\mathbb Z$ given by $\sigma(n)=n+1$ is a bijection, and $\sigma(f(n))=f(\sigma(n))=n+2$. So $\sigma$ describes an automorphism.
- Problem. Given only $f$, can $\mathbb E:=\{n\in\mathbb Z\mid n\text{ is even}\}$ be defined(only with $=, \neg, \land, \lor, \to, \forall, \exists$)? The answer is **NO**.
	- **Proof**. Since $\sigma$ is an automorphism, then $x\in\mathbb E\iff\sigma(x)\in\mathbb E$ and this is ridiculous.
- This is extremely important: If one can find any $\sigma$ that is automorphic, then any set $D$ should satisfy $a\in D\iff G(a)\in D$ before itself become definable(due to the lemma).

Example(Definability and language). Consider $M_1=(\mathbb Q; <)$ and $M_2=(\mathbb Q; +, \times)$.
- $M_1$ is a poor system: Due to the requirements, any parallel transfer(not going against the orders) is an automorphism(since only one `<` provides poor information). So any particular number cannot be distinguished.
- $M_2$ is much richer: We can define $0$ by $r+r=r$, $1$ by $r^2=r\land r\neq 0$ and every finite particular integer.
- This is **quite algebraic**: Richer language provides more abundant operations, and one can do much more things with logic connectives and the **quantifiers**. If regarding the language as algebra functions, it would be helpful.
	- For a model $M$, many operations came out by the typical structure $A$ itself.

Example(Quantifiers). Quantifiers allow us to do **search** operations inside $A$. In $M=(\mathbb N; +)$, writing $\exists y(x=y+y)$ defines if $x$ is even. Here bound variables(forall, exists) do the searching work while free variables are actually "not free" since they play the role of the input(in definability).

Example. For finite models $M$, one sentence is useful:
$$
\exists x_1\exists x_2\cdots\exists x_n\left(\bigwedge\limits_{i<j}(x_i\neq x_j)\land\forall y\left(\bigvee_{i=1}^n(y=x_i)\right)\right)
$$
This sentence says that $|A^M|=n$.

Example. $M_1=(\mathbb Q; <)$ and $M_2=(\mathbb R; <)$. They are not isomorphic(since $\overline{\overline{\mathbb Q}}\neq\overline{\overline{{\mathbb R}}}$), but they are elementarily equivalent since the total order in $\mathbb Q$ and $\mathbb R$ appears nothing different.
### $\it 2.4$ Theories and Elementary Classes
Def. **Theory**. An $\mathcal L$-theory is a set if $\mathcal L$-sentences. For an $\mathcal L$-structure $M$ and an $\mathcal L$-theory $\Sigma$, we write $M\models\Sigma$ if $M\models\varphi$ for every $\varphi\in\Sigma$.

Def. **Elementary Class**. A class $\mathcal K$ of $\mathcal L$-structures is an elementary class if there is an $\mathcal L$-theory such that, for every $\mathcal L$-structure $M$, $M\in\mathcal K\iff M\models\Sigma$.
