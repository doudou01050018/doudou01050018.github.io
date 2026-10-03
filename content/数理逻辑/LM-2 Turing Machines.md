---
publish: true
---

# 数理逻辑 Note 2
## $\mathrm{II}$ 图灵机与可判定性 Turing Machine and Decidability
> 这一讲在数理逻辑课程中的地位是工具性的，其主要为后续哥德尔不完备定理的证明提供一个对照版本，而不是深入讲述 TCS。需要着重掌握的内容是：图灵机的形式化定义、手搓图灵机的一些小技巧、可判定性的定义、图灵机的扩展、停机问题。以及“神谕图灵机”。

在最后涉及一些有关自指问题的思考。
### $\it 2.1$ 图灵机的形式化定义 Turing Machine
Def. **Turing Machine 图灵机**. Turing Machine is an infinite tape with characters one by one. It is formally defined as a tuple $\mathbf T=(Q, \mathcal A, \mathcal B, \delta, q_0, q_1, q_2)$.
- $Q$: The set for all possible states(As an automaton);
- $\mathcal A$: The set of **input alphabet**(Which means all characters for the problem input);
- $\mathcal B$: The set of **tape alphabet**(Which means all possible characters in the problem);
- $\delta$: Transform function, defined as $\delta:(Q-\{q_1, q_2\})\times\mathcal B\to Q\times\mathcal B\times\{\mathrm L, \mathrm R\}$.
	- $\delta(q, c)=(q', c', \mathrm R)$,  means if $\mathbf T$ in state $q$ reads $c$, then $\mathbf T$ write $c'$(to replace $c$), change to state $q'$ then move **right**(left if $\mathrm L$).
- $q_0, q_1, q_2\in Q$ are distinct states. $q_0$ is the initial state, $q_1$ is the **accepted** state(if $q=q_1$, then the machine stops and report **accepted**), $q_2$ is the **rejected** state(if $q=q_2$ then the machine stops and report **rejected**).
- 图灵机形式上可以被描述为七元组 $\mathbf T=(Q, \mathcal A, \mathcal B, \delta, q_0, q_1, q_2)$（主要是参与一些计数问题或者基数问题）。
- ***Attention***. $\color{Red}\textbf{图灵机是算法的形式化定义}$。

Def. **Halt 停机**. If the Turing Machine is currently at $q_1$ or $q_2$, then it immediately return accept or reject, this is called **halt**. Any machine halts if the machine stops after **finite** steps.
- A Turing Machine may never stops.

Diagrams for Turing Machines. (Which is to illustrate the automaton)

**Important Fact(Church-Turing Thesis)**. What computers can do is all what Turing machines can do.

#### $\it 2.1.1$ 图灵机构造技巧 Construction Tricks
一般来说，图灵机的状态机要求是有限的（不能将信息编码到状态机内），因此关键的做法是使用不同的字符（比如对 $1$ 进行标记，可以使用 $1', 1_A, 1_M$ 之类的表示不同处理状态的 $1$）而非多复杂的状态机。
- 图灵机的构造一般不考虑复杂度（因为这是理论模型，实际上没人会这么写程序），通常读写头都是在不同的处理部分两段横跳并且做标记（充分利用字符集）。以及会用一些“子程序”来简化（sub-routine）。

可以适当记录一些图灵机构造的小技巧。

### $\it 2.2$ 可判定性 Decidability
Sgns. Let $\{0, 1\}^*$ denote the set of all binary(0, 1) strings of any length(possibly $0$). $|s|$ denote the length of a binary string $s$.

Def. **Decidable 可判定的**. A set $S\subseteq\{0, 1\}^*$ is **decidable** if there exists a Turing Machine $\mathbf T$ which halts on every input $w$, accepting if $w\in S$ and rejecting if $w\notin S$. We call $\mathbf T$ a **decider** for $S$.
- eg. $E=\{s\in\{0, 1\}^*\mid |s|\text{ is even}\}$ is decidable.

#### $\it 2.2.1$ 存在不可判定的集合吗？Existence of a undecidable set?
The answer is **YES**. Randomly choose a subset $S\subseteq\{0, 1\}^*$, the probability of undecidable is $1$.
- This is because cardinality. Turing Machine is defined as **finite** size automatons, alphabets and transitions. Thus the number of different Turing Machines are $\aleph_0$. But $\overline{\overline{\{0, 1\}^*}}=\aleph$. Since one Turing Machine could only decide one set $S$, so almost all subsets are undecidable.
- But the construction is not easy. 通过基数，我们论证了几乎所有的集合是不可判定的，但想要实际构造这样的集合实际上是很困难的。

