---
publish: true
---

# 数理逻辑 Note 1
## $\mathrm I$ 命题逻辑 Propositional Logic
> 在具体展开有关命题逻辑的讨论之前，需要弄清楚逻辑所使用的对象。与集合论一致，我认为不存在“彻底的原子命题”，即命题与集合一样永远是相对的概念。在集合论中，元素与集合、集合的集合等本质上是相同的事物，我们说“集合”是为了呈现其中的层次关系。命题也是，如果一个命题永远需要其他命题来确定真假，那在建模上不妨根据研究的问题设置“无法再细分下去”的**原子命题**作为我们推理的起点，其中研究的重点在于逻辑联结词所带来的**层次关系**以及**构筑方式**等。

命题逻辑即为只包含原子命题与逻辑联结词（组成句子、模型、理论）的基础逻辑，是最简单的情形，在命题逻辑中，语法和语义表现为同构，难以区分，相当于同一套推理逻辑。
### $\it 1.1$ 命题逻辑的定义 Prop Logic - Defining Game Rules
- 首先定义命题逻辑的最基础组成部分：原子命题、句子、逻辑联结词。
Def. **Prop Logic 命题逻辑**. A prop logic(propositional logic) is built from **atomic props**, logic connectives  and parenthesis. 
- Use $A, B, C, \cdots$ to represent atomic props.
- logic connectives: $\neg, \land, \lor, \to$.
- parenthesis: $($ and $)$.
- 意思是，命题逻辑由原子命题、逻辑连接词以及括号组成。命题逻辑规定了逻辑游戏的规则：使用原子命题 $A_i$ 作为一种对研究具体问题**最小单位**的建模，并具体规定了 $\neg, \land, \lor, \to$ 等运算符的操作规则。

Def. **Language 语言**. A language(or a **PL-language**) is a set of atomic props. 
- eg. $\mathcal L=\{A_i\mid i\in\mathbb N\}$, where $A_i$ are atomic props.
- **语言**指所涉及的所有原子命题的集合。
	- 问题：$\overline{\overline{L}}=\aleph_0$ 或 $\overline{\overline{L}}=\aleph$ 是合法的吗？**是**，可以是任意的基数，但句子必须是**有限**的。

Algo. Recursively define **Sentence**:
- Every $A_i\in\mathcal L$ is a sentence;
- If $\varphi$ is a sentence, then $(\neg\varphi)$ is a sentence;
- If $\varphi, \psi$ are sentences, then $(\varphi\land\psi), (\varphi\lor\psi), (\varphi\to\psi)$ are all sentences;
- For a string $\mathbf S$ with **finite** length, $\mathbf S$ is a sentence if and only if $\mathbf S$ could be constructed from the three rules above.
- 句子的定义是递归的、构造的。有关为什么要加括号：为了保证 Unique Readability 或 Unique Decomposition。

Prop. $\texttt{cnt['('] == cnt[')']}$ formally let $L(\varphi)$ be the numbers of `(` in sentence $\varphi$, $R(\varphi)$ be the numbers of `)` in sentence $\varphi$. then $L(\varphi)=R(\varphi)$. 
- **Proof**. For atomic props, $L(A_i)=R(A_i)=0$. Every complex sentence exists a way to construct it from atomic props. We will prove that the constructing methods WILL NOT change $R(\varphi)-L(\varphi)$. Detailed, $[L(\varphi)=R(\varphi)]\Rightarrow[L((\neg\varphi))=R((\neg\varphi))]$, and $[L(\varphi_1)=R(\varphi_1)]\land[L(\varphi_2)=R(\varphi_2)]\Rightarrow [L((\varphi_1\otimes\varphi_2))=R((\varphi_1\otimes\varphi_2))]$, where $\otimes\in\{\land, \lor, \to\}$.
- 对于任意的句子，其满足左右括号数量相等（显然），证明使用归纳法。

