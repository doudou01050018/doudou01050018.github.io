---
publish: true
---

# LM-4 FOL(2)
### $\it 0.1$ Intro
In note FOL(2) we have the precise definition of FOL formal proofs.
## $\mathrm I$ FOL proof system Intro
### $\it 1.1$ Church's undecidability Thm.
Review then definition of proof in prop logic. $\Sigma\vdash\varphi$ if there exists a sequence of prop logic sentence $\theta_1, \theta_2, \cdots, \theta_n$ where $\theta=\varphi$. For $1\le i\le n$, either one of the following holds:
- $\theta_i\in\Sigma$ (in the assuption set).
- $\theta_i$ is **VALID**.
- (MP)There exists $1\le j, k<i$ s.t. $\theta_j=\psi$ and $\theta_k=\psi\to\theta_i$.

In this definition, we used a **SIMPLIFIED** trick(defining by semantics) since, it is simple to know if a prop logic sentence $\varphi$ is valid(under any prop logic assignment, $\varphi$ is true) and there exists such algorithms to do the work. But in FOL:

Def. **Validity**. An FOL sentence $\varphi$ is valid, **iff**: 
- For any model $M$(and any assignment $s$, although $s$ is not important in a sentence), $M\models\varphi[s]$.

Thm. **Church's undecidability Thm**. FOL validity is **UNDECIDABLE**: There exists no algorithm which, given an any FOL sentence, always halts and correctly decides whether it is valid.
- **Proof**(Simplified Version). We can write an FOL sentence $\theta_{\mathbf T, w}$ denoting that $\models\theta_{\mathbf T, w}\iff T(w)\text{ halts}$. Then a machine solving validity problem would decide $\mathrm{HALT}$.
### $\it 1.2$ Axiomization of proof
To handle with such problem, an effective way is to replace "validity" with several logical AXIOMS. To begin with, we should define **substitution**.

Def. **Substitution**. Let $\varphi$ be a formula, $x$ a variable, and $t$ a term. We write $\varphi[t / x]$ for the result of replacing every **free occurrence** of $x$ in $\varphi$ by $t$. The term $t$ is **free for $x$ in $\varphi$** if this replacement does not cause any variable in $t$ to become bound.
- Equivalent: Replacing every free $x$ in $\varphi$ does not cause any variable in $t$ being **captured** by the quantifiers in $\varphi$. That is: for every variable in $t$, no free occurrence of $x$ in $\varphi$ is within the range of a quantifier $\forall y$ or $\exists y$.
- Eg. $\varphi=\exists yR(x, y)$. Then replacing $z\gets x$ happens nothing. But $y\gets x$ is not a valid substitution since $y$ is restricted by $\exists y$.

Def. **LOGICAL AXIOMS**. A logical axiom is a formula our proof system allows us to write without deriving if from earlier lines or taking it from $\Sigma$. It is a set of general rules of inference.
- **Propositional Axioms**. Every uniform substitution instance from a valid prop sentence.
	- Eg. $A\to(B\to A)$ is always valid, so $\forall xP(x)\to(Q(c)\to\forall xP(x))$ is valid.
	- This tell us that, if a formula has a propositional-like skelenton and the prop sentence is valid, then the formula is always valid.
- **Universal Instantiation**. $\forall x\varphi\to\varphi[t / x]$, where $t$ is free for $x$ in $\varphi$.
	- Eg. $\forall xP(x)\to P(c)$. Here constant $c$ guarantees not to be captured in $\varphi$.
- **Distribution**. $\forall x(\varphi\to\psi)\to(\forall x\varphi\to\forall x\psi)$.
- **Vacuous Quantification**. $\varphi\to\forall x\varphi$ for $x\notin F(\varphi)$.
	- Notice: $x$ is **NOT** in the free variable set of $\varphi$.
- **Existential Duality**. $\exists x\varphi\to\neg\forall x\neg\varphi$ and $\neg\forall x\neg\varphi\to\exists x\varphi$.
- **Equality Axioms**. $t=t$ and $t=u\to(\varphi[t / x]\to\varphi[u / x])$ where $t, u$ are free for $x$ in $\varphi$.

Def. **Inference Rules and Formal Proofs**. We use two rules:
1. $\dfrac{\varphi\quad\varphi\to\psi}{\psi}$(MP).
2. $\dfrac{\varphi}{\forall x\varphi}$(Generalization, $x$ not occur freely in any undischarged assumption).