#### $\it 2.2.2$ 图灵机的扩展 Extensions of the Turing Machine
**Marked Machines 带字符标记的图灵机**. (extended alphabet) A Turing Machine with a finite alphabet $\mathcal B$ can be **simulated** by a standard Turing Machine(whose alphabet is $\{0, 1, b\}$) on **encoded inputs**.
- **Proof**. Consider encoding characters in $\mathcal B$ to binary strings(eg. $1\to01, 0\to 00, 1'\to 11, 0'\to 10, b\to bb$). Construct a standard Turing Machine where two digits are considered as one character.
	- Under such circumstance, the input **must be encoded** into $0, 1$.
	- 之所以要定义“标准图灵机”，是因为在理论分析中要尽可能简化的模型，而在实际问题分析中我们希望尽可能有更多的工具。在扩展符号的图灵机中，输入内容必须按规则编码，但其计算能力与标准图灵机一致。

**Basic Operations and Subroutines 基本操作与子过程**. Base on an **informal** view, we can describe simple procedures of some basic operations. 
- **Scanning and Locating**. writing and erasing. copying. comparing. shifting and inserting. counting(by marks). searching and replacing. Using a stack.

#### $\it 2.2.3$ 其他扩展（待探索）Other Extensions
$2\times\infty$ 图灵机：存在等效，使得图灵机扩展为两条平行纸带、两个独立读写头。同理可以扩展为任意有限的 $n\times\infty$ 以及 $n$ 个独立读写头。

存在无限二维平面的图灵机（一个读写头，可以上下左右移动）。存在 $n$ 维图灵机。

#### $\it 2.2.4$ 命题永真问题的判定性 Validity decidablilty
First, encode all the characters. Then we can use all these symbols.

| Symbol  | Code      |
| ------- | --------- |
| $A_i$   | $0001^i0$ |
| $($     | $001$     |
| $)$     | $010$     |
| $\neg$  | $011$     |
| $\land$ | $100$     |
| $\lor$  | $101$     |
| $\to$   | $110$     |

We want to show that $\mathrm{VALID}=\{\varphi\mid\varphi\text{ is a valid \cal L-\rm sentence}\}$ is decidable.
- First, scan the characters and reject if there exist invalid characters.
- Next, use a stack to check if brackets are valid.
- Next, scan all atomic props and count the number $n$.
- Next, enumerate(binary) $0$ to $2^n-1$(by "add 1" operation), where a binary integer represent a truth assignment.
	- For every assignment, scan the string and use a stack to record the result. This is to compute T/F under given assignment.
- Finally, if any of the assignments returned false, reject. Otherwise accept.

#### $\it 2.2.5$ 图灵机自身的编码 Encoding Turing Machines
Notice that a Turing Machine can be presented by a finite alphabet:

| Object | Code       |
| ------ | ---------- |
| $q_i$  | $1^{i+1}0$ |
| $0$    | $10$       |
| $1$    | $110$      |
| $b$    | $1110$     |
| $L$    | $10$       |
| $R$    | $110$      |

Now, $Q, \mathcal A, \mathcal B, q_0, q_1, q_2$ are all encoded. To encode $\delta$, concatenate the codes for $q, c, q', c', \mathrm{dir}$ to represent $\delta(q, c)=(q', c', \mathrm{dir})$. So, now we can fully encode a Turing Machine $\mathbf T$.

Def. **Code**. Define the **Code** of a Turing Machine $\mathbf T$ as the string encoding the number of states, followed by the codes of its transitions in order. Written as $\langle\mathbf T\rangle$. 

