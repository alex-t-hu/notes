### permutations
length of permutation same as inversions
descent of a permutation is an index $i$ where $w_i>w_{i+1}$
eulerian poly $\sum_{w}x^{des(w)}$
major index is sum of $i$ where $i$ is a descent, and equi-distributed as inversions
number of cycles and records (number of entries greater than all elements to the left of it)
number of exceedances ($w_i>i$) and descents are equidistributed

### young tableau
a way to fill boxes such that numbers are increasing across rows and down columns
$f^\lambda$ is number of standard young tableaux of shape $\lambda$ , it is equal to $n!/(\prod_{x\in\lambda}h(x))$
$\sum_{|\lambda|=n}(f^\lambda)^2=n!$
Schensted insertion algorithm -> permutation to YT -> 3 5 2 4 7 1 6 ->
P and Q of identical shape and $n$ boxes
3 - 1

35 - 12

25
3

12
3

24
35

12
34

...
we can take $P,Q$ and map back to a permutation by working backwards, from where $n,n-1,\dots$, are inside of $Q$
the length of first row of $\lambda$ is longest increasing subsequence in $w$, lenght of first column is length of longest decreasing subsequence in $w$
number of 123-avoiding permutations in $S_n$ is $C_n$ - can rotate $Q$ by 180 degrees, map it to $2n+1-k$ and stick it to $P$ to get a $2xn$ YT
for any permutation and Schensted shape $\lambda$, $\lambda_1+\lambda_2$ is maximal subsequence coverable by two increasing subsequences
shifted young diagram looks like a YD but every row starts 1 block later than previous. there's a hook length formula of $n!/\prod_{a\in \lambda h(a)}$ where $h(a)$ is defined to include an extra "broken leg", the length of the row after the bottom of the original hook

### q-analog
$\left[ n\right]_q=1+q+\dots + q^{n-1}$ 
$\left[ n\right]_q!=\left[1\right]\cdot\dots \left[n\right]$

$\left[n, k \right]_q =\left[n \right]_q! / \left[k\right]_q! / \left[ n-k \right]_q!$ 
the coefficients of $q$ are positive integers called Gaussian coefficients, they're symmetric or palindromic, first increase and then decrease
$\left[n,k \right]_q = \left[n-1 ,k \right]_q+ q^{n-k} \left[n-1 ,k-1 \right]_q$ 
the number of standard YDs that fit inside a $k\times n-k$ rectangle is n choose k, and similarly
$\left[n, k \right]_q = \sum_{\lambda \subseteq k\times n-k} q^{|\lambda|}$ because 
if $yx=qxy,qx=xq,qy=yq$, then $(x+y)^n=\sum_{k=0}^n \left[ n, k\right]_q x^ky^{n-k}$
for any permutation, $\left[ n\right]_q!=\sum_{w\in S_n}q^{inv(w)}$ 
q-multinomial coefficients are $\left[n, n_1,\dots n_r \right]_q = \left[ n\right]_q!/ (\left[n_1\right]_q!\dots \left[n_r\right]_q!)$ 
we have $\left[n, n_1,\dots n_r \right]_q=\sum_w q^{inv(w)}$ where $w$ is a permutation of $\{1^{n_1},\dots r^{n_r}\}$
permutations of $S=\{1^k2^{n-k} \}$ correspond to YD $\lambda \subseteq k\times n-k$

### grassmannian
$Gr_{kn}$ is k-dimensional linear subspaces of n-dimensional space
let $q$ be a prime power, then $Gr_{kn}(\mathbb{F}_q)$ has $\left[ n, k\right]_q$ elements. alternatively look at reduced-row-echelon form, and non-pivot elements form a young diagram in a $k\times n-k$ rectangle

### stirling
stirling numbers of the first kind $s(n,k)=(-1)^{n-k}c(n,k)$  
signless stirling numbers of the first kind $c(n,k)=$ number of permutations in $S_n$ with $k$ cycles
$s(0,0)=1$ and $s(n,0)=0$ 
$\sum_{k=0}^n c(n,k)x^k=x(x+1)\dots (x+n-1)$ because there's $n$ spots to put the last element   
$\sum_{k=0}^n s(n,k)x^k=x(x-1)\dots(x-n+1)$ because 
1
1 1
2 3 1
6 11 6 1
$c(n+1,k)=c(n,k-1)+nc(n,k)$ 