Prop. **句子递归构造的等价条件 Unique Decomposition**. Exactly one of the following holds, for any $\mathcal L$-sentence $\varphi$:
1. $\varphi$ is a atomic prop.
2. There exists an $\mathcal L$-sentence $\psi$ such that $\varphi=(\neg\psi)$.
3. There exists $\mathcal L$-sentences $\psi, \theta$ such that $\varphi=(\psi\otimes\theta)$, where $\otimes\in\{\land, \lor, \to\}$.
	- $\psi$ in (2) and $\psi, \theta$ in (3) are **uniquely determined** (by the way $\varphi$ was constructed). 意思是任意的 $\mathcal L$-句子要么写成 $(\neg\psi)$，要么写成 $(\psi\otimes\theta)$，要么本身就是原子命题，具体是哪一种取决于上一步构造的方式。
### $\it 1.2$ 命题模型（语义） Prop Models
语义部分，命题逻辑的模型简单地表示为 $\mathcal L$ 的一个子集。在模型 $M$ 的“驱动”下，每个句子非真即假。

Def. **Model 模型**. A model is a subset of $\mathcal L$. It could be empty. We write $M\models\varphi$ **iff**(recursively):
- $M\models A_1$ **iff** $A\in M$, where $A$ is an atomic prop.
- $M\models\neg\varphi$ **iff** $M\not\models\varphi$.
- $M\models(\varphi\land\theta)$ **iff** $M\models\varphi$ and $M\models\theta$.
- $M\models(\varphi\lor\theta)$ **iff** $M\models\varphi$ or $M\models\theta$(or both).
- $M\models(\varphi\to\theta)$ **iff** $M\not\models\varphi$ of $M\models\theta$.
-  形式上，模型的含义很简单：就是 $\mathcal L$ 的一个子集（可为空）。
- 为什么需要模型？模型通常用于语义分析，一个模型 $M$ 中的所有原子命题为真，所有不处于 $M$ 中的原子命题为假。说白了一个模型 $M$ 就是一种对 $A_1, A_2, \cdots, A_n$ 的赋值（反映真实世界的某种情况）。在命题逻辑中，任何一个句子 $\varphi$ 可以视作 $\mathrm{bool}^n\to\mathrm{bool}$ 的布尔函数（或者无穷个 $A_i$，总之），因此每个句子非真即假。

> **Intuition**. Prop models repersent some kind of **possibility** or **T/F assignment**. We believe atomic props in a model $M$ is true, while atomic props not in $M$ is false.
> The use of a model is to check, (under such belief) whether a sentence is true, written as $M\models\varphi$.

Def. (Sgn) $A(\varphi)$ represents all atomic props appeared in $\varphi$.

Def. T/F Calculation. 句子的赋值（**模型的另一种定义方式**）。设一个句子 $\varphi$ 使用的原子命题全部来自 $B\subseteq\mathcal L$，对于所有满足 $A(\varphi)\subseteq B$ 的 $\varphi$，可以使用函数 $g:B\to\mathrm{bool}$ 判定**单个原子命题**的真假（如 $g(A_1)=1, g(A_2)=0$）。这里的函数 $g$ 被称为**真值赋值**（truth assignment）。也就是说 $g$ 是 $B$ 中所有原子命题的一种赋值。然后定义 $v_g(\varphi)$ 表示在 $g$ 对 $B$ 中元素的赋值下，句子 $\varphi$ 的真假。具体而言依旧遵循归纳：
- Define $v_g(\varphi)$ recursively(based on unique readability):
	- If $\varphi=A_i$, then $v_g(\varphi)=g(A_i)$;
	- If $\varphi=(\neg\psi)$, then $v_g(\varphi)=\mathrm 1$ **iff** $v_g(\psi)=\mathrm 0$;
	- If $\varphi=(\psi\land\theta)$, then $v_g(\varphi)=\mathrm 1$ **iff** $v_g(\psi)=1$ and $v_g(\theta)=1$;
	- If $\varphi=(\psi\lor\theta)$, then $v_g(\varphi)=\mathrm 1$ **iff** $v_g(\psi)=1$ or $v_g(\theta)=1$;
	- If $\varphi=(\psi\to\theta)$, then $v_g(\varphi)=1$ **iff** $v_g(\psi)=0$ or $v_g(\theta)=1$.