Def. **Universal Turing Machine**. We will **explicitly** give an example of the code of the Turing Machine.
- First, let $a=\lceil\log_2(n+2)\rceil$. Unify index of nodes in the automaton and encode each state(from $q_0$ to $q_[n-1]$, along with $q_{\mathrm{accept}}$ and $q_{\mathrm{reject}}$) with its binary code(exactly $a$ bits, with zeros leading).
- Then after encoding every state, let $0=\texttt{00}, 1=\texttt{01}, b=\texttt{10}$ to encode the alphabet.
- Last, we need to encode $\delta$. Write $(q, c)\to(q', c', \texttt{L/R})$ for each transition.
- **Notice**. From there we could construct an **Universal Turing Machine**. Given an encoded Turing Machine $\langle\mathbf T\rangle$ and its originally expected input $w$ and pack them as $\langle\mathbf T, w\rangle$ as the input of the universal Turing Machine. The universal Turing Machine is expected to read every state and transition of $\mathbf T$ and simulate what $\mathbf T$ is doing on $w$. Such universal machine exists.

Def. For a Turing Machine $\mathbf T$ and its input $w$, define $\langle\mathbf T, w\rangle:=1^{|\langle\mathbf T\rangle|}0\langle\mathbf T\rangle w$ as an overall code.
- Then by such construction we can recognize both $\mathbf T$ and its input $w$.

### $\it 2.3$ 停机问题 The HALTING PROBLEM
Prob. **The Halting Problem 停机问题**. A universal Turing Machine can simulate any given machine on any given input. **The Halting Problem** asks whether there is an algorithm which always answers, if the machine halts with given $\langle\mathbf T, w\rangle$.

Thm. **Undecidability of the Halting Problem 停机问题不可判定**. Let $\mathrm{HALT}:=\{\langle\mathbf T, w\rangle\mid\mathbf T\text{ halts on input }w\}$. We want to prove: the set $\mathrm{HALT}$ is **undecidable**.
- **Analysis**. 这看起来像一些悖论或者自指问题（罗素悖论？）
- **Proof**. Suppose the existence of a Turing Machine $H$ which can solve the problem. Thus $H$ halts on every input. Specifically, if the input is $\langle\mathbf T, w\rangle$, this should be accepted if $T$ halts on $w$ or else be rejected.
- Construct a machine $D$ on input $x$ performs the following procedure:
	- Check whether $x$ is the code of a Turing Machine. If not, reject. Let $T_x$ be the described machine.
	- Form the string $1^{|x|}0xx=\langle T_x, x\rangle$ and run $H$ on it.
		- This asks whether the machine described by $x$ halts on input $x$.
	- If $H$ accepts, run forever.
	- If $H$ rejects, accept.
- Since all operations are valid, $D$ can be implented as a Turing Machine. Consider run $\langle D\rangle$ on itself $D$.
	- If $H$ accepts $\langle D, \langle D\rangle\rangle$, then $D$ would run forever, contrary to the answer given by $H$.
	- If $H$ rejects, this says that $\langle D\rangle$ runs forever on $D$, but the truth is $H$ returned rejected by the end of $D$.
- Both possibilities give a contradiction. Therefore $\mathrm{HALT}$ is **undecidable**. **No single algorithm can always give the correct answer for every machine and every input**.

**Intuition**. 应当意识到，停机问题的对角线证明，与 Russell 悖论和 Godel 句子的构造，是同一个“**自指问题**”的不同表现形式。目前暂时不需要深究其中原理。
- Russell Paradox: $A:=\{X\mid X\notin X\}$.
- Godel's sentence: $G$ says, "I am not provable".
- The Halting Problem: machine $D$ asks "if $D$ halts".
- The proof of $\overline{\overline{A}}<\overline{\overline{P(A)}}$. (Assume $f:A\leftrightarrow P(A)$ and construct $B=\{x\in A\mid x\notin P(A)\}$ (sth like this) as a contradiction).

这是同一种东西！
#### $\it 2.3.1$ Another Solution of the Halting Problem
By this section we will give another easier solution of the Halting Problem.

Def. **Universal Turing Machine 通用图灵机**. We give a strict rule to formally define the universal Turing Machine. A universal Turing Machine $\mathbf U$ is expected: (Given $\mathbf T$ and $w$)
1. If $\mathbf T$ accepts $w$, then $\mathbf U$ accepts $\langle\mathbf T, w\rangle$.
2. If $\mathbf T$ rejects $w$, then $\mathbf U$ rejects $\langle\mathbf T, w\rangle$.
3. If $\mathbf T$ runs forever on $w$, then $\mathbf U$ runs forever.
4. If the input is not a valid code for $\langle\mathbf T, w\rangle$, $\mathbf U$ rejects.