stirling number of second kind $S(n,k)=$ number of set-partitions of $[n]$  into k non-empty blocks
1
1 1
1 3 1
1 7 6 1
1 15 25 10 1
$S(n+1,k)=S(n,k-1)+kS(n,k)$
$\sum_{k=0}^n S(n, k)(x)_k=x^n$ by considering functions that map from $n$-element set to $x$-element set
$S(n,k)$ is number of non-attacking rook placements on triangle board with $n-k$ rooks
Bell number $B_n=\sum_{k=0}^n S(n,k)$
set-partition like $(1,4,6|2,3|5)$ can be visualized as a rook-placement, and if it's non-crossing / non-nesting, both have catalan number possibilities
arc diagram - 1 connected to 4, 4 connected to 6, 2 connected to 3

### eulerian number
$A_{n,k}$ are number of permutations with $k$ descents and number of increasing binary trees on $n$ nodes with $k$ left edges
Eulerian triangle is like
1
1 1
1 4 1
1 11 11 1
$A_{n+1,k}=(n-k+1)A_{n,k-1}+(k+1)A_{n,k}$

### posets and lattices
$a$ is covered by $b$ , $a\lessdot b$ if $a<b$ and there does not exist $c$ with $a< c < b$
hasse diagram is formed by covering relations with larger element on top
a linear extension is a map from $P$ to 1 thru $n$ such that $a\le b\implies f(a)\le f(b)$
we can convert YT into linear extensions
"Baby hook length formula" - given a rooted tree with $n$ nodes, the number of linear extensions of $P$, $ext(P)=n!/\prod_{a\in T}h(a)$ where $h(a)$ is number of nodes including and above vertex $a$
lattice is a special poset with meet $\land$ and join $\lor$ . for any $x,y\in L$, $x\land y$ is unique maximal element of $L$ which is less than or equal to both $x$ and $y$, and $x\lor y$ is unique minimal element of $L$ that is greater than or equal to both $x$ and $y$
boolean lattice $B_n$ contains subsets of 1-n such that $S_1\le S_2 \equiv S_1\subset S_2$
partition lattice $\Pi_n$ contains set-partitions of 1-n such that $\pi\le \sigma$ iff each block of $\pi$ is contained in some block of $\sigma$ (not distributive)
young's lattice $\mathbb{Y}$ is infinite lattice of all Young diagrams ordered by containment, and $L(m,n)$ is sublattice in a $m\times n$ box
lattice is distributive if it satisfies the two distributive laws: $x\land (y\lor z)= (x\land y)\lor (x\land z)$ 
a subset $I$ of $P$ is an order ideal if for any $x\in I$ and $y\le x$, $y\in I$
let $J(P)$ be the poset of all order ideals in $P$ ordered by containment
$P\rightarrow J(P)$ is 1-1 btwn finite posets and finite distributive lattices

### trees
$\sum_{|T|=n} x^T = (x_1+\dots x_n)^{n-2}$ where $x^T = \prod_{i=1}^n x_i^{deg_T(i)-1}$
similarly, $\sum_{T\subset K_{m,n}} wt(T) = (x_1+\dots x_m)^{n-1}\cdot (y_1+\dots y_n)^{m-1}$ where $T$ is a spanning tree of 

prufer code is $n-2$ nums formed by taking away smallest leafs and adding the other node in that edge. we can also define a bipartite prufer code with $m+n-2$ nums

the laplacian is $diag(deg) - A$
$\tilde{L}$ is $n-1\times n-1$ cofactor; $t(G)=det(\tilde{L})$, and this is same as $1/n$ times prod of non-zero eigenvalues
> $L=BB^T$ where $B$ is the $n\times m$ matrix reprenting $G$ with $\pm 1$
   $det(C\cdot D) = \sum det(C_S)det(D_S)$ if $C$ is $k\times m$ and $D$ is $m\times k$ because $det(\tilde{B}_S)$ is $\pm 1$ if $S$ is a spanning tree of $G$ and otherwise $0$

a matrix $B$ is totally unimodular if any minor is either $0,1,-1$ and if every column has two non-zero entries, $\pm 1$, it is totally unimodular

cartesian product of graph is when you connect two pairs of nodes if one pair is equal and the other belongs to the graph. the Hypercube graph of $n$-dimensions is $H_n = H^n$ so it has eigenvalues $\pm 1 ^n$

weighted MTT says $\sum_{\text{T spanning tree}} wt(T) = det(\tilde{L})$ where weight is product of edge weights