Def. **Valid, Satisfiable**. 
- $\varphi$ is **valid**（**有效**，或**永真**）, if $\varphi$ is true in **every** $\mathcal L$-model.
- $\varphi$ is **satisfiable**（可满足）, if exists a (particular) model $M$ such that $M\models\varphi$.
- $\varphi$ is **not satisfiable**（不可满足）, if there **does not** exists any model $M$ such that $M\models\varphi$.

Lemma. 真值赋值 $v_g(\varphi)$ 与 model 的等价性. 若定义域 $B\subseteq\mathcal L$，存在真值赋值 $f:B\to\mathrm{bool}$，那么 $v_f(\varphi)=1$ 当且仅当 $M_f\models\varphi$。（由其递归定义可以直接证明）
### $\it 1.3$ 理论与证明（语法） Theories and Proofs
语法部分，讲述理论及其推导形式。

Attention! The word `theory` and `proof` here are specific. 这里的**理论**与**证明**并不是一般理解的意思。

Def. **Theory 理论**. Formally a theory is a set of sentences which implies "assume those sentences are true". 
- **理论**是一个句子的集合，代表“假设这些句子是真的”。
- Usually write $\Sigma$ as a set of sentences.
- 基？一个理论可以存在一个“最小独立生成集”，就好比向量空间的基。但大多数理论并不是独立的，其含有许多冗余的句子。我们可以抽象出一个“极小生成基”（去掉任何句子，都会丢失信息）。

Def. **Proof 证明**. We need to formalize proof. Formally a proof is a sequence of sentences $\theta_1, \theta_2, \cdots, \theta_n$, where $\theta_n$ represents the conclusion. We write $\Sigma\vdash\varphi$ and say "$\Sigma$ proves $\varphi$" if exists a set of sentences $\theta_1, \theta_2, \cdots, \theta_n$:
- $\theta_n=\varphi$ as the conclusion.
- For any sentence $\theta_m$, at least one of the following holds:
	- $\theta_m$ is valid(true in any circumstances);
	- $\theta_m\in\Sigma$, which means $\theta_m$ is assumed true;
	- (**MP Rule**) There exists sentence $\theta_k$ and $(\theta_k\to\theta_m)$ **previously**(formally, the index is smaller).