Description. **The Halting Problem 停机问题**. A universal Turing Machine can simulate any given(encoded) machine on any given input. However, we don't know if the simulation halts. The *Halting Problem* asks whether there exists an algorithm which always answers the question.

Let $[\mathbf T]$ denote the code of a Turing Machine $\mathbf T$. Define $\mathrm{HALT}^*:=\{\langle\mathbf T, [\mathbf T]\rangle\mid\mathbf T\text{ halts on input }[\mathbf T]\}$. Is $\mathrm{HALT}^*$ decidable?
- Notice: This is an easier problem, compared to the original $\mathrm{HALT}$.

Lemma. **Undecidability of the Halting Problem 停机问题不可判定**. The set $\mathrm{HALT}^*$ is undecidable.
- **Analysis**. 这又是这种自指问题。这种方法在于，假设其成立，则根据其规则构造出一种矛盾或不存在的对象，进而证伪其命题。
- **Proof**. Suppose that a machine $\mathbf H$ decides $\mathrm{HALT}^*$. Thus $\mathbf H$ halts on every input and accepts **iff** $\mathbf T$ halts on $[\mathbf T]$.
- Construct a machine $\mathbf D$ which on every input $w\in\{0, 1\}^*$, performs the following procedure:
	- Check whether $w$ is some $[\mathbf T]$. If not, halt and reject.
	- Decode $\mathbf T$ from $w$ and form $y=\langle\mathbf T, [\mathbf T]\rangle$. Run $\mathbf H$ on $y$.
	- If $\mathbf H$ accepts, run forever.
	- If $\mathbf H$ rejects, halt and accept.
- Consider given the input of its own code $[\mathbf D]$ to $\mathbf D$. The first check succeeds, and $\mathbf D$ runs $\mathbf H$ on $\langle\mathbf D, [\mathbf D]\rangle$.
	- If $\mathbf H$ accepts, then $\mathbf D$ should run forever by its construction. However, by $\mathbf H$'s definition $\mathbf D$ is supposed to halt on $[\mathbf D]$.
	- If $\mathbf H$ rejects, then $\mathbf D$ should halt and accept by its construction. However, by $\mathbf H$'s definition $\mathbf D$ is supposed to run forever.
- Both possibilityes give a contradiction. Therefore $\mathrm{HALT}^*$ is undecidable.

Thm. The set $\mathrm{HALT}$ is undecidable.
- **Proof**. If $\mathrm{HALT}$ is decidable, then $\mathrm{HALT}^*$ is decidable.
- Notice. This does not prevent us from determining whether particular machins halt. It says no single algorithm can always give the correct answer for every machine and every input.
#### $\it 2.3.2$ Analysis on self-reference problem
**Intuition**. 有关自指问题的分析。自指问题会出现的原因在于我们对逻辑有一个最根本的默认，任何一个陈述（在语义中）必须或者为真，或者为假，不存在又真又假或不真不假的情况。这会导致什么问题？
- 考虑一种形式化，每个句子的真假可以形式化为布尔变量 $x_i$，而其内容本身则提出了一个有关 $x_1, x_2, \cdots, x_n$ 的方程。
- 在正常的推理下，推理链应当是 DAG。而一旦有方程指向自身（存在环），那么推理在语义上就失效了，这个环不能提供一个“语义的起点”。此时整个推理变成了解方程问题而非确定性的逻辑推演。
	- 举一个典型例子，“这句话是假的”。其方程为 $x=\neg x$。这个方程在布尔代数中无解，因此这个自指没有合法解释（无成真赋值）。
	- 另一个例子，有 $100$ 句话，其中第 $k$ 句形如“总共有 $k$ 句话是假的”（即 $[n=k]$）。假设有 $n$ 句话是假的，那么 $n=100-\sum\limits_{i=1}^{100}[n=k]\ge 99$。而恰好 $n=99$ 满足这个方程，因此只有第 $99$ 句话是真的。
- 所以在涉及推理环以及自指问题时，我们放弃了寻找一个确定性的推理，而是转而寻找成真赋值（模型论）。