Def. **FIRST ORDER PROOF**. Let $\Sigma$ be a set of $\mathcal L$-sentences and $\varphi$ an $\mathcal L$-formula. A **proof** of $\varphi$ from $\sigma$ is a **FINITE** sequence of formulas $\theta_1, \theta_2, \cdots, \theta_n$ with $\theta_n=\varphi$, such that for each $m\le n$, at least one of the following holes:
1. $\theta_m\in\Sigma$.
2. $\theta_m$ is a logical axiom from one of the 6 families specified above.
3. There are $k, l<m$ s.t. $\theta_k=(\theta_l\to\theta_m)$. (MP rule)
4. There are $k<m$ and a variable $x$ s.t. $\theta_m=\forall x\theta_k$. (Generalization)
- Write $\Sigma\vdash\varphi$ if such a proof exits, and $\vdash\varphi$ if $\Sigma=\varnothing$.
### $\it 1.3$ Analysis on Substitution and Generalization
In the rule of substitution, we require that $t$ should not affect the variables, **esipecially** those restricted by the quantifiers.

What does free variables mean in proofs? 
- Suppose there is $P(x)$ in a proof. Here $x$ denote a variable symbol, and we can regard this as a "particular" object(if applying a particular model). 
- Or: Since we don't know what model we are going to apply later, so we shoule be conservative when dealing with free variables since it could be anything in some model(if referring it to some model later). So to make sure that the syntax proof is 100% correct, we should not do anything further on free variables

**Free variables in proofs.** A free variable has no fixed semantic value in the syntactic proof. Under a later structure $M$ and assignment $s$, it may denote any element $s(x)\in A$. Therefore any syntactic manipulation involving a free variable must be valid uniformly for all possible assignments. The side conditions on substitution and generalization guarantee this uniformity.

Note. The correctness of such proof system is **NOT** obvious compared to the prop logic one. The work to prove it valid belongs to the soundness thm and the completeness thm.
## $\mathrm{II}$ Soundness
Here comes the interesting part.
### $\it 2.1$ Soundness
Def. For a FOL theory $\Sigma$ and a formula $\varphi$, write $\Sigma\models\varphi$ if, for every structure $M\models\Sigma$ and every assignment $s$, we have $M\models\varphi[s]$.
- Also write $\models\varphi$ when $\varnothing\models\varphi$.

Thm. **Soundness Thm**. $\Sigma\vdash\varphi\implies\Sigma\models\varphi$.
- **Proof**. Suppose there exist a proof sequence $\theta_1, \theta_2, \cdots, \theta_n$ ends with $\theta_n=\varphi$. Fix any structure $M$ that supports $M\models\Sigma$, we want to show that for any assignment $s$, there is $M\models\theta_m[s]$(for any $1\le m\le n$). Each $\theta_m$ must comes out of one of the four rules.
- Case 1. $\theta_m\in\Sigma$. Since $M\models\Sigma$, we have $M\models\theta_m$. Since $\theta_m$ is a sentence, the truth does not depend on the assignment $s$. So $M\models\theta_m[s]$ for any $s$.
- Case 2. $\theta_m$ comes from one of the 6 axioms. We should check every rule is correct **semantically**.
- Case 3. $\theta_m$ comes from MP rule. Suppose $\theta_l$ and $\theta_k=\theta_l\to\theta_m$ already exists($k, l<m$). Then by induction, for every assignment $s$ we have $M\models\theta_l[s]$ and $M\models(\theta_l\to\theta_m)[s]$. By semantics, when under such $s$, $M\models\theta_m[s]$ can be inducted. Then this holds for every $s$.
- Case 4. $\theta_m$ comes from **generalization**. Suppose $\theta_m=\forall x\theta_k$($k<m$). By induction, for any assignment $r$ there is such $M\models\theta_k[r]$.
	- From semantics, $M\models\forall x\theta_k[s]$ requires that for any $d\in A$, $M\models\theta_k[s(x\mid d)]$ holds. By induction, any $r$ would support $\theta_k$, so for every $s$ there is $M\models\forall x\theta_k[s]$.
- Therefore every proof line $\theta_m$ is true under any assignment. In particular, $M\models\theta_n[s]$ for any $s$. Since $M$ is arbitrary, $\Sigma\models\varphi$.

