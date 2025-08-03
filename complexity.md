## Basics
>finite automaton is $(Q,\Sigma,\delta,q_0,F)$: states, alphabet, transition function, start state, accept states
 nondeterministic finite automaton additionally satisfies $\delta: Q\times \Sigma_{\epsilon}\rightarrow P(Q)$
 generalized nondeterministic finite automata has one start and end state, arrows between every state, and regex's as arrows:
 $(Q,\Sigma,\delta,q_{\text{start}},q_{\text{accept}})$ where $\delta: (Q-\{q_{\text{accept}}):$ ...
 pumping lemma - if A is regular language, there is a number $p$ the pumping length such that any string $s\in A$ with $|s|\ge p$ can be written as $s=xyz$ where $xy^iz \in A$ , $|y|>0$ and $|xy|\le p$

>context-free grammar $(V,\Sigma,R,S)$ variables, terminals, rules, start. can be written in:
>chomsky normal form $A\rightarrow BC$ where $B,C$ can't equal start variable, and $A\rightarrow a$ , and $S\rightarrow \epsilon$ is allowed
>nondeterministic pushdown automaton has input alphabet and stack alphabet $\Sigma$ and $\Gamma$ , and transition function $Q\times \Sigma_{\epsilon} \times \Gamma_{\epsilon}\rightarrow P(Q\times \Gamma_{\epsilon})$ and is equivalent to a nondeterministic CFG

>deterministic PDA $\delta: Q\times \Sigma_{\epsilon} \times \Gamma_{\epsilon} \rightarrow (Q\times \Gamma_{\epsilon}) \cup \{ \emptyset\}$ ; also, exactly one of $\delta(q, a, x), \delta(q, a, \epsilon), \delta(q, \epsilon, x), \delta(q, \epsilon, \epsilon)$ is not null
  CFGs aren't closed under complementation, but DPDAs are
>DCFG has every valid string a forced handle. $v=xhy$ , where $h$ is the unique handle in every valid string that starts with $xh$. a handle is the RHS of some substitution rule, and the latest substitution in some rightmost derivation
>DK-test constructs a DFA DK associated with a CFG $G$ that accepts $z$ iff $z$ is the prefix of a valid string, and $z$ ends in a handle. $G$ is DCFG iff DK's accepted states have exactly one completed rule and no dotted rules of the form $B\rightarrow u.av$ where $a\in\Sigma$

>Conversion of DCFG to DPDA works by using DK and using the stack to store DK's state, so every time we reach the handle $B \rightarrow u$ we can pop $|u|$ things off the stack and then process $B$
>Conversion of DPDA to DCFG and PDA to CFG is similar **TODO**

>$LR(k)$ is a CFG except for each valid string $v=xhy$, $h$ is the unique handle in every valid string $xhy_{:k}y'$ 
>We do so by introducing $k$ lookahead symbols and redefining the $DK-k$ test to have states that are dotted rules along with all possible combinations of lookahead symbols $a_1\dots a_k \in \Sigma^k$ . Start state has epsilon move $S_1\rightarrow .u\quad a$ , shift transitions take state $T\rightarrow u.xv\quad a$ to  $T\rightarrow ux.v\quad a$ where $x$ can be either variable or terminal and consumes $x$, and epsilon transitions take $T\rightarrow u.Cv\quad a$ to $C\rightarrow .C\quad b$ where $b$ is the $k$ leftmost characters of $va$ . Equivalence of two rules is if both are completed and $a_1=a_2$ or one of them isn't completed and $a_1$ follows its dot.
>$G$ is $LR(k)$ iff every accept state doesn't have two consistent dotted rules; this means when you parse it with a bottom-up shift reduce parser of lookahead $k$ you don't have any ambiguities

## Turing Machine

>Turing machine $M$ . Multitape can be simulated by separating all the $k$ tapes by $k+1$ $\#$'s and marking where each of the $k$ heads is. Nondeterministic TM can be simulated using 3 tapes - input, simulated, which nodes we're exploring
>**verifier** for language $A$ is an algorithm $V$ with $A=\{w | V \text{accepts } \langle w,c\rangle \text{for some string } c \}$ , with runtime measured relative to $w$ 
>**Recursively enumerable equivalent to Turing-recognizable** 

>$A_{CFG}=\{ \langle G, w\rangle | \text{G is a CFG that generates w} \}$ is decidable by converting $G$ into Chomsky normal form so all derivations can be done in $2n-1$ steps
>$E_{CFG}=\{ \langle G \rangle | \text{G is a CFG and } L(G)=\emptyset \}$ is decidable. Mark symbols that can be turned into terminals; so start by marking all terminals and then mark all LHS variables whose RHS is all marked
>$EQ_{CFG}=\{\langle G, H\rangle | \text{G and H are CFGs and }L(G)=L(H) \}$ is not decidable
>$A_{TM}$ is undecidable based on running the TM on itself, and $\overline{A_{TM}}$ is not Turing-recognizable
>A language is decidable equivalent to whether it and its complement are both Turing-recognizable
>$HALT_{TM}=\{\langle M,w\rangle | \text{M is a TM and M halts on input w} \}$ is undecidable
>Linear bounded automaton is a TM with a finite tape. It is decidable because it has finite number of states so looping can be detected. $E_{LBA}$ is undecidable because we can use it to verify computation histories and $A_{LBA}$ is decidable but $E_{LBA}$ is not

>Rice's Thm - the language of all TM's whose languages have some property is undecidable

>SELF is a TM that prints its own description. The idea is
>Print two copies of the following, the second one in quotes
>"Print two copies of the following, the second one in quotes"
>**Lemma** there is a computable function $q$ such that for any $w$ $q(w)$ is the description of a TM $P_w$ that prints out $w$ and then halts
>**SELF** is a TM that always prints itself. Let $A=P_{\langle B\rangle}$ and $B$ be a TM that given an input $\langle M \rangle$ where $M$ is a portion of a TM, compute $q(\langle M\rangle)$ , combine with $\langle M \rangle$ to get a TM description $\langle SELF \rangle$, and print this description and halt. The key is that $q(\langle B\rangle)$ is $\langle A\rangle$ 
>**Recursion Thm** if $T$ is a TM that computes $t: \Sigma^*\times\Sigma^*\rightarrow\Sigma^*$ there is a TM $R$ that computes $r:\Sigma^*\rightarrow \Sigma^*$ where for every $w$ , $r(w)=t(\langle R\rangle, w)$. We modify $P_w$ to preserve whatever's already on there and then print $w$
>**$A_{TM}$** is undecidable: Assume $H$ decides $A_{TM}$ , then construct the machine $B$ that on input $w$ runs $H$ on $\langle B, w\rangle$ and returning opposite
>$MIN_{TM}$ isn't Turing-recognizable - run the enumerator until a longer machine than itself appears

## Godel incompleteness theorem

>**formal proof** $\pi$ is $S_1,S_2,\dots S_\ell=\phi$ The set of true statements on the natural numbers with plus and times is undecidable, the provable statements are Turing-recognizable, and some true statements are unprovable **incompleteness**
>key idea: our TM can construct a formula with one variable $\exists x, \phi_{M,w}\in \text{Th}(\mathcal{N},+,\times)$ that's true iff $M$ accepts $w$, 

>A is turing-reducible ($\le$ ) to B if A is decidable relative to B which means having if we have an oracle decider for B, A is decidable

## Kolmogorov Complexity
>Incompressible strings cannot be defined as the output of the TM $M$ run on input $w$ consistently, i.e. $|\langle M\rangle | w \ge |x|$ 

## Space

>**savitch's theorem** says for any $f: \mathcal{N} \rightarrow \mathcal{R}^+$ , where $f(n) \ge n$ , $NSPACE(f(n))\subseteq SPACE(f^2(n))$ 
>you solve the problem of whether or not you can go from state $a$ to $b$ in at most $t$ steps recursively, and since $t=2^{f(n)}$ there's $f(n)$ levels
>$PSPACE=\cup_k SPACE(n^k)$ , and obviously $L \subset NL\subset P\subset NP \subset PSPACE = NPSPACE \subset EXP$ 
>PSPACE-complete means $B$ is in PSPACE and every A in PSPACE is polynomial time reducible to to B, and only satisfying second condition is PSPACE-hard
>**TQBF=$\{ \langle \phi \rangle | \phi \text{is a fully quantified Boolean formula} \}$** is PSPACE-complete. If a TM uses space $n^k$ a config uses roughly $O(n^k)$ bits and we can actually construct a formula $C_0(C,C')$ that outputs whether two configurations are connected in polynomial time and size since all adjacent configurations are different only at 3 consecutive tape indices. Next we can construct a formula that outputs whether two configurations have distance $2^i$ with some clever inductive thing, and there's $2^{O(n^k)}$ max distance we need to check. Then, $M$ accepts $x$ iff the formula $\exists C,C', C=C_x, C'=C_{final}, C_{n^k}(C_x, C_{final})$ is a tautology, aka inside of TQBF

>L/NL is languages that are decidable in log space on TM = N/SPACE($\log n$). Our TM has two tapes, a read only and read/write.
>To define NL completeness, we need a notion of reducibility that uses $\log n$ space
>PATH (whether there's a path from s to t in directed graph, $\{(G,s,t) | \text{G is directed graph with path from s to t} \}$) is NL-complete because we can construct a graph of nodes representing configurations id-ed by $\log n$ bits
>PATH can actually reduce to PATH where the graph is regular
>NL = coNL because $\overline{PATH}$ is in NL. 

>a function $f(n)$ is space-constructible if $f(n)\ge O(\log n)$ and the function that maps $1^n$ to binary representation of $f(n)$ is computable in space $O(f(n))$ 
>**space-hierarchy theorem** for any space constructible function $f: \mathcal{N}\rightarrow \mathcal{N}$ , a language $A$ exists that is decidable in $O(f(n)$) space but not in $o(f(n))$ space
>as a result, NL isn't equal to PSPACE, PSPACE isn't equal to EXPSPACE
>a function $t$ where $t(n)\ge O(n\log n)$ is time constructible if the function that maps $1^n$ to binary representation of $t(n)$ is computable in $O(t(n))$ 
>**time-hierarchy theorem** states there's a language $A$ that's decidable in $O(t(n))$ time but not decidable in $o(t(n)/\log t(n))$ time
>$EQ_{REX\uparrow}=\{\langle Q, R\rangle | \text{Q and R are equivalent regex's with exponentiation}\}$ is intractable
## Probability
>**BPP** is class of languages decided by probabilistic polynomial time Turing machine with error probability of $1/3$
>PRIMES is in BPP
>**RL** is languages for which exist randomized $O(\log n)$ space, poly time TM $M$ such that for every $x\in A$ probability of accepting at least 2/3, and for every $x\not\in A$ probability of accepting is 0.
>**RP** is **BPP** except we must have error prob 0

$BPP\subseteq P/poly$ because for all $n$ we can construct a circuit by just choosing the random seed carefully
$BPP\subseteq \Sigma_2\cap \Pi_2$ because our reduction outputs the formula $\exists S_1\dots S_t \forall r\quad M(x, r\oplus S_1)=1 \lor M(x, r\oplus S_k)=1$ 
a sequence of functions $G_n : \{0, 1\}^{s(n)} \rightarrow \{ 0, 1\}^{t(n)}$ is a PRG for time $t(n)$ TM if we have $|Pr_{r\in \{0, 1\}^{t(n)}}M(x,r)=1 - Pr_{r\in \{0, 1\}^{s(n)}}M(x,G(r))|<0.1$  
a function $f_n$ is $\epsilon$-hard to approximate by size $s(n)$ circuits if $Pr_{x\in \{0, 1\}^n} |C(x)=f_n(x)| \le 1/2 + \epsilon$ 

if there is a function sequence $f_n$ computable in $DTime(2^{O(n)})$ and is 0.1-hard to approximate for size $2^{\delta n}$ circuits, $BPP=P$ 
a set collection $T_1\dots T_t$ is a $(k,\delta)$ design if $|T_i|=k$ and for all $i\neq j$ we have $|T_i \cap T_j|\le k\delta$ 
if $m\ge 10ek/\delta$, there exists a $(k,\delta)$ design of size $2^{\Theta(\delta k)}$ which can be found in time $2^{\Theta(k)}$ 
$G(x)=(f(x|T_1),\dots , f(x|T_t))$ is a PRG if we choose $k=C/\delta \log t$ and $t=poly(n)$ and $f_k$ is $0.1/t(n)$-hard to approximate for circuits of size $2^{O(\delta k)}$
therefore, there exists $C$ and if $t(n)$ is a polynomial time function, there is a function computable in $DTime(2^{O(n)})$ that cannot be $0.1/t(n)$ approximated by circuits of size $2^{C\delta n}$, then $BPtime(t(n))\subseteq Dtime(t(n)^{O_\delta(1)})$  by using the PRG $G$ to deterministically simulate the acceptance probability of any randomized TM $M$
$G$ is a PRG because we can transform this into the problem of predicting $G(z_{i+1})$ given $G(z_1),\dots G(z_i)$ which is similar to computing $f$ 

## Circuits
P/poly is all languages $A$ for which there is a constant $k$ and family of circuits $C_n$ that compute $A$ and $size(C_n)\le O(n^k)$ where size includes all vertices and edges. sometimes we care only about uniform circuits, which means there is a polynomial-time or log-space TM that outputs $C_n$ when input $1^n$ 
At least $1-o(1)$ of all functions $f:\{ 0, 1\}^n\rightarrow \{0, 1\}$ have no circuit of size $2^n/(8n)$ 
If $A$ is in $P$, then we have a log-space uniform family that computes $A$ 

$\Sigma_k$-SAT is $\{\exists x_{1,1}\dots x_{1,m_1}\forall x_{2,1}\dots x_{2,m2} \dots \phi(x) | \phi \text{ polynomial size} \}$ , $\Sigma_1$ is NP-complete
$\Pi_k$-SAT is $\{ \forall x_{1,1}\dots x_{1,m_1} \exists \dots \phi(x) | \phi \text{ polynomial size} \}$ , $\Pi_1$ is coNP-complete
when does $\Sigma_k$ or $\Pi_k$ (we include all languages that are complete to $\Sigma_k$-SAT or $\Pi_k$-SAT) include $\Sigma_{k+1},\dots$
If P=NP, then PH is a subset of P. Sketch: Show $\Sigma_2 \subset P$ 

**karp-lipton:** if $NP\subseteq P/poly$, $\Sigma_2=\Pi_2$ . intuitively, using $\Sigma_2$, we can guess a polynomial size circuit for SAT and then force it to work, and then use it to do stuff
we need to verify $c$ is indeed a circuit solving SAT; this is in $\Pi_1$
- for every pair $(s,z)$ where $s$ is an instance of SAT and $x$ is a solution, $c(s)$ must be true
- for every instance $s$ where $c(s)$ is true, $s$ must be solvable (this one is by using $c(s)$ repeatedly to try to construct some solution)
if we have a problem in $\Pi_2$ of $\phi= \forall x \exists y \psi(x,y)$ , then $s(x) = \exists y \psi(x,y)$ is an instance of SAT, and if $c$ is a valid circuit for SAT, $\exists c \forall (x,z) V(c, z) \land c(s(x))$ 

Size$_n(p(n))=\{ f: \{0, 1\}^n\rightarrow \{0, 1\}\ \text{f can be computed by circuit of size p(n)}\}$
if $1\le p(n) \le 2^n/(10n)$ then Size$p(n))\not\subseteq$ Size$(p(n)+O(n))$ because we look at a sequence of functions from $f$ to $0$ where each function is related to the next by perturbing one datapoint.
$DTime(2^{O(2^n)})\not\subseteq P/poly$ since we can just list all the functions computable with circuits of size at most $2^n/(10n)$ 
we can get stronger results by fixing $k$: $\Sigma_4 \not\subseteq Size(n^k)$ 
so actually $\Sigma_2 \not\in Size(n^k)$ 

$AC_i$ are functions computed by circuit families with $size(C_n)\le n^{O(1)}$ and $depth(C_n)\le O(\log^i n)$ , and $NC_i$ consists functions computed by $AC_i$ with fanin at most 2 (easier to assume not gates are pushed down to inputs). $AC_i\subseteq NC_{i+1} \subseteq AC_{i+1}$ . Any circuit of polynomial size computing Parity of $x_1\dots x_n$ has depth at least $\Omega(\log n/\log \log n)$
CNF - And of Ors. DNF - Or of Ands.

> **Circuit lower bound**
> - $AC_0$
> 	- For all $d\ge 2$, Parity of circuit of depth $d$ and size $2^{O_d(n^{1/(d-1)})}$ by clumping groups of size $n^{1/(d-1)}$
> 	- A $s$ restriction for $f:\{0,1\}^n\rightarrow\{0, 1\}$ is when restricting to $s$ vars
> 	- If $d\le \log n / \log \log n$, any depth $d$ circuit for Parity requires size $2^{\Omega(n^{1/(d-1)})}$ 
> 		- **Halsted switching lemma** - let $f$ be a width $w$ DNF formula (each term has at most $w$ things ANDed together). $Pr_{\rho \in R(s,n)}\left[ DT(f|_\rho) > t \right] \le (10pw)^t$
> 		- We can replace the first two layers with a formula with the order of ANDs and ORs switched to merge into the next layer, which reduces depth by 1. This only works if we take a suitable random restriction, so that we can guarantee the tree depth of the first two layers is small, so we don't increase circuit complexity too much. Crucially, the restriction of parity is parity, with a constant shift. We keep reducing depth until depth 2, where we know at least $n2^n$ gates required.
> - $AC_0\left[ p \right]$ is circuits with polynomial size, constant depth, and unbounded fan in with gates and or not $Mod_p$ 
> 	- $MOD_p: \{0, 1\}^n \rightarrow \{0, 1\}$ is 1 if the sum is non-zero mod p. 
> 	- For $p\neq p'$, $MOD_p$ can't be computed by $AC_0\left[ p'\right]$ circuits. Depth $d$ circuit requires size at at least $2^{\Theta_{p,p'}(n^{1/(2d)})}$
> 		- We solve for the $2,3$ case
> 			- Gates can be approximated by low-degree polynomial distribution. Induct over the $d$ layers, there exists a distribution $P$ of polynomial from $\mathbb{F}_3^n$ to $\mathbb{F}_3$ of degree at most $(2t)^d$ such that for all $x\in \{0, 1\}^n$, $Pr_{p\sim P}\left[ p(x)\neq C(x) \right]\le S3^{-t}$ 
> 			- It's hard for a $\mathbb{F}_3$ polynomial to compute sum mod 2 - $Pr_{x\in \{0,1\}^n} p(x)=MOD_2(x) \le 1/2 + O(D/\sqrt{n})$
> 		- $AC_0\left[m\right]$ is harder. If ACC allows any MOD gates, $NTIME(2^{poly(\log n)}) \not\subseteq ACC$ is strongest result.
### Finite field
the probability of a non-zero d-degree multi-variate function equaling 0 in a field $\mathbb{F}_q$ is at most $d/q$

## Interactive Proving systems
$IP$ consists of languages $A$ for which there is an interactive protocol $(V,P)$ , $V$ is a probabilistic polynomial time TM and $P$ is all-powerful, such that on an input $x$ over $p(n)$ rounds messages are sent, and
For every $x\in A$, $Pr\left[ (V,P)(x) \right]\ge 2/3$ and for every $x\not\in A$ and every prover $P^*$, $Pr\left[ (V,P^*)(x) \right]\le 1/3$ 

>**IP$\subseteq$PSPACE**
>IP solves for $a(v)$, the probability verifier accepts after all that has happened $v$. Let $r$ be the starting node, $a(r)=\max_{P^*} Pr(V,P)(x)$ accepts

>**#3-SAT** is the counting of the number of solutions to a 3-SAT formula. It is in IP.
>	- the number of solutions is the sum of the polynomial, $\phi$ over all $2^n$ inputs
>	- prover sends verifier **a polynomial** of what happens if we replace first $k$ inputs of $\phi$ with random stuff, next input as a variable, and sum over remaining variables
>	- there's this "chain" where the validity of each polynomial can be faked but with a low probability

>Graph Non Isomorphism is $\{ (G_1,G_2) | \text{the graphs aren't isomorphic} \}$
>the verifier chooses a random graph, and then sends a permuted version of that graph $H$ to the prover
>the prover outputs which graph is isomorphic to $H$

>**IP=PSPACE**. Suffices to show TQBF is in IP
>Let $\psi = \exists x_1 \forall x_2 \dots \varphi(x_1,\dots x_n)$ and $\psi'=\exists x_1 L_{\le 1} \dots Q_{n}x_n L_{\le n} \varphi(x_1,\dots x_n)$
>- Verifier sets $t=1,c_t=1$
>- Prover sends $f_t(x_t, x_{t+1},\dots)$ which is $\psi'$ with $x_1=\alpha_1,\dots x_{t-1}=\alpha_{t-1}$
>- Verifier checks $\alpha_t = f_t(0, \dots)\cdot f_t(1, \dots)$ if $Q_t=\forall x_t$ and $1-(1-f_t(0,\dots)) (1-f_t(1,\dots))$ if $Q_t=\exists x_t$
>- Verifier checks that $\alpha_n=f_n(\alpha_1,\dots \alpha_n)$
>- if $\psi\not\in A$, $(V,P)(\psi)$ mistakenly accepts iff at least one of the $f_t$ is fake. Degree of $f_t$ is at most $n^3$, so by union bound we have at most $n^4/q$ error probability

>**MIP=NEXP** - multiple non-communicating provers, where $k$ is polynomial in $n$.
>**Succint-3-SAT** - NEXP-complete under polynomial time reductions. It is a formula $\phi_C$ on $2^n$ variables where circuit $C:\{0, 1\}^{n+1}\times \{0, 1\}^{n+1}\times \{0, 1\}^{n+1}\rightarrow \{0, 1\}$ describes the clause is in the formula
>Combine sum-check with low-degree tester. Turns out we need only 2 provers.

>**Low degree testing problem**
>Provers $P_1,\dots P_k$ jointly hold $\tilde{A}: \mathbb{F}_q^n \rightarrow \mathbb{F}_q$, verifier needs to verify the function is low degree. So, the verifier chooses lines and queries the provers on the line; then, it is a 1-dimensional function so easy to verify degree.

### PCPs and Hardness of Approximation
>**weak PCP formulations**
>NP is languages with 2-Prover-1-Round-game protocol $(V,P_1,P_2)$. Verifier samples $O(\log |x|)$ random bits, sends message of length at most $O(\log |x|)$ to each prover, receives response of length $O(1)$ from each, decides to accept/reject $x$ accordingly
>For every $x\in A$, $Pr\left[ (V,P_1,P_2)(x)\right]=1$. There is $\epsilon$ such that for every $x\not\in A$, $Pr\left[ (V,P_1,P_2)(x) \right]<1-\epsilon$
>
>A PCP verifier $V_{PCP}$ for a language $A$ is a probabilistic TM that gets as input $x\in \Sigma^*$ and oracle access to witness $w\in \Gamma^*$ with $|\Gamma|=O(1)$. Verifier uses $O(\log |x|)$ random bits and based on them and $x$ chooses 2 locations $i_1,i_2$ and accepts based on $w_{i_1},w_{i_2}$.
>If $x\in A$, exists witness $w$ with $Pr\left[V_{PCP}(x,w)=1 \right]=1$ and if $x\not\in A$, for all $w$, $Pr\left[ V_{PCP}(x,w)=1 \right]<1-\epsilon$
>Any language $A\in NP$ has a PCP verifier since we can take the 2P1R protocol and extract from the witness which is the truth tables of the provers the prover's responses
>
>**Label cover** involves $\Psi=(G=(V,E),\Gamma,\Phi=\{\Phi_e\}_{e\in E}$ where $\Phi_e\subseteq \Gamma\times\Gamma$ and $|\Gamma|$ is the alphabet size. An assignment is a labeling $A: V\rightarrow \Gamma$  and we're trying to maximize
>$val(\Psi)=|\{e=(u,v)\in E| (A(u),A(v))\in \Phi_e \}| / |E|$ 
>**gap-LabelCover**$\left[ c,s\right]$ has input $\Psi$ where we're promised that $val(\Psi)\ge c$ (yes case) or $val(\Psi)\le s$ (no case), and $M$ solves it if it always accepts on yes case and rejects on no case
>- gap-LabelCover$\left[1, 1-\epsilon\right]$ is NP-hard for some $|\Gamma|=k$, $\epsilon$, using the 2-Prover-1-Round protocol to construct a bipartite graph with vertices based on $P_1$'s and $P_2$'s responses to $V$'s questions
> - **gap-3SAT**$\left[c, s\right]$ is where one is given a 3CNF formula $\phi$ with $val(\phi)\ge c$ or $val(\phi)<s$ and need to distinguish (val($\phi$) is the max fraction of clauses that $A$ satisfies). There exists $\epsilon>0$ with $(1,1-\epsilon)$ is NP-hard

>**weak PCP reductions**
>**gap-IS** - There exist $\epsilon$ where gap-IS$\left[ 1/7, 1/7(1-\epsilon)\right]$ is NP-hard. Given a 3CNF formula we expand each clause into 7 nodes of a graph, and draw edge between $(c_i,\alpha)$ and $(c_j,\beta)$ if $c_i,c_j$ share a variable and $\alpha(x_k)\neq \beta(x_k)$. 
>**Amplification**: $\exists \epsilon > 0$, $\forall \ell \in \mathbb{N}$, gap-IS$\left[ 1/7^ \ell, 1/7^ \ell (1-\epsilon)^{\ell}\right]$ is NP-hard implies $\forall 0<\alpha < 1$, approximating IS within factor $\alpha$ is NP-hard. we do so by constructing an expanded graph with nodes equal to $\ell$-tuples of nodes of $G$, and edges whenever at least one of the $\ell$ pairs of nodes is connected in $G$
>we can flip this to get gap-Vertex Cover$\left[ 6/7, (6+\epsilon)/ 7\right]$. vertex cover has a trivial 2-approx., conjectured optimal
>Does $G$ contain a clique of size $\ge k$? $\alpha$-approx is NP-hard

> **Strong PCP Theorem** - $\forall \epsilon > 0$, $\exists k \in \mathbb{N}$ s.t. gap-ProjLabelCover$\left[ 1,\epsilon\right]$ is NP-hard on bi-regular / (bipartite weaker, proved in class) graphs and alphabet size $\le k$
> Parallel Repetition to improve soundness - $L^t$ and $R^t$, and also $E_t = \{ ( (u_1,\dots, u_t), (v_1,\dots v_t) ) | (u_i, v_i) \in E \forall i =1,\dots t\}$, and $\phi_e(\sigma_1, \dots \sigma_t) = (\phi_{e_1}(\sigma_1), \dots \phi_{e_t}(\sigma_t))$.
> if $val(\Psi)\le 1-\epsilon$, then $val(\Psi^{\otimes t}) \le (1-e')^t$ where $\epsilon'=\Theta(\epsilon^2)$
> Counterexample to naive parallel repetition of 2-prover-1-round-game protocol
> - Verifier samples random bits $b_1,b_2$ to send to each prover; provers sample $(i_1,a_1),(i_2,a_2)\in \{1, 2\}\times \{0, 1\}$ and verifier accepts if $i_1=i_2,a_1=a_2=b_1$. Turns out $val(NA)=val(NA^{\otimes 2})$

>**Strong PCP reductions**
>3SAT - left vertices are clauses; right are vertices
>
>setCover - with weak PCP, we can reduce from vertex cover to get gap-SC$\left[ 6/7, (6+\epsilon)/7 \right]$ is NP-hard for some $\epsilon>0$
>gap-WeightedSetCover$\left[ \ell, \ell / \epsilon\right]$ is NP-hard for all $\epsilon>0$ for some $\ell$. Reduce from strong projection label cover instance $\Psi=(G=(L\cup R, E), \Sigma, \{ \Phi_e\}_{e\in E} )$ to a weighted set cover with universe $E\times U$ where $U$ is the size $2^{|\Sigma_R|}$ universe for a collection of sets $A_1,\dots A_{|\Sigma_R|},B_1,\dots B_{|\Sigma_R|}$, and the available sets are $S_{u, \sigma_u} = \cup_{v:(u,v)\in E} \{(u,v)\}\times A_{\phi_{u,v}(\sigma_u)}$ and $S_{v,\sigma_v}=\cup_{u:(u,v)\in E}\{(u,v) \}\times B_{\sigma_v}$, weighted by $|L|/|R|$ to balance things.
>In fact, this theorem got improved, it's impossible to approximate set-cover within $(1-\epsilon)\log n$

>Maxcut approximation. Known to be NP-hard to approximate to $16/17+\epsilon$, but the best approx algorithm is less than $16/17$. We define a problem where $x_v = \pm 1$ depending on which side of the cut, and then we relax $x_v$ to be a $n$-dimensional vector, and then project these vectors back to $\pm 1$ by selecting a random vector. We end up with $\alpha_{GW} = \min_{z\in \left[-1, 1 \right]} \frac{Arccos(z)/\pi}{(1-z)/2}$ approximation

>**Unique games** - a harder version of strong PCP, where bipartite graph, and there is a 1-1 map $\phi_e:\Sigma\rightarrow \Sigma$ such that $\Phi_e = \{ (\sigma, \phi_e(\sigma)) | \sigma \in \Sigma \}$
>Unique games conjecture says for all $\epsilon,\delta > 0$, exist $k\in \mathbb{N}$ such that gap_UniqueGames$\left[1-\epsilon, \delta \right]$ is NP-hard on instances with alphabet size at most $k$
>Implies $\alpha_{GW}+1$ approximation is NP-hard
>2-to-1 Games Theorem: for all $\epsilon$ exists $k$, gap-UniqueGames$\left[ 1/2, \delta\right]$ is NP-hard with alphabet size at most $k$

### Communication Complexity
A and B want to compute $f: X\times Y \rightarrow \{0, 1\}$, which they both know
A knows x and B knows y; they need to know $f(x,y)$
Let $\Pi$ be a protocol; $CC(\Pi)=\max_{x,y} CC(\Pi(x,y))$; $CC(f)=\min_{\Pi} CC(\Pi)$
A randomized protocol computes $f: X\times Y \rightarrow \{0, 1\}$ if for all $x,y$ we have $\Pr_{r_A, r_B, r_{pub}}\left[ \Pi(x,y) = f(x,y) \right] \ge 2/3$
Let $R(f)= \inf_{\Pi \text{ a randomized protocol for }f} \max_{x,y} CC(\Pi(x,y))$ 

**CliqueVsls**$:X\times Y \rightarrow \{0, 1\}$ where $X=Y= \{0, 1\}^{\binom{n}{2}} \times \{0, 1\}^n$. $A$ has $(G,C)$ where $C$ is clique; $B$ has $(G,I)$ where $I$ is independent set. The problem is whether $|C\cap I|=1$. A protocol is
- if A has $v\in C$ with deg $v$ at most $n/2$, send to B; if B has $v\in I$ with deg v $\ge n / 2$, send to $A$, delete vertices

**DISJ** is $1 - \bigvee_{i=1}^n x_i \land y_i$. The Randomized CC is actually at least $\Omega(n)$. 

**Equality** can be solved in $O(1)$ for random by comparing $\langle x, h\rangle \mod 2$ with $\langle y, h \rangle \mod 2$. Using private randomness, we can make polynomials $A(t)=\sum x_i t^i$, $B(t) = \sum y_i t^i$ and define some prime $p \ge 3n$ to get $O(\log n)$

Let TISP$\left[S, T\right]$ be languages computable by multi-tape TM in time $O(T(n))$ and space $O(S(n))$
Let $A_0 = \{x0^ny | f(x,y)=0\}, A_1 = \{x0^n y | |x|=|y|=n, f(x,y)=1 \}$. If there's a multi-tape TM $M$ that uses $T(n)$ time and $S(n)$ space and accepts $A_1$ and rejects all strings in $A_0$ then $CC(f)=O(S(n)T(n) / n)$
- We construct $\Pi$ by $A$ and $B$ simulating $M$ on $x0^n y$ and every time the head of $M$ on input crosses over to other part, $A$ or $B$ sends the work tapes and $M$'s state to the other so they can continue the simulation
- Consider the language $Palindrom = \{x x^R \}$. If $ST=o(n^2)$ then $Palindrom \not\in TISP(S,T)$. This is because the communication complexity for $f(x,y) = 1_{x = y^R} = \Omega(n)$ 

For lower bounds on single tape TMs, we define a more general notion of CC, randomized-public-coin protocol $\Pi$ has only public randomness, and must have $0$ error.
Let $A_0 = \{x0^ny | f(x,y)=0\}, A_1 = \{x0^n y | |x|=|y|=n, f(x,y)=1 \}$, and if there is a single tape TM $M$ that uses $T(n)$ time and accepts $A_1$ and rejects $A_0$, then $R_0(f) = O(T(n) / n)$
- We construct $\Pi$ by $A$ and $B$ simulating $M$ except $A$ and $B$ jointly choose a random $0\le k \le n$ such that $A$ is in charge of the left of $k$ and $B$ is charge of right of $k$ and they only need to pass $M$'s state during crossings since there's 1 tape. The expected number of crossings is at most $T(n)/n$ by pigeonhole
- As a corollary, any single tape TM for palindrome must have time $\Omega(n^2)$

Circuit lower bounds with communication complexity, for $NC_1$. WLOG fan-in $2$. Let $KW_f = \{ (x, y, i) | f(x) \neq f(y), x_i \neq y_i \}$
Karchmer-Widgerson game is when A gets $x$ with $f(x)=0$ and B gets $y$ with $f(y)=1$ and the goal is to find $i$ such that $x_i\neq y_i$.
- If there is a circuit $C$ with fan-in 2 computing $f$ with depth $d(C)$ then the KW game has a protocol with CC $O(d(C))$. This protocol is starting at the output of the circuit and finding input nodes that evaluate differently until we reach one of the inputs
- It's open to find $f$ where the KW game has lower bounds

Let $f$ be monotone if $x \le y$, then $f(x) \le f(y)$
Let $M_f = \{ (x,y,i) | f(x) > f(y), x_i > y_i\}$
KW game of $M_f$ is similar as above, and analogously if $C$ is a monotone circuit with fan-in 2 computing $f$ with depth $d(C)$ then KW game of $f$ has protocol with CC $O(d(C))$

>application of CC to matching
  Match is whether an undirected graph has a matching of size at least $n/3$. We transform this to a CC problem of finding differences in edges between graphs on $3m$ vertices with $m$ disjoint edges and graphs on $3m$ vertices such that all edges are adjacent to $m-1$ vertices. We transform this into the CC problem of finding a pair out of $m$ pairs that doesn't intersect with $m$ (not $m-1$) vertices which then gets transformed to DISJ and thu sis at most $\Omega(n)$.


