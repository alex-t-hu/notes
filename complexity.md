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

karp-lipton: if $NP\subseteq P/poly$, $\Sigma_2=\Pi_2$ . intuitively, using $\Sigma_2$, we can guess a polynomial size circuit for SAT and then force it to work, and then use it to do stuff
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
## Interactive Proving systems

>**IP=PSPACE** 
>Graph Non Isomorphism is $\{ (G_1,G_2) | \text{the graphs aren't isomorphic} \}$ 

>PCPs and Hardness of Approximation
>Probabilistically Checkable Proofs
>**Clique** decision problem is does $G$ contain a clique of size $\ge k$ . optimization is find size of largest clique, which is NP-hard, and also even $\alpha$-approx is NP-hard