**Intuition**. In FOL, syntax is much more **Conservative** than semantics, though in prop logic, they are almost parallel.
- In prop logic, the semantics are **boolean**: one atom prop should only be redirected to a True or False boolean value. But in FOL, what syntax $x$(and $f, R, c$) represents strongly relies on the model $M$. Without model you can do (almost) nothing further given only abstract symbols, since different models $M_1, M_2$ should explain the symbols very differently.
- Therefore syntax has to be model-blind. The rule to conduct formal proof requires must adapt **ANY** reasonable model $M$. So this is a **Conservative** choice that the formal proof system only execute proof program with 100% safety.
### $\it 2.2$ Consistency
Def. A FOL theory $\Sigma$ is **consistent** it there is no sentence $\chi$ such that $\Sigma\vdash\chi\land\Sigma\vdash\neg\chi$. Otherwise it is **inconsistent**.
- Equivalently, $\Sigma$ is inconsistent if it proves every sentence.
	- Apply MP. $\chi\to(\neg\chi\to\varphi)$ is a tautology. Therefore $(\chi\land\neg\chi)\to\varphi$ is valid for any $\varphi$.

Thm. (Model Existence). For every FOL theory $\Sigma$, $\Sigma$ has a model(existing a model $M$ such that $M\models\Sigma$) **iff** $\Sigma$ is consistent.
- Soundness thm provides proof for Nessesity, and the proof of Sufficiency depends on the **Completeness Thm**.
## $\mathrm{III}$ Completeness
Now we discuss theories and models under a fixed first-order language $\mathcal L$.
### $\it 3.1$ Pre-works
Def. **Maximally Consistent**. A theory $\Sigma$ is maximally consistent if $\Sigma$ itself is consistent and every $\mathcal L$-theory $\Sigma'\supsetneq\Sigma$ is inconsistent.

