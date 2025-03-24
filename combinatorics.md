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