arborescence $T$ of $G$ is a directed subgraph where undirected its a spanning tree and directed there's one root $r$ where everything emanates from. $Arb_r(G)$ counts this
$L=diag(d_1,\dots d_n) - A$ where $d_i$ is indegree
Directed MTT says for any $r,k$, $Arb_r(G)=L^{kr}$, the $(k,r)$ cofactor of $L$

effective resistance btwn $A$ and $B$, $R_{AB}(G)$, is $\frac{\sum_{\text{T' spanning tree of G'}} wt(T')}{\sum_{\text{T spanning tree of G}} wt(T) }$ where $G'$ is $A$ and $B$ glued together

complementary graph with a root vertex, then any spanning tree $T$ of $G^+$corresponds to rooted forest $F$ of $G$. Then $F_G(x) = \sum_{\text{T spanning tree of G+}}x^{deg_T(0)-1}$ is gen func for number of rooted forests with some number of connected components, and $F_G(x)=(-1)^{n-1}F_{\overline{G}}(-n-x)$ 

### parking functions
$f:\left[n\right]\rightarrow \left[ n \right]$, car $i$ prefers to park in $f_i$, if not available they'll drive forward to first available
- $f$ is parking function equiv to for any $k$ there's at most $k$ $f_i$ at least $n-k+1$
- exists permutation with $f_i \le w_i$
-  number of weakly increasing parking functions, $f_1\le f_2\dots$ is $C_n$
Tree/Dyck path bijection
- $(n+1)^{n-1}$ parking functions - we biject to labeled trees on $n+1$ vertices by drawing a Dyck path by drawing the diagonals and putting $i$ on diagonal $f_i$
- We construct tree by going through the Dyck path and attaching each vertex to the leftmost open child. We construct Dyck path by doing leftmost DFS and recording children (children may be empty)
- $p_n(x)=\sum x^{area(P)}=\sum_{f} x^{\binom{n+1}{2}- \sum f_i}=\sum x^{inv(T)}$ where inversions of trees is number of $i<j$ such that $j$ is on the path from root $0$ to $i$
- Recurrence $I_0(x)=1, I_n(x) = \sum_{k=1}^n \binom{n-1}{k-1}(1+x+\dots x^{k-1})I_{k-1}(x)I_{n-k}(x)$

### alternating permutation
$w_1<w_2>w_3 \dots$
$p_n(-1)=A_n$
$A_n = \sum_{k\ge 1, \text{ k odd}}\binom{n-1}{k-1}A_{k-1}A_{n-k},A_0=1$, $k$ even is true as well
>EGF - $A(x) = \sum A_n x^n / n!$

$A^{even}(x) = sec(x),A^{odd}(x)=tan(x)$
Seider-Entringer-Arnold triangle
1
0 1
1 1 0
0 1 2 2
...
the numbers 1,1,1,2,5,16,61 -> $A_n$?

### coloring
there's a unique poly $\chi_G(t)$ such that $\chi_G(k)$ is number of $k$-colorings
$\chi_{G}(k)=\chi_{G\setminus e}(k) - \chi_{G / e}(k)$
$\chi_{K_n}(t) = t(t-1)\dots (t-n+1)$

### acyclic orientations
number of ways to orient edges of undirected graph such that there's no directed cycles
$AO(K_n)=2^n-2, AO(F)=2^{|E|},AO(K_n)=n!$
$AO(G)=(-1)^n \chi_G(-1)$
$AO(G)=AO(G\setminus e)+AO(G / e)$

### chordal graphs
every cycle of length at least $4$ has a chord 
perfect elimination ordering $v_1,\dots$ satisfies the set of all $v_j$, where $j<i$ and $v_j$ adjacent to $v_i$ is a clique
$\chi_G(t) = \prod_{i=1}^n (t-a_i)$
### tutte polynomial
- $T_G(x,y)$ satisfies $T_G = T_{G\setminus e} + T_{G / e}$ for all non-loop and non-bridge
- for bridge, $T_g=x\cdot T_{G / e}$
- for loop, $T_G = y\cdot T_{G\setminus e}$
- $T_G=1$ if $G$ has no edges
- $W_G(x,y)=\sum_{A\subseteq E}x^{k(A)-k(E)}\cdot y^{k(A)+|A|-|V|}$ whitney's corank-nullity polynomial of $G$, $k(A)$ is number of connected components of $H=(V,A)$
- $T_G(x,y)=W_G(x-1,y-1)$