**Attention**. Only $\theta_k$ and $\theta_k\to\theta_m$ appeared previously can we directly write $\theta_m$ next. (It's a formalized quest)
- Trick: write a complex valid sentence and formally follow the rule.
- 语法的 MP 规则是相当死板的，仅有属于 $\Sigma$ 本身、恒真（这里定义结束了语义部分，这样做在命题逻辑里是合法的）或由 MP 规则推导来的语句可以写作接下来的证明。
- 从代数看，“证明”这个动作在语法中更像一种代数生成，其**不考虑语义的真伪**（只不过命题逻辑里借用了语义的“恒真”概念）。

Def. **Tautology**【恒真命题】We say sentence $\varphi$ is a tautology if $\varnothing\vdash\varphi$ or just write $\vdash\varphi$.

$\color{Red}\textbf{Warning.}$  $\color{Cyan}\textbf{Currently theorms and models appear nothing different, but only now}$. This is because prop logic is the simplist logic. **Pay attention to this**.
- 在简单的命题逻辑中，模型与理论看起来有一致的内涵，但仅当现在。 命题逻辑中语法和语义的对称性，进入一阶逻辑后会被打破。届时“模型”和“理论”的差别将变得极其显著，完备性定理的证明也会变得非平凡。现在看到的“一样”，是命题逻辑特有的简单性。

Def. **Consistency 一致性**. Define an $\mathcal L$-theory $\Sigma$ **inconistent** if $\Sigma\vdash\varphi$ for **any** sentence $\varphi$. Otherwise we say $\Sigma$ is **consistent**.
- 从定义看，**不一致**意为 $\Sigma$ 能证明任何句子，实际上可以证明：其充要条件为，$\Sigma$ 能证明一个矛盾式 $A\land\neg A$（爆炸原理）。
- 一致与否是理论 $\Sigma$ 的重要性质，其定义为**是否能证明任意句子**（根据语法的机械推理规则），这与“内部存在矛盾”是等价的。

（**爆炸原理**）Let $A\in\mathcal L$ and suppose $\Sigma\vdash(A\land\neg A)$, then $\Sigma$ is inconsistent.
- **Analysis**. Our aim is to prove $\Sigma\vdash\varphi$ for **any** $\varphi$, so consider sth **formally** instead of meaningful.
	- 要证明如此荒谬的定理，我们需要考虑一些邪门的形式逻辑。
- **Proof**. Let $\varphi$ be any sentence. There exists a proof $\theta_1, \theta_2, \cdots, \theta_n$ for $A\land\neg A$. Notice that $(A\land\neg A)\to\varphi$ is valid(because whether $A$ is true or false, $A\land\neg A$ is false), write this after. We claim that $\theta_1, \theta_2, \cdots, \theta_n, (A\land\neg A)\to\varphi, \varphi$ is a proof of $\varphi$ from $\Sigma$.
- **Intuition**. 此时任何证明处于**既对又错**的状态，只能用魔法对抗魔法，真假失去了意义。于是我们的结论是：如果一个理论 $\Sigma$ 能证明一切，那就能证明形如 $A\land\neg A$ 这样的矛盾；如果能证明矛盾，那就能证明一切（已经由 inconsistent 证明）。因此其逆否命题为，如果 $\Sigma$ 不能证明一切，那么 $\Sigma$ 就不存在内部矛盾。这便是 consistent 这个概念为何要被定义为“不能证明一切命题”。

### $\it 1.4$ 完备性与紧致性 Completeness and Compactness
- 完备性：在命题逻辑中，理论一致等价于可以为真（存在成真赋值）。
- 紧致性：在命题逻辑中，理论一致当且仅当其任何有限部分是一致的。

【先给出 Completeness Thm 与 Compactness Thm】
Thm. **Completeness Theorem 完备性定理**. $\Sigma$ is consistent if and only if there exists $M$ such that $M\models\Sigma$. 
- 一个理论是一致的，当且仅当其可满足（存在模型 $M$ 使得 $M\models\Sigma$）。
- $M\models\Sigma$ 推出 consistent 较为好证，逆向需要用到 Zorn's Lemma。整个证明分为三步：证明 maximally consistent 的 $\Sigma'$ 存在，证明存在模型 $M$ 满足 $\Sigma'$，从而证明 $M\models\Sigma$。

Thm. **Compactness Theorem 紧致性定理**. $\Sigma$ is consistent if and only if for any $\Sigma_0\in\Sigma$ such that $|\Sigma_0|$ is finite, there exists $M_0$ such that $M_0\models\Sigma_0$.
- 一个理论是一致的，当且仅当每一个有限子集都是可满足的。这个命题紧跟完备性定理。
#### $\it 1.4.0$ 演绎闭包，极大一致理论 Deductive Closure, Maximally Consistent
Prop. **Deductive Closure 演绎闭包**. If $\Sigma$ is consistent, then $\Gamma:=\{\varphi\mid\Sigma\vdash\varphi\}$ is consistent. $\Gamma$ is called **the deductive closure of $\Sigma$**.
- Proof. Suppose $\Gamma$ is inconsistent, which implies $\Gamma\vdash(A\land\neg A)$. There exists a proof $\theta_1, \theta_2, \cdots, \theta_n$ for this. For every $\theta_i$,  we have $\Sigma\vdash\theta_i$. Write every proof $\Sigma\vdash\theta_i$ and then write $\theta_1, \theta_2, \cdots, \theta_n$ and then we proved $A\land\neg A$ from $\Sigma$. So $\Sigma$ is inconsistent, that's false. So $\Gamma$ is consistent.

Def. **Maximally Consistent 极大一致性**. We say a theory $\Sigma$ is Maximally Consistent, if there does not exist a $\Sigma'\supsetneq\Sigma$ s.t. $\Sigma'$ is consistent.
- 意思是，不存在更大的一致集合。这相当于对 $\mathcal L$ 中的所有元素表态（否则可以进行更具体的表态）且由 $\mathcal L$ 生成的所有命题（或者其逆否）都处于 $\Sigma$ 中。**这是一种抽象的存在**，而非需要构造的解，其存在性由 **Zorn's Lemma** 保证。
- **Intuition**. 当前的 AI4math。其相当于在尽可能挖掘一个已知的演绎闭包内容，但足够强的公理系统中一定存在不可证明也不可证伪的命题（如 ZFC 公理与连续统假设），根本碰不到“极大”的屏障。

Thm. **Deduction Theorem 演绎**. If $\Sigma\cup\{\theta\}\vdash\varphi$, then $\Sigma\vdash(\theta\to\varphi)$.
- **Proof**(Summarize method). Consider a sequence of proof $\theta_1, \theta_2, \cdots, \theta_n$ where $\theta_n=\varphi$. Assume $\Sigma\vdash(\theta\to\theta_i)$ for every $i$.
	- First prove $\Sigma\vdash(\theta\to\theta_1)$. $\theta_1$ must be valid, inside $\Sigma$ or $\theta$.
		- If $\theta_1$ is valid, then $\theta\to\theta_1$ is valid. Thus $\Sigma\vdash\theta\to\theta_1$.
		- If $\theta_1=\theta$, then $\theta\to\theta$ is valid.
		- If $\theta_1\in\Sigma$, then $\Sigma\vdash\theta_1$. Thus $\Sigma\vdash(\theta\to\theta_1)$.
	- Then, assume $\Sigma\vdash(\theta\to\theta_j)$ for $j<i$. We want $\Sigma\to(\theta\to\theta_i)$.
		- If $\theta_i$ is valid, then $\theta\to\theta_i$ is valid.
		- If exist $\theta_j(j<i)$ and $\theta_k=\theta_j\to\theta_i$, by assumption we have $\Sigma\vdash(\theta\to\theta_j)$. Thus $\Sigma\vdash(\theta\to(\theta_j\to\theta_i))$(Which is $\Sigma\vdash(\theta\to\theta_k)$). Thus $\Sigma\vdash(\theta\to\theta_i)$.
		- Hence $\Sigma\vdash(\theta\to\varphi)$.
- **Intuition**. 可以看出，从 $\Sigma\cup\{\theta\}$ 中剥离 $\theta$ 对证明的贡献是极其困难的，因为在证明的中间若没有 $\theta$ 的贡献则整个证明根本无法进行。解决办法是，将具体证明中（与 $\theta$ 有关联的）$\theta_i$ 替换为 $(\theta\to\theta_i)$，这是一种形式上的等价变换，即 $\Sigma\cup\{\theta\}\vdash X$ 和 $\Sigma\vdash(\theta\to X)$ 是**完全等价**的，只是写作不同的形式（这在证明序列中体现尤为明显）。这个证明为了剥离 $\theta$ 的影响，构造了 $(\theta\to\theta_i)$ 的这一等价形式，利用归纳完成了证明。
	- 或者更彻底一点，若 $\Sigma=\{\theta_1, \theta_2, \cdots, \theta_n\}$，那么 $\Sigma\vdash X$ 与 $\left(\bigwedge\limits_{i=1}^n\theta_i\right)\Rightarrow X$ 完全等价。
	- 写作式子 $(\theta_1\to(\theta_2\to(\theta_3\to(\cdots\to X))))$ 永真或者 $\neg\theta_1\lor\neg\theta_2\lor\cdots\lor\neg\theta_n\lor X$ 永真更直观，相当于将最里层的 $(\theta_n\to X)$ 提出来了。这个过程的名字叫 **Currying**。

#### $\it 1.4.1$ 佐恩引理 Zorn's Lemma
> 第一步，证明存在极大一致理论。

Lemma. **Zorn's Lemma 佐恩引理**. 
- Def. **Partially Ordered Set 偏序**. A partially ordered set $(P, \le)$ is a set $P$ and a binary relation $\le$ which satisfies:
	- Reflexivity: $x\le x$ for every $x\in P$;
	- Antisymmetry: If $x\le y$ and $y\le x$, then $x=y$;
	- Transitivity: If $x\le y$ and $y\le z$, then $x\le z$.
	- Then the relation $\le$ is called a **partial order** on $P$.
		- eg. $(\mathbb N, \le)$, $(\mathcal P(\mathbb N), \subseteq)$.
- Def. **Chain 链**. Let $(P, \le)$ be a partially ordered set. A chain is a subset $C\subseteq P$ in which any two elementa are comparable(for all $x, y\in C$, either $x\le y$ or $y\le x$).
	- An **upper bound** for a chain(for a chain!) $C\subseteq P$ is an element $a\in P$ s.t. $x\le a$ for every $x\in C$.
	- An element $m\in P$ is **maximal**(for the whole $P$!) if there is no $x\in P$ s.t. $m\le x$ and $m\neq x$.
- Then we have **Zorn's Lemma**: Let $(P, \le)$ be a non-empty partially ordered set. If every chain in $P$ has an upperbound in $P$, then $P$ has a maximal element.
	- 佐恩引理有一系列等价形式，通常我们默认其正确性。在这里引入佐恩引理是为了证明存在极大一致理论。

Thm. **Lindenbaum's Theorem(The existence of maximally consistent theory)**. Every consistent $\mathcal L$-theory $\Sigma$ is contained in a maximally consistent $\mathcal L$-theory $\Sigma'$.
- **Proof**. Let $S:=\{\Gamma\mid\Gamma\text{ is a consistent \cal L-\rm theory and }\Sigma\subseteq\Gamma\}$ ordered by inclusion($\subseteq$). Since $\Sigma$ is consistent, $\Sigma\in S$, so $S$ is non-empty. We need to prove that every chain $C$ has an upper bound. Let $\Gamma_C:=\bigcup\limits_{\Gamma\in C}\Gamma$.
	- Suppose $\Gamma_C$ is inconsistent, then $\Gamma_C\vdash A\land\neg A$. Since a proof is **finite**, it uses finitely many assumptions from $\Gamma_C$. Each of them belongs to some member of $C$. Because $C$ is ordered, there exists a $\Gamma_*\in C$ that contains all these assumptions. Then $\Gamma_*\vdash A\land\neg A$. This contradicts the consistency of $\Gamma_*\in C\subseteq P$. Thus $\Gamma_C$ is consistent as an upper bound of $C$.
- By Zorn's Lemma, $S$ has a maximal element $\Sigma'\in S$. Thus $\Sigma'$ is consistent and contains $\Sigma$. Any consistent $\Sigma''\supseteq\Sigma'$ would be in $S$, contradicting the maximality.
- Thus $\Sigma'$ is **maximally consistent**. 至此我们完成了第一步，证明极大一致理论的存在性。

#### $\it 1.4.2$ 可靠性定理 Soundness Theorem
Prop.  Suppose $\Sigma$ is maximally consistent, then:
- For every $\mathcal L$-sentence $\varphi$, exactly one of $\varphi$ and $\neg\varphi$ is in $\Sigma$.
	- **Intuition**. 因为 $\Sigma$ 极大一致，这直觉上意味着 $\Sigma$ 能证明所有东西且保持自身一致（这直觉上就等价，所有原子命题的真假已经明确，否则一定存在模糊的命题，也就是存在更大的一致理论）。
	- **Proof**. If $\varphi\in\Sigma$ and $\neg\varphi\in\Sigma$, then this is dumb. So we want to prove that, if neither $\varphi\in\Sigma$ nor $\neg\varphi\in\Sigma$, there exists a consistent $\Sigma'\supseteq\Sigma$.
		- Suppose $\varphi$ and $\neg\varphi$ are neither contained in $\Sigma$. Let $\Sigma_1:=\Sigma\cup\{\varphi\}$, $\Sigma_2:=\Sigma\cup\{\neg\varphi\}$.
		- **Intuition**. 这里我们心里知道 $\Sigma_1$ 和 $\Sigma_2$ 至少有一个是一致的，所以我们希望根据条件导出矛盾。
		- Because $\Sigma$ is maximally consistent, so $\Sigma_1$ and $\Sigma_2$ are both inconsistent! Which is: $\Sigma_1\vdash A\land\neg A$, $\Sigma_2\vdash A\land\neg A$.
		- By **deduction thm**, we have $\Sigma\vdash\varphi\to(A\land\neg A), \Sigma\vdash\neg\varphi\to(A\land\neg A)$. Combine: $\Sigma\vdash(\varphi\lor\neg\varphi)\to(A\land\neg A)$. Notice that $\varphi\lor\neg\varphi$ is **valid**. So $\Sigma\vdash A\land\neg A$. This is false.
- For all $\mathcal L$-sentence $\varphi$ and $\psi$, $(\varphi\land\psi)\in\Sigma$ **iff** $\varphi\in\Sigma$ and $\psi\in\Sigma$.
	- Proof.

Lemma. **Soundness Theorem 可靠性定理**. Suppose $M\models\Sigma$. If $\Sigma\vdash\varphi$, then $M\models\varphi$.
- **Proof**. Prove $M\models\theta_i$ from $1$ to $n$ for the proof $\theta_1, \theta_2, \cdots, \theta_n=\varphi$.(By construction)
	- $M\models\theta_k\to\theta_j, M\models\theta_k$ then $M\models(\theta_k\land(\theta_k\to\theta_j))\Rightarrow M\models\theta_j$.
- “证明出来的都是对的”。

#### $\it 1.4.3$ 完备性定理 Completeness Theorem
> 再次引入完备性定理、紧致性定理：

Thm. **Completeness Theorem 完备性定理**. A theory $\Sigma$ is consistent **iff** $\Sigma$ is satisfiable(exists model $M$ s.t. $M\models\Sigma$).
- **Proof**. **Necessity**. This is easy. Suppose $M\models\Sigma$, but $\Sigma\vdash A\land\neg A$. By soundness $M\models A\land\neg A$, this would never happen.
- **Sufficiency**. This is the important part. Suppose $\Sigma$ is consistent. We know that there exists a maximally consistent theory $\Sigma'\supseteq\Sigma$.
	- 接下来我们证明存在 $M\models\Sigma'$。既然理论的形式推导是从基本元开始的，而 $\Sigma'$ 是极大的，所以关键在于从原子命题的分析开始。
	- Extract atomic props which is considered true: $M := \{A\in\mathcal L\mid A\in\Sigma'\}$. We want to show that $M\models\varphi$ **iff** $\varphi\in\Sigma'$.
	- Detailedly, follow the construction rules for sentences.(Here we only consider $\neg$ and $\land$ because this is complete)
		- $\varphi=A$, then $M\models A\iff A\in M\iff A\in\Sigma'$.
		- $\varphi=(\neg\psi)$ and exactly $\psi$ or $\neg\psi$ is in $\Sigma'$. Say $M\models\psi$ and $\varphi=(\neg\psi)$, so $M\models\psi\iff M\not\models\varphi\iff\varphi\notin\Sigma'\iff M\models\neg\varphi$. (By prop "exactly one of $\varphi$ and $\neg\varphi$ is in $\Sigma'$").
		- $\varphi=(\psi\land\theta)$. So $\begin{aligned}M\models(\psi\land\theta)&\iff (M\models\psi)\land(M\models\theta)\\ &\iff(\psi\in\Sigma')\land(\theta\in\Sigma')\\ &\iff(\psi\land\theta)\in\Sigma'\end{aligned}$.
	- Thus, $M\models\Sigma'$. Finally $M\models\Sigma$.

（极大一致理论与模型为**双射关系**）若 $\Sigma'$ 是极大一致理论，则其对应唯一的 $M$ 使得 $M\models\Sigma'$。
- 证明，首先证明存在 $M$ 使得 $M\models\Sigma'$，这个刚才证过了。
- 然后证明若 $M_1\models\Sigma'$ 且 $M_2\models\Sigma'$，则可以推出 $M_1=M_2$：
	- 考虑任意原子命题 $A_i$，根据引理，$A_i\in\Sigma'$ 与 $\neg A_i\in\Sigma'$ 恰有一成立。若 $A_i\in\Sigma'$，则 $A_i\in M_1$ 且 $A_i\in M_2$；若 $\neg A_i\in\Sigma'$，则 $A_i\notin M_1$ 且 $A_i\notin M_2$。因此 $M_1=M_2$。

#### $\it 1.4.4$ 紧致性定理 Compactness Theorem
(Use Completeness Thm to prove Compactness Thm)

Thm. **Compactness Theorem 紧致性定理**. An $\mathcal L$-theory $\Sigma$ is satisfiable **iff** it is finitely satisfiable(for every **finite** subset $\Sigma_0\subseteq\Sigma$, $\Sigma_0$ is satisfiable).
- **Proof**. **Necessity** is trivial.
- **Sufficiency**. Suppose $\Sigma$ is finitely satisfiable but $\Sigma$ itself is not satisfiable, which is $\Sigma\vdash A\land\neg A$(By completeness theory). We have the proof $\theta_1, \theta_2, \cdots, \theta_n$ which is **finite**?! Let $\Sigma_0$ be the assumptions used in this proof. Then $\Sigma_0$ is finite. Exists $M\models\Sigma_0$ then $M\models A\land\neg A$ which would never happen.

#### $\it 1.4.5$ 有关完备性的小结
**Intuition**. 在此说明完备性定理的证明动机是什么。理论的“一致”是一个很宽松的约束，其只要求内部不出现矛盾，也就是原子命题的真假组合只要不矛盾即可，除此以外不做任何限制。因此一个理论 $\Sigma$ 是一致的，但其原子命题有很多种可能的赋值使其成立。而极大一致理论就是一个极强的约束：其跟模型本身是**本质相同的**（存在极大一致理论与模型之间的双射）。此时极大一致理论对每一个句子都作了表态，不允许有任何的模糊（比如理论告诉你 $A\lor B$ 是对的，但不告诉你 $A$ 和 $B$ 哪个才是真正的正确），这完全相当于模型给每个原子命题赋值。

那如何由 $\Sigma$ 得到 $\Sigma'$ 呢？在离散的语境中，我们通常的想法是由 $\Sigma$ 经过某种构造算法得到 $\Sigma'$（尽管很可能极其复杂），但也有“根本构造不了”的情形，比如 $\Sigma$ 是无穷集，甚至不可数。因此 Zorn's Lemma 就直接绕开了 $\Sigma'$ 的构造问题，直接由其定理保证 $\Sigma'$ 的存在性，免去了复杂的构造。

于是一旦证明了 $\Sigma'$ 的存在性，我们就只用利用 $\Sigma'$ 的“极大”性质将其与 $M\models\Sigma'$ 划等号了。

### $\it 1.N$  有关佐恩引理 About Zorn's Lemma
佐恩引理拥有一系列等价形式，其最基础的形式就是**选择公理**（ZFC 的 C），还等价于良序定理、Tarski 不动点定理、甚至“每个向量空间都有基”。