**Intuition**. 有关图灵机以及自指问题。可以将图灵机也视为一种方程组。可以将纸带上的符号以及每一步所处的状态视为一个至多可列的方程组（要求每一步的状态和符号是什么），这样按照图灵机的机械执行，理想情况下（不存在自指时）一些变量的赋值总是由另一些变量决定（这就是图灵机的执行过程）。
- 而停机问题就是说，现在这个方程出现了自指。比如 $n$ 是图灵机的总步数，那在图灵机（对应的方程组中）问“是否有 $[n<\infty]$”并尝试构造这样的机器就是一个自指问题。这个自指问题没有合法解，因此不存在能判定停机问题的通用图灵机。
### $\it 2.4$ Decidability of other problems
To prove some particular problem undecidable, we could prove an algorithm deciding it would also decide $\mathrm{HALT}$. 
- eg. Construct an instace $I_{\mathbf T, w}$ of a decision problem $P$ such that $\mathbf T(w)\text{ halts}\iff I_{\mathbf T, w}\text{ has answer yes}$. An algorithm deciding $P$ would also decide $\mathrm{HALT}$. This is called a **reduction** from the halting problem to $P$.

Def. **Conputable function 可计算函数**. A function $f:\mathbb N\to\mathbb N$ is computable if there exists a Turing Machine which on every input $1^n$, halts with the output $1^{f(n)}$.
#### $\it 2.4.1$ Analysis on the Busy Beaver function
Define Busy Beaver function: let $S(n)$ be the maximum steps of running $\mathbf T$ with blank input, where $\mathbf T$ has $n$ non-halting states(not concluding $q_{\mathrm{accept}}$ and $q_{\mathrm{reject}}$) and $\mathbf T$ is guaranteed to halt given a blank input.
- Set $S(0)=0$.

Thm. The function $S(n)$ is not computable.
- **Proof**. Suppose $S(n)$ is computable. Given any Turing Machine $\mathbf T$ and the input $w$, we could construct another Turing Machine $\mathbf T_w$ which always starts on a blank tape, writes $w$, returns its head to the initial position and then simulates $\mathbf T$ on $w$. Therefore $\mathbf T_w$ halts on a blank tape **iff** $\mathbf T(w)$ halts.
- Let $r$ be the number of non-halting states of $\mathbf T_w$(computable, from the code of $\mathbf T_w$). We know exactly $S(r)$, so if $\mathbf T_w$ halts, then it must halts within $S(r)$ steps. So construct a Turing Machine $\mathbf A$ to simulate $\mathbf T_w$ for at most $S(r)$ steps(first calculate $S(r)$ by a previous procedure). If it TLEs, rejects, otherwise accepts.
- Since $\mathbf T$ and $w$ are arbitrary, the function of $\mathbf A$ is exactly the same to determine $\mathrm{HALT}$. This is impossible. Thus the Busy Beaver funciton is not computable.

**Analysis**. 这里我们就使用了“归约”，将 Busy Beaver 问题转化为停机问题。
#### $\it 2.4.2$ Computably Enuerable
Def. **Computable Sets**. A set $A\subseteq\mathbb N$ is computabe or decidable, if there exists a Turing Machine which, on every input $1^n$, halts and accepts if $n\in A$, and halts and rejects if $n\notin A$.

Def. **Computably Enumerable**. A set $A\subseteq\mathbb N$ is computably enuerable if there exists a Turing Machine $\mathbf T$ such that for every $n\in\mathbb N$, $n\in A$ **iff** $\mathbf T(1^n)$ halts. Notice that if $n\notin A$, then $\mathbf T(1^n)$ may reject or run forever.
- $\mathrm{HALT}$ is computably eumerable. By given $\mathbf T$ and $w$, just simulate $\mathbf T$ on $w$. If $\mathbf T$ halts, then accept. Otherwise the machine could run forever, but that does not matter for computable enumerability.
#### $\it 2.4.3$ Brief on Hilbert's 10th problem
Prob. Given a poly $P(X_1, X_2, \cdots, X_m)\in\mathbb Z[X_1, X_2, \cdots, X_m]$. Determine whether there are integers $a_1, a_2, \cdots, a_m$ such that $P(a_1, a_2, \cdots, a_m)=0$.

Lemma(Davis-Putnam-Robinson-Matiyasevich). Let $A\subseteq\mathbb N$ be computably enumerable. There exists $m\ge 1$ and a poly $P(t, Y_1, \cdots, Y_m)\in\mathbb Z[t, Y_1, \cdots, Y_m]$ such that for every $n\in\mathbb N$, $n\in A$ **iff** $\exists y_1\cdots y_m\in\mathbb Z$ such that $P(n, y_1, \cdots, y_m)=0$.
- 也就是任意一个可计算枚举的 $A\subseteq\mathbb N$ 都可以被表示为某个整系数多项式的整数解集。