Lemma. Suppose $\Gamma\vdash\varphi$ and let $c$ be a constant symbol which occurs in no sentence of $\Gamma$. Then there is a variable $y$ s.t. $\Gamma\vdash\forall y\varphi[c\mapsto y]$.
- Here $\varphi[c\mapsto y]$ is obtained by replacing every occurence of $c$ by $y$.
- **Proof**. Let $\theta_1, \theta_2, \cdots, \theta_n$ be a proof of $\varphi$ from $\Gamma$. Choose a variable $y$ which occurs in **none** of these formulas and replace every occurence of $c$ in the proof by $y$. (Define $\theta_i':=\theta_i[c\mapsto y]$ and prove that this is another valid proof by induction)
	- Under such circumstance, the assumptions from $\Gamma$ remain unchanged.
	- Since $y$ is fresh, the replacement preserves logical axiom instances and applications of the inference rules. We therefore obtain a proof of $\varphi[c\mapsto y]$ from $\Gamma$ in which $c$ does not occur.
- Since $\Gamma$ consists of sentences, we may apply generalization to obtain $\forall y\varphi[c\mapsto y]$.

Thm. **Deduction Thm**. Let $\Gamma$ be a theory, $\psi$ be a sentence and $\varphi$ be a formula. If $\Gamma\cup\{\psi\}\vdash\varphi$, then $\Gamma\vdash(\psi\to\varphi)$.
- **Proof**. Let $\theta_1, \theta_2, \cdots, \theta_n$ be a proof of $\Gamma\cup\{\psi\}\vdash\varphi$. Define $\theta_i':=\psi\to\theta_i$ and we want to show that this new sequence is valid under $\Gamma$. Apply induction.
	- One difficult is that we want to show $\forall x(\psi\to\theta_m)$ is valid when $\theta_m=\forall x\theta_k$ occurs. The key is to apply axioms:
		- $\psi\to\forall x\psi$. $\forall x(\psi\to\theta_k)\to(\forall x\psi\to\forall x\theta_k)$. Then by induction there is $\psi\to\theta_k$ valid. Therefore $\psi\to\forall x\theta_k$.

Lemma. If $\Gamma$ is consistent and $\Gamma\cup\{\psi\}$ is inconsistent, where $\psi$ is a sentence. then $\Gamma\vdash\neg\psi$.
- **Proof**. Since $\Gamma\cup\{\psi\}$ is inconsistent, then there is a sentence $\chi$ such that $\Gamma\cup\{\psi\}\vdash\chi\land\neg\chi$. By deduction $\Gamma\vdash\psi\to(\chi\land\neg\chi)$.
- Since $(\psi\to(\chi\land\neg\chi))\to\neg\psi$ is an instance of prop tautology, MP therefore gives $\Gamma\vdash\neg\psi$.

Thm. **Lindenbaum's Thm**. Every consistent $\mathcal L$-theory $\Sigma$ is contained in a maximally consistent $\mathcal L$-theory $\Gamma$.
- **Proof**. Let $\mathcal P:=\{\Delta\mid \Delta\text{ is a consistent }\mathcal L\text{-theory and  }\Sigma\subseteq\Delta\}$. This is a partially-ordered set by $\subseteq$. The set is non-empty since $\Sigma\in\mathcal P$.
- Let $\mathcal C$ be a non-empty chain in $\mathcal P$. Define $D:=\bigcup\limits\mathcal C$. $D$ is consistent.
	- Otherwise, $D\vdash\chi\land\neg\chi$. This proof is finite, so it only uses finitely many assumptions from $D$. Forming these sentences would get a theory $X\in\mathcal C$(Since $\mathcal C$ is a chain) containing all of them(every sentence belongs to some element in $\mathcal C$, and let $X$ be their 'boss' since there is total order), saying that $X$ is inconsistent. This is a contradiction.
- Therefore $\bigcup\limits\mathcal C$ is an upper-bound of the chain $\mathcal C$. By *Zorn's Lemma*, $\mathcal P$ has a maximal element $\Gamma$. Therefore these is no such consistent $\Gamma'\supsetneq\Gamma$.
### $\it 3.2$ Henkin Witness Construction
**Intuition**. The complenteness thm wants to show that if a theory $\Sigma$ is consistent, then there exists a structure $M\models\Sigma$. The Henkin Witness Construction aims at explicitly writing out what the variables are really saying and then apply a model to interpret the constant symbols.

Def. **Witness**. Let $\Sigma$ be an $\mathcal L$-theory, and $C$ be a set of constant symbols in $\mathcal L$. We say that $C$ is a set of witnesses for $\Sigma$ if, for every formula $\varphi$ with at most one free variable $x$, there is some $c\in C$ such that $\Sigma\vdash\exists x\varphi\to\varphi[c / x]$.
- If $\varphi$ is a sentence, we may choose any variable $x$.

Lemma. Let $\Sigma$ be an $\mathcal L$-theory and $\varphi$ be an $\mathcal L$-formula with at most one free variable $x$. Let $c$ be a new constant symbol(neither in $\Sigma$ nor $\varphi(x)$). If $\Sigma\vdash\varphi[c / x]$, then $\Sigma\vdash\forall x\varphi$.
- **Proof**. Take a proof of $\varphi[c / x]$ from $\Sigma$, and choose a variable $y$ which does not occur in the proof or in $\varphi[c / x]$. Replace evey occurence of $c$ in the proof by $y$.
- Since $c$ does not occur in $\Sigma$, the assumptions are unchanged. Since $y$ is fresh, the replacement preserves the logical axiom instances and inference steps(by induction). We obtained a proof of $\varphi[y / x]$ from $\Sigma$.
- Generalize, and $\forall y\varphi[y / x]$. Renaming the variable gives $\forall x\varphi$.

Lemma. **Adding-one-witness**. Let $\Sigma$ be a consistent $\mathcal L$-theory. Let $\varphi$ have at most one free variable $x$. Let $c$ be a new constant symbol. Then $\Sigma\cup\{\exists x\varphi\to\varphi[c / x]\}$ is consistent.
- **Proof**. Suppose not, then by deduction $\Sigma\vdash\neg(\exists x\varphi\to\varphi[c / x])$, which is $\Sigma\vdash\exists x\varphi$ and $\Sigma\vdash\neg\varphi[c / x]$. Since $c$ occurs in neither $\Sigma$ nor $\varphi$, the fresh constants lemma gives $\Sigma\vdash\forall x\neg\varphi$. By quantifier duality, this is exactly $\Sigma\vdash\neg\exists x\varphi$. This contradicts with the consistency of $\Sigma$.

Lemma. Let $\Sigma$ be a consistent $\mathcal L$-theory. Set $\kappa:=\max(|\mathcal L|, \aleph_0)$, and let $C$ be a set of $\kappa$ new constant symbols. In the language $\overline{\mathcal L}:=\mathcal L\cup C$, there is a theory $\overline\Sigma$ such that:
1. $\Sigma\subseteq\overline\Sigma$.
2. $\overline\Sigma$ is consistent.
3. $C$ is a set of witness for $\overline\Sigma$.
- **Proof**. First consider the countable case. Write $C=\{k_0, k_1, \cdots\}$. The language $\overline{\mathcal L}$ is countable, so its formulas with at most one free variable can be enmerated as $\varphi_0, \varphi_1, \cdots$.
- For each $n$, let $x_n$ be the free variable in $\varphi_n$. Let $x_n=v_0$ if $\varphi_n$ is a sentence. We construct an increasing sequence of consistent theories. Let $\Sigma_0=\Sigma$. Having constructed $\Sigma_n$, choose $d_n\in C$ which occurs in neither $\Sigma_n$ nor $\varphi_n$, and set $\Sigma_{n+1}:=\Sigma_n\cup\{\exists x_n\varphi_n\to\varphi_n[d_n / x_n]\}$.
	- Such a constant $d_n$ exists: the cardinality is countable.
	- Applying the adding-one-witness lemma, this shows that every step preserves consistency.
- Let $\overline\Sigma:=\bigcup\limits_{i=0}^\infty\Sigma_i$.
	- First $\Sigma\subseteq\overline{\Sigma}$. Then, each formula $\varphi_n$ has a witness in $C$.
	- Finally, $\overline\Sigma$ is consistent: Suppose not, then a proof of contradiction would use only finite assumptions, all belonging to some $\Sigma_n$. This would contradict the consistency of $\Sigma_n$.
- If the language is uncountable, change the set of index from $\mathbb N$ to other larger set with cardinality $\alpha$ and use transfinite recursion, taking unions at limit stages. This would use no "more" than $\kappa$ constant symbols during the construction.

Def. **Equivalence Class**. An equivalence class on a set $A$ is a relation $R\subseteq A^2$:
1. $xRx$ for every $x\in A$.
2. $xRy\to yRx$.
3. $xRy\land yRz\to xRz$.
- The equivalence class for $x\in A$ is $[x]:=\{y\in A\mid xRy\}$.
- **Quotient Set**: $A / R:=\{[x]\mid x\in A\}$.
### $\it 3.3$ Build A Model
Lemma. Let $\Sigma$ be a consistent $\mathcal L$-theory with a set of witness $C$. Then $\Sigma$ has a model.
- **Proof**. By *Lindenbaum's Lemma*, extent $\Sigma$ to a maximally consistent $\mathcal L$-theory $\Gamma$. We use propreties of maximal consistency: $\Gamma$ contains every sentence it proves, contains exactly the deduction thm and classical prop reasoning. Let $C$ remains a set of witnesses for $\Gamma$.
- Define a relation on $C$ by $c\sim d\iff(c=d)\in\Gamma$. This is an equivalence relation, and let $A:=C / \sim$. The set $A$ is non-empty since $x=x$ provides a constant in $C$. We now construct an $\mathcal L$-structure with universe $A$.
	- Well defining. (Equivalence classes)
	- Induction on terms. (Every term interprets exact one constant)
	- Truth lemma. (Structural Induction)

Thm. **COMPLETENESS THM**. Every consistent first order theory has a model.
- **Proof**. Let $\Sigma$ be a consistent $\mathcal L$-theory. By witness extension lemma, there is a consistent theory $\overline{\Sigma}\supseteq\Sigma$ in an expanded language $\overline{\mathcal L}$ with a set of witnesses.
- By the preceding lemma, there is an $\overline{\mathcal L}$-structure $M\models\overline\Sigma$. Forgetting the added constant symbols gives an $\mathcal L$-structure satisfying $\Sigma$.
### $\it 3.4$ Summary
What is the idea? When doing such construction on an initially arbitrary theory $\Sigma$, there are 2 problems:
1. The theory could be ambiguous, it could prove neither $\varphi$ nor $\neg\varphi$.
2. The symbols are abstract, $\forall x\varphi$ and $\exists x\varphi$ could not tell which $x$ it is since they are only symbols.

For the first problem, we introduce such **Maximally Consistent Theory** $\Gamma$. It must prove either $\varphi$ or $\neg\varphi$ for **any** $\mathcal L$-sentence(every sentence is judged). It provides way more information than $\Sigma$ and keeps its consistency. Therefore, if a sentence is like $\varphi\lor\chi$, then $\Gamma$ can tell which exact sentence is true.

Yet another problem still exists. Simply applying $\Gamma$ could only deal with prop logic situations. For functions $f$, relations $R$, constants $c$ and the quantifiers, we cannot tell which element(in the real universe) is actually in the reference. We introduce at least $\aleph_0$ new constant symbols and added witness axioms as $\exists x\varphi(x)\to\varphi(c)$.
- This is because `\forall` could be transferred into every constant, but `\exists` should point out exactly one. We name this element as constant $c$ as an **actor** before introducing the model.
- By $\Gamma$, every sentence like $f(c_1, c_2, \cdots, c_n)=c_k$ and $R(c_1, c_2, \cdots, c_n)$ should be judged. When interpreting constant symbols to real universes, we just copy such relationship.

Under such inference rule, there may possibly occur $c_1=c_2$ in $\Gamma$ and this is saying "two actors in the same identity in the process of derivation". We therefore need `=` equivalent classes to distinguish them.

Our final step is to prove that under such construction, $\Sigma$ is valid.
## $\mathrm{IV}$ Compactness
### $\it 4.1$ Definition
Def. **Finitely Satisfiable**. A theory $\Sigma$ is satiafiable **iff** there exists a model $M$ such that $M\models\Sigma$. A theory $\Sigma$ is finitely satisfiable if every **FINITE** subset $\Sigma_0\subseteq\Sigma$ is satisfiable.

Let's introduce the Compactness Thm again:

Def. **Compactness Thm**. A theory $\Sigma$ is satisfiable **iff**, every finite subset $\Sigma_0\subseteq\Sigma$ is satisfiable.
- Very important notice: This **DOES NOT** require that different finite subsets share a same model. They can be totally different.
- **Negation**. A theory $\Sigma$ is not satisfiable **iff**, there exists some finite subset $\Sigma_0\subseteq\Sigma$ which is not satisfiable.
- **Proof**. Suppose $\Sigma$ is not satisfiable, then by completeness it is inconsistent, which proves $\chi\land\neg\chi$ for some $\chi$. This proof uses finitely many assumptions from $\Sigma$ and such sentences form a finite $\Sigma_0\subseteq\Sigma$. Hence $\Sigma_0\vdash\chi\land\neg\chi$ saying it is inconsistent. By reversing the proposition we proved Compactness thm.
### $\it 4.2$ Important Applications
#### $\it 4.2.0$ Main Idea
In FOL, sentences and proofs must be **FINITE**. Therefore its expressions are restricted in face of **INFINITE** objects. Adding infinte constant symbols **explicitly** writes out the difference between finite and infinite(by compactness).
#### $\it 4.2.1$ Well Order
Def. **Well Order**. A **totally** ordered set $(A, \le)$ is well-ordered if, for every non-empty subset(chain)$B\subseteq A$, there exists a minimal element in $B$.
- Eg. $(\mathbb N, \le)$ is well-ordered, but $\mathbb Q$ or $\mathbb R$ is not, as $\inf\left\{ 1, \dfrac12, \dfrac13, \cdots \right\}=0$ is not in the set.

Then we have such proposition:

Prop. In language $\mathcal L=\{\le\}$, the class of well-ordered linear orders is not elementary. Other words, you cannot define all well-totally-ordered structures by simply using $\le$.
- **Proof**. Suppose that an $\mathcal L$-theory $\Sigma$ precisely describes all well-totally-ordered linear ordres. Add new constant symbols $c_0, c_1, \cdots$ to form $\mathcal L'=\mathcal L\cup\{c_i\mid i\in\mathbb N\}$. Consider the $\mathcal L'$-theory $\Sigma'=\Sigma\cup\{c_{i+1}<c_i\mid i\in\mathbb N\}$(Here $x<y$ denote $x\le y\land x\neq y$).
- We want to show that $\Sigma'$ is finitely satisfiable. Let $\Delta\subseteq\Sigma'$ be finite. Choose $N\in\mathbb N$ to be the largest index of $c_i$. Expand $(\mathbb N; \le)$ by interpreting $c_i$ as $N-i$ for $i\le N$, and as $0$ for $i>N$. Let this new model be $M_1$. Consider each sentence $\varphi\in\Delta$: if $\varphi\in\Sigma$, then $M_1\models\varphi$ since $M_1$ is a well-total-order. Then, if $\varphi=(c_{i+1}<c_i)$, it is also satisfied. Thus $M_1\models\Delta$.
- By compactness, there is an $\mathcal L'$-structure $M'=(A; \le; c_0^{M'}, c_1^{M'}, \cdots)$ s.t. $M'\models\Sigma'$. Since $\Sigma\subseteq\Sigma'$, there exists a $\mathcal L$-structure $M_*=(A;\le)$ s.t. $M_*\models\Sigma$(by forgetting all the new constants), which must be well-ordered.
- However, the non-empty subset $\{c_i^{M'}\mid i\in\mathbb N\}\subseteq A$ has no minimal element since $c_{i+1}^{M'}<c_i^{M'}$(this comes from $M'\models\Sigma'$). This is a contradiction.
#### $\it 4.2.2$ Finity
Prop. In Language $\mathcal L=\varnothing$, the class of finite structures is not elementary.
- **Proof**. Suppose that an $\mathcal L$-theory $\Sigma$ tells if structure $M$ is finite. Consider adding new constant symbols $c_0, c_1, \cdots$ to construct an $\mathcal L'=\{c_0, c_1, \cdots\}$. Let $\Sigma'=\Sigma\cup\{c_i\neq c_j\mid i, j\in\mathbb N, i\neq j\}$ be an $\mathcal L'$-theory.
- Let $\Delta\subseteq\Sigma'$ and $N\in\mathbb N$ be the maximal index of $c_i$. Find a finite set $A$ s.t. $|A|\ge N$ and $c_i\to a_i$ distinctly. Construct such $M=(A; c_0^M, c_1^M, \cdots, c_N^M)$. Since $M$ is finite, $M\models\Sigma$ and $\varphi\in\Sigma\implies M\models\varphi$. Therefore every finite $\Delta\subseteq\Sigma'$ is satisfiable.
- By compactness, there is an $\mathcal L'$-structure $M=(A; c_0^M, c_1^M, \cdots)$ s.t. $M\models\Sigma'$. Let $M_*=(A)$ be an $\mathcal L$-structure and automatically we have $M_*\models\Sigma$(Since $\Sigma$ has no $c_i$ symbols), thus $M_*$ is finite. However, $c_0^M\in A, c_1^M\in A, \cdots$ suggests that $A$ is an infinite set.
#### $\it 4.2.3$ Connected Undirected Graphs
Prop. In Language $\mathcal L=\{R\}$ where $R$ is a binary symbol(eg. $xRy$) for adjacency, the class of connected undirected graphs is not elementary.
- **Proof**. Suppose that an $\mathcal L$-theory $\Sigma$ tells whether a graph $M=(G; R)$ is undirectly-connected. For each $n\in\mathbb N$, define a formula $\delta_n(x, y)$ saying that there is a path of length at most $n$ from $x$ to $y$. Define recursively:
	- $\delta_0(x, y):=(x=y)$.
	- $\delta_{n+1}(x, y):=\delta_n(x, y)\lor\exists z(\delta_n(x, z)\land zRy)$.
- Add two constant symbols $c, d$ into the language(to form $\mathcal L'=\{R; c, d\}$). Consider such $\mathcal L'$-theory $\Sigma'=\Sigma\cup\{\neg\delta_n(c, d)\mid n\in\mathbb N\}$. The additional sentences say that there are some node $c, d$ disconnected.
- Next, consider some finite subset $\Delta\subseteq\Sigma'$. We need to construct a structure. Let $N$ a large-enough number(at most $N$ variables occured in $\Delta$ and every $\neg\delta_X(c, d)$ has $X\le N$). Construct a graph $0\longleftrightarrow1\longleftrightarrow2\longleftrightarrow\cdots\longleftrightarrow N+1$ and interpret $c\to 0$ and $d\to N+1$. The model with this graph $M=(G; R; c, d)$ satisfies $\Delta$($M$ is connected, and the distance between $c$ and $d$ is far enough).
- By compactness, there is a model $M=(G; R; c, d)$ s.t. $M\models\Sigma'$. Since $\Sigma\subseteq\Sigma'$, $M\models\Sigma$ and we can switch the language to $\mathcal L$ since $\Sigma$ has no symbol $c, d$.
- Therefore, $M=(G; R)$ is an undirected connected graph. But consider $M\models\Sigma'$ under $\mathcal L'$, $c^M$ and $d^M$ are disconnected in $G$. This is a contradiction.