Thm. Hilbert's 10th problem is undecidable.
- **Proof**. Suppose there exists an algo $\mathbf A$ to determine whether the poly has integer solution. Let $K\subseteq\mathbb N$ be the code set of $\mathrm{HALT}$. We know that $K$ is CE(computably enumerable) but not decidable.
- By DPRM thm, $n\in K$ **iff** $\exists\mathbf y\in\mathbb Z^m$ and a poly $P$ where $P(n, \mathbf y)=0$. ($P$ and $m$ are fixed for a fixed $K$)
- Then construct an algo to decide $K$:
	- Input $n$. Let $P_n(\mathbf y)=P(n, \mathbf y)$.
	- Use $\mathbf A$ to determine whether $P_n$ has an integer solution. Then, $n\in K$ **iff** $P_n$ has an integer solution $\mathbf y$. Then, the algo decides $K$, so it decides $\mathrm{HALT}$.
- Thus no such algo exists.
### $\it 2.5$ Turing Oracles
Def. **Oracle Turing Machine**. Let $A\subseteq\mathbb N$ and let $\chi_A(n)=\begin{cases} 1\quad n\in A\\0\quad n\notin A \end{cases}$. Write $\chi_A(n)$ on a tape indexed by $\mathbb N$ and call it the **oracle tape(read-only)**. Another tape is the work tape(of the normal Turing Machine, initially written sth). An Oracle Turing Machine relative to $A$ has the two tapes, indexed by $\mathbb Z$, with alphabet $\Gamma=\{0, 1, b\}$.
- Formally its finite program is a tuple $\mathbf T=(Q, \Gamma, \delta, q_0, q_1, q_2)$ where $Q$ is finite. $\delta:(Q-\{q_1, q_2\})\times\{0, 1\}\times\Gamma\to Q\times\Gamma\times\{\texttt L, \texttt R\}^2$.
- The Oracle Turing Machine has 2 headers. The oracle header moves on the oracle tape(indexed by $\mathbb N$) and the normal header moves on the work tape. By giving the current state, the information of oracle header and the character of normal header, $\delta$ decides the new state, the character to write(on the work tape) and the direction for both headers respectively.
	- Notice: Though we require the oracle header to move step by step, it is equal to directly checking any information on the oracle tape directly(anytime anywhere).
- Note: write $\mathbf T^A(w)$ as the result of the oracle Turing Machine given $A$.

**Analysis**. The Oracle Turing Machine plays a role of a computer given an "answer" from set $A$. What's new is that the oracle Turing Machine has a reference from $A$, where $A$ can be **any** set.
- eg. Given the oracle of $\mathrm{HALT}$, one oracle Turing Machine can decide much more problems than common Turing Machine.

Def. **Turing Reduction**. Call $A\le_TB$($A$ is **Turing reducible** to $B$) if there exists an oracle Turing Machine $\mathbf T$ given $B$ to decide $A$.
- **Turing Equivlance**. If $A\le_TB$ and $B\le_TA$, then we say $A$ and $B$ are Turing equivlant($A\equiv_TB$). Such equivlance classes are called **Turing Degree**.
- This could tell "Which problem is harder".
### $\it 2.6$ Tiny Summary
从目前有关图灵机的学习看出，图灵机从“算法的数学形式化”出发，规定了算法的上限。但其本身却是一个简单的模型：本质上只是有限个 if-then 的选择分句。这与逻辑系统的语法推演是一致的：都是“如果是这样，那么根据某种机械规则，可以得到那样”。所以图灵机以及现在所有的算法某种程度上是与逻辑系统同构的东西，因此其也必定面临一个相同的问题：**自指**。

图灵机 $\mathbf T$ 可以被编码为 $\langle\mathbf T\rangle$ 而保留其所有信息，因此当图灵机的输入可以为图灵机，其必然指向自指问题，这跟逻辑环的“布尔方程无解”是一样的。**图灵机的运行，就是逻辑推演在时间维度上的展开。自指，是任何足够强的形式系统（无论它是静态的证明系统，还是动态的计算系统）无法逃避的宿命。停机问题、哥德尔定理、罗素悖论，本质上都是同一个自指方程在不同数学宇宙中的投影**。
