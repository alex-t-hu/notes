### Graph
Johnson's algorithm for detecting all cycles
### Fibonacci heap
n vertices, m edges
Prim: insert: n, decrease key m, extract min n
Dijkstra: insert: n, decrease key m, extract min n
d-heap : $\log_d{n}$,  $\log_d{n}$ , $d \log_d{n}$ for the three ops ($n$ is heapsize)
fibonacci heap: $1$,  $1$ , $\log n$ 
- nums stored as bunch of heap-ordered-trees with ptr to smallest root
- extract min splits the HOT with smallest root, merges HOT of same root degree.
	- our potential is # of HOTs (non-negative), so each merge is decreasing potential by 1, so each merge is free
	- we still have to scan max # of tree root degree buckets
- decrease key finds the to-be-deleted node and chops off from parent. if its parent is not a root and loses its second child, the parent also gets chopped off. this is achieved by marking non-root nodes after their first kid gets chopped off, and un-marking root nodes.
	- another benefit of marking, besides implementation, is it pays for cascading cuts if we set potential to be 2 x # marked nodes. we pay for cuts by reduce # marked nodes by 1 and increase # HOT's by 1
MST improve by running Prim until k heap nodes, then contracting, then repeating. 
- m + t log k, so k = 2^(m/t). t' <= m/k, k' = 2^(m/t') >= 2^ k, k' <= n so not too many iterations
- for Dijkstra can't contract, fail

### Persistence
Ephemeral, Partial Persistence (update recent, query past), Full Persistence (update past, query past). Persistent tree:
- fat node: history of each node, binary search $\log{t}$ x everything
- path copying: construct a path corresponding to each update $\log t + \log n$ . $n\log n$ construction
- lazy path copying: potential as full alive nodes (in the most recent tree) so path copying is free
Planar point location (in 2d polygonal tiling, find which a point falls in)
- imagine x as t. $n\log n$ time/space for constructing persistent balanced BST, $\log n$ queries
- using lazy path copying, $n$ space and $\log n$ queries. we need to do stuff with updating red/black bits and rebalancing to make this work

### Splay
Splay trees. $\phi = \sum_x r(x)$ . rank is log of descendant weight sum.
Cost of splaying $x$ to root $t$ is at most $3(r(t)-r(x))+1$ amortized cost. We can splay $x$ to its grandparent, and get amortized cost of $2 + \Delta \phi \le 3(r'(x)-r(x))$ 
what is working set/cache optimality?
Split(x) and Join in $log(n)$ amortized; we can insert/delete using these

### VEB
shortest path when weights $\le C$ 
$m + nC$ by replace PQ with array (d mod C+1). Space C + n (in addition to the graph)
lazy tree of arrays of size $\Delta$ and depth $k$ , so insert in $k$ , delete min $k + \Delta$ , decrease key in $k$ . this is monotone 

VEB queues for k-bit integers recursively store a Q for the top $k/2$ bits, and $2^{k/2}$ Q for the bottom $k/2$ bits. This gives $\log k$ find/insert/delete, and $2^k$ space

hashing find $h$ maps $U \equiv {1, \dots , m}$ into table of size $s \ll m$ . Map $n$ items; $n \le s$ 
universal hashing means we have a family $\forall x, y\in U, |\{h \in H | h(x)=h(y)\}| \le \frac{H}{s}$
uniform distance is stronger
pairwise independent is $\forall x\neq y\in U, P(h(x)=z_{1} \land h(y)= z_{2})=\frac{1}{s^2}$
uniform is $P(h(x)=z) = \frac{1}{s} \forall zx+b \mod p) \mod s$ for some  and  uniform and  
birthday problem: $n$ into $n^2$ produces no collision at least 0.5 probability
2-level hashing Fredman, Kimlos, Szemeredi: hash $n$ items into $O(n)$ space by using quadratic space to hash at each bucket (0.5 chance of working each time; repeat 2 times)

### Flow
Maximum Flow (Ford fulkerson)
Flow $f_e$ , $0\le f_e \le u_e$ , $\sum_w f(v, w) - f(w, v) = 0$ and 
ford-fulkerson: $u_f(v, w) \equiv u(v, w) - f(v, w)$, $u_f(w, v) \equiv u(w, v) + f(v, w)$ . an augmenting path has  positive capacity on flow residual graph.
if numbers at most $U$, max flow is at most $mU$, so we get $m^{2}U$ 
we can reduce residual flow to $(1-\frac{1}{m})^{t}f$, so $t=m\log f=m\log(nU)$ and each augmenting path found in $O(m + n\log n)$ time. for rationals, we have extra factor of $m$ 
We can scale max flow (for integers) by rounding down the LSB of capacities, solve the unit capacity problem by running $m$ BFS's each in $O(m)$ time, to get $m^2\log U$
The dual problem is increasing distance between $s$ and $t$ , if we make APs the shortest path, then any vertex's distances to $s$ is increasing. So, there are $\frac{mn}{2}$ APs, so $m^2n$ 

Blocking flows is a forward flow on graph where each edge is a forward edge in the BFS from start to end. $mn^2$ by constructing blocking flow $n$ times.
- advance (part of retreat/block)
- retreat $mn$ overall
- block $mn^2$ overall, each block path destroys 1 edge. alternatively each block add one unit of flow so $nf$ 
For unit capacity, each block path destroy $n$ edge hence $mn$ overall. Also $m^{1.5}$ because $k+\frac{m}{k}$ max flows (by distance argument)
Scaling this gives $mn\log U$ 
Link-cut tree: add node, get root, cut tree, link tree. advance: link. retreat: cut. augment: capacity adjustment (not sure if part of link-cut)
$mn\log n$ 
Flow decomposition barrier
orlin $mn$ 
Goldberg-rao $m^{1.5}\log U$ 

min cost max flow is same as min cost circulation (just add flows).
also, we define price functions "certificate of optimality" by asserting reduced cost $c_p(vw)=c(vw)+p(v)-p(w)$ is non-negative
alternatively, min cost max flow also can be solved with shortest augmenting paths (using "cost"). however, if there's negative costs, we need to saturate those first, and then fix those using $m$ min cost flows, $m^{2}\log m$ 

### LP
$v^*=\text{min}\space c_1x_1+c_2x_2+c_3x_3$
$A_{11}x_{1}+A_{12}x_{2}+A_{13}x_{3}=b_{1}$
$A_{21}x_{1}+A_{22}x_{2}+A_{23}x_{3}\ge b_{2}$
$A_{31}x_1+A_{32}x_2+A_{33}x_3\le b_3$
$x_1\ge 0$
$x_2 \le 0$
$x_3$ UIS

$v^*=\text{max}\space b_1y_1+b_2y_2+b_3y_3$
$A_{11}^Ty_{1}+A_{21}^Ty_{2}+A_{31}^Ty_{3}\le c_{1}$
$A_{12}^Ty_{1}+A_{22}^Ty_{2}+A_{32}^Ty_{3}\ge c_{2}$
$A_{13}^Ty_1+A_{23}^Ty_2+A_{33}^Ty_3= c_3$
$y_1$ UIS
$y_2 \ge 0$
$y_3 \le 0$ 

>**canonical form**       max $c^Tx,Ax\le b$  
>**standard form**        min $c^{T}x, Ax=b, x\ge 0$ 
>**dual min**                  max $b^Ty$, $A^Ty \le c$
>**feasibility**                 $Ax=b,x\ge0$ 

> proof min = dual min:

dual: flip y, so we have $y^*$ minimizing $b^T y$ and $A^T y^* \ge c$
take a tight and maximally column independent subset $c_S$ , drop columns from $A$, so now we can assume $A^T y^* = c$ and $A$ is full column-rank
if there doesn't exist $x^*$ with $Ax^* = b$, there exists a vector with $A^T z=0$ but $b^T z < 0$ , so we can improve $y$ .
also, $b^T y^* = c^T x^*$ 
also $x^* \ge 0$, or else we can increase $c$, perturb $y^*$ correspondingly, and get a better result

>complementary slackness

let $s = c-A^T y$ with $s\ge 0$ so our duality gap $c^T x - b^T y=s^Tx$, so during optimality either $x_i=0$ or $s_i=0$  

> shortest path

$w^* = \text{max } d_t - d_s$ where $d_j - d_i \le c_e$ for every $e$ 
dual is $v^* = \text{min } \sum c_ey_e$ where $\sum y_{ji} - y_{ij} = -1, 1, 0$ and $y_{ij}\ge 0$ 

>**simplex algorithm**

$x$ is a vertex iff we have some **basis** $B$ with $x_N=0$, $A_B$ non-singular, $A_B^{-1} b\ge 0$ 
$\tilde{c}_N = c_N - c_BA_B^{-1}A_N$ 
if $\ge 0$ , optimal. if $\tilde{c}_i < 0$, can pivot. but need to stop when $x_B$ hits 0.
$y=c_BA_B^{-1}$ is dual optima 

>**Ellipsoid algorithm**

need $n$ variables and possibly exponential number of constraints. need some "separation oracle" that gives violated constraint
$E(D,z)=\{x|(x-z)^TD^{-1}(x-z)\le 1 \}$ 
$D=BB^T$

all vertices at most $n^{O(1)}$ bits, so initial ellipsoid volume $2^{n^{O(1)}}$
construct a new ellipsoid containing $x_1\ge 0$ half centered at $(\epsilon, 0,\dots)$ . can solve to get
$d_1=(1-\epsilon^2),d_i\sim (1-\epsilon^2)^{-1}$ and final to initial volume ratio is around $1-\frac{1}{2n}$ , so $n^{O(1)}$ halving to get volume $1$

### Approximation

>APX are NP optimization problems that have constant ratio solution polynomial in $n$ solution
  Polynomial-time approximation scheme is above but for every $\epsilon$
  Fully polynomial-time approximation scheme in polynomial time in $n$ and $\frac{1}{\epsilon}$ 

>Set cover - smallest subset of sets whose union is the universe. greedy gives $\log n$ approx
>Vertex cover - set of vertices covering all edges. take both vertices on an edge for 2-approx. or, we can relax an integer LP. $2 - O(\frac{1}{\sqrt{\log n}})$ possible
>Independent set is NP-hard and approximation ratio at least $n^{1-\epsilon}$ 

>**Knapsack problem** with max profit $P$ and $n$ items and integer profits can be solved in $nP$ time with DP! This is Pseudopolynomial, because exponential in bitsize, so weakly NP-hard

rounding technique - $\epsilon$ approximation by scale profits to $\lfloor \frac{n}{\epsilon P'}p_{i}\rfloor$ , so new opt is between $\frac{Pn}{P'\epsilon}-n$ and $\frac{Pn}{P'\epsilon}$ , so DP runtime is $\frac{Pn^2}{P'\epsilon}$ . we don't know $P'$ but can try $1,2,4,\dots$ by running DP with fixed $\epsilon$ , and then run DP. this gives $n^2\log n + n^2/\epsilon$ . this example of pseudopoly algo giving FPAS. conversely, FPAS to pseudo poly by say input problem has integers at most $t$, then $\epsilon=\frac{1}{nt+1}$   solves.

>**Pseudopolynomial knapsack** - 
>https://arxiv.org/pdf/2308.04093 https://dl.acm.org/doi/pdf/10.1145/3618260.3649719

>**Online scheduling** - $m$ jobs and $n$ computers. Greedily can assign the job to the computer with least load, and achieve $2-\frac{1}{m}$ because optimal at least $\frac{1}{m}\sum p_j$ and $\max p_j$ , and say our max load machine had $L$ before adding $p_j$ to it; then $L+p_j/m\le OPT$ and $L+p_j \le (2-1/m)OPT$ 
>Longest-processing-time-first offline scheduling - same as before but sort by decreasing time. $4/3$ 
>use **enumeration** to schedule largest $k$ machines optimally and then add last machine, its value is at most $p_{k+1}\le OPT\frac{m}{k+1}$ giving $\epsilon=m/k$ approx. 
>if $k$ distinct types, I can solve decision problem $\le T$ in $mn^{2k}$ with DP. for general case, i guess OPT $T$ , jobs of size $\ge \epsilon T$ is large, round jobs between $\epsilon T$ and $T$ to powers of $1+\epsilon$ which increases optimal by factor of $(1+\epsilon)$ , add smaller jobs with add $\epsilon T$ to optimum at most, which gives $2\epsilon$ approximation
>$2-\frac{1}{70}$ approx: light machines less loaded $\frac{1}{70}$ than heavy machines; small jobs on heavy machines as much as possible

>**Traveling salesman problem**

TSP is minimum edge weight tour (visit each exactly once and return). often on a complete graph
Metric TSP is at least once.
Euclidean TSP has PAS.
Hamiltonian paths can be solved with TSP with edge weights 1 and 2
The MST is at most Metric TSP, and we get 2-approximation by touring each edge at most twice
Held-karp bound 4/3 conjectured; penalizes large degrees

>**factory problem** 

$\min \sum f_iy_i + \sum c_{ij}x_{ij}$  where $x_{ij} \le y_i$ and $\sum_i x_{ij} \ge 1$ and $x_{ij}\ge 0$ . $y_i$ is binary indicating factory open/closed. $x_{ij}$ is binary if $i$ serves customer $j$ . for each $j$ , we get rid of at most $\frac{1}{\rho}$ of the $\sum_i x_{ij}$ with $c_{ij}>\rho C_j$ , and, repeatedly, choose $j$ with smallest $C_j$  and then open facility $i$ connected to $j$ with smallest $f_i$ ; this special $i_{min}$ has $f_{i_{min}}<\sum_{i\in N(j)}f_iy_i/(1-\frac{1}{\rho})$ , and also for any $j'$ with some $i\in N(j)\cap N(j')$ , $x_{i_{min}j'} \le x_{ij'}+ x_{ij} + x_{i_{min}j} \le \rho C_{j'} 2\rho C_{j} < 3\rho C_j$ , so total cost is bounded by $\sum \frac{f_iy_i}{1-\frac{1}{\rho}} + 3\rho \sum c_{ij}x_{ij}$ so balancing gives 4-approx

>**max sat**

$y_i$ for var $i$ , $z_j$ for clause $j$ , try to maximize number of true OR-statements. can either assign $y$ randomly to get a $1-\frac{1}{2^k}$ approximation where $k$ is num variables, or write a linear program: $\max \sum z_j$ where $z_j \le \sum_{i\in p(j)} y_i + \sum_{i\in n(j)} (1-y_i)$ , and then we can round $y_i$ to $1$ with probability $y_i$ and bound probability of $j$ being satisfied by $(1-\frac{1}{k})^{k} z_j$ , and combine to get $\frac{3}{4}$ 

>**fixed parameter tractable** means solution depends on $k$ exponentially only, like **vertex cover** depends on size of vertex cover exponentially. can be shown by for some edge, try choosing either vertex

>**Ski-rental** - i can rent for 1 or T, idk how long need to rent for. well, I just wait until day $T$ and then buy, I get 2-competitive
>Sell-item between $m$ and $M$ , know when is last opportunity to sell
>- say we sell if at least fixed threshold $p$ , and otherwise sell last opportunity. adversary can achieve $p-\epsilon$ while we get $m$ or $M$ while we get $p$, so adversarially the worse ratio is $\sqrt{\frac{M}{m}}$ 
>- if we can sell fractional amounts, let's geometrically uniformly allocate, so $m\cdot 2^i$ for $i\le \log \frac{M}{m}$ , and we get $\log \frac{M}{m}$ ratio 
>- if we can sell randomized amounts, we just randomly set a threshold $m\cdot 2^i$ and achieve the same ratio

>**Paging problem**
>- FIFO, LRU, FWF is $k$-competitive, but not MRU/LIFO
>careful casework by dividing into $k$ consecutive fault chunks, and considering three cases on $q$, the LRU page from the previous chunk
>- faults on $q$ means $k$ distinct requests that aren't $q$, so $k+1$ distinct requests, so OPT must fault at least once
>- faults on some page twice, so similarly $k$ distinct requests in between
>- otherwise, $k$ faults for other pages and $q$ is accessed, so $k+1$ distinct requests
>**adversarial sequence** to fault every step, prophetic algorithm only faults once every $k$ by storing the next $k-1$ elements and smartly evicting

>**Randomized paging**
>- Oblivious adversary outputs sequence at beginning, $k$ -competitive for a random alg means $\mathbb{E}\left[C_A(\sigma)\right]\le k\cdot OPT + O(1)$ 
>- Fully-adaptive adversary knows coin flips
>- Adaptive adversary knows past coin flips at each point
>- RANDOM eviction is $k$-competitive
>**MARKING algorithm** - on fault evict unmarked page randomly and mark its replacement; on access mark; unmark all if all marked (this defines a new **phase**); $\log k$-competitive
>- each phase, start of phase we have $S_i$ in cache, a **clean** request does not belong to $S_i$ and **dirty** request belongs to $S_i$ , if there's $c_i$ clean requests and $k-c_i$ dirty. probability of evicting clean request is 1 and stale is $c/(k-s)$ so we get an upper bound expected evictions of $c_i + c_i/k+\dots + c_i/(k-c_i)=c_i\log k$ , define potential function $\Phi_i$ to be difference between optimal solution's cache and online cache, the cost of offline is at least $c_i-\Phi_i$ because at least that many elements not in offline at the start of phase, and also at least $\Phi_{i+1}$ since those differing elements must be results of evictions, so summing over all phases gives $C_i\ge \frac{1}{2}\sum c_i$
>if we give online $h>k$ for prophetic, LRU gives $h/(h-k+1)$ competitive

>**k-server problem**
>- HARMONIC is randomized, move with probability inverse to goal distance $k^k$ , WORK FUNCTION is $X_i$ minimizes $W_i(X) + d(X_{i-1},X)$ $2k$-competitive; $W_i(X)$ is optimal cost of serving first $i$ requests and ending with servers in state $X$ 
>**on-a-line** double coverage algorithm - if request outside convex hull, move nearest point to it. else, move nearest point on each side to it equal distance until one hits. $k$-competitive
>- $\Phi=kM+\sum_{i<j}d(s_i,s_j)$ where $M$ is min-cost matching btwn optimal algorithm's points and our online algorithm's points. if we try moving a point in our optimal offline algorithm by $d$  , it increases $\Phi$ be at most $kd$ due to its effect on $M$ ; if we then move our two points/1 point to that request point in our online algorithm, we decrease $\Phi$ by at least $d$ , since $M$ doesn't increase and $\sum_{i<j}d(s_i,s_j)$ decreases by $d$ 
### Game Theory
>100 prisoners, 100 boxes with numbers, each looks at 50 boxes independently. win if every prisoner finds their number. **30%** by labeling the boxes and looking at cycles starting at their number

>Deep RL
>Tabular no-regret learning algorithm
>Convex Optimization technique

>**Normal-form games**
>- players $1$ thru $n$ , each player has set of actions $A_i$ and payoff function $u_i: A_1 \times \dots A_n \rightarrow \mathbb{R}$ 
>- a strategy $x_i$ is a probability distribution over actions
>- maxmin $x_i \in \arg \max\limits_{x_i \in \Delta(A_i)} \min\limits_{x_j\in \Delta(A_j) \forall j\neq i}u_i(x_i',x_{-i})$ 
>- in 2-player non-degnerate game, odd # of nash
>- **regret** $r_{i,a_i}(x_1\dots x_n)\equiv u_i(a_i, x_{-i})-u_i(x_1\dots x_n)$ 
>- **nash improvement function** $\varphi : \Delta(A_1)\times\dots\times\Delta(A_n)\rightarrow \Delta(A_1)\times\dots\times\Delta(A_n)$ 
$$\varphi_{i,a_i}\equiv \frac{x_{i,a_i}+\text{pos}(r_{i,a_i}(x_1\dots x_n))}{1+\sum_{a_i'\in A_i}\text{pos}(r_{i,a_i'}(x_1\dots x_n))}$$$$u_i(\varphi_i(x_1\dots x_n),x_{-i})-u_i(x_1\dots x_n)=\frac{\sum_{a_i\in A_i}(r_{i,a_i}(x_1\dots x_n))^2}{1+\sum_{a_i\in A_i}(r_{i,a_i}(x_1\dots x_n))^2}$$
>in 2-player zero-sum game, $u_1=x^TU_1y=-u_2$ and $(x^*,y^*)$ is Nash iff $x^* \in \arg \max\limits_{x\in \Delta(A_1)} \min\limits_{y \in \Delta(A_2)} x^TU_1y$ and similarly $y$ , and we can solve the LP
 $\text{max } v, v\le x^TU_1a_2\quad \forall a_2\in A_2, \sum x_i=1,x\ge 0$ . the set of Nashes are Cartesian product of nonempty, convex, compact sets
 generally, solving 2-player is harder than LP. At least if payoffs are rational solutions are rational, but this doesn't hold for $\ge 3$ players
>in **coarse-correlated/correlated** equilibrium, $\mathbb{E}_{(a_1\dots a_n)\sim \mu}u_i(a_1'\dots a_n) \le \mathbb{E}_{a_1\dots a_n \sim \mu}u_i(a_1\dots a_n)$ or $$\mathbb{E}_{(a_1\dots a_n)\sim \mu}u_i(\phi(a_1)\dots a_n) \le \mathbb{E}_{(a_1\dots a_n) \sim \mu}u_i(a_1\dots a_n)$$ set of $\mu$  is a rational convex polytope
>**existence proof of CCE** - by maximin + convexity, equivalent to finding $\mu$ so that 
>$$\max_{\nu}\min_{\mu}\mathbb{E}_{s\sim \mu}\mathbb{E}_{(i,s_i')\sim \nu}u_i(s_i',s_{-i})-u_i(s_i,s_{-i})\le 0$$ pick $\mu$ outputs $(s_1,\dots s_n)$ with prob $\nu_{1,s_1}\dots \nu_{n,s_n}$ 
>**ellipsoid against hope** applies ellipsoid method; computational, constructive proof of minimax theorem

>**hindsight rationality and $\Phi$-regret** . learning a game means there doesn't exist transformation $\phi: \mathcal{X}\rightarrow\mathcal{X}$ of strategies applied to historical play that gives strictly better utility
>convex and compact $\mathcal{X}$ and set $\Phi$ of $\phi$'s, a $\Phi$-regret minimizer interacts with environment thru 
>- NextStrategy outputs $x^{(t)}$ 
>- ObserveUtility$(u^{(t)})$ outputs $u^{(t)}: \mathcal{X}\rightarrow\mathbb{R}$ and can depend adversarially on $x^{(1)},\dots x^{(t)}$ 
>- $\Phi Reg^{(T)}\equiv \max_{\hat{\phi}\in \Phi} \sum_{t=1}^Tu^{(t)}(\hat{\phi}(x^{(t)}))-u^{(t)}(x^{(t)})$ , want to be **sub-linear**. **linear utility** where $u^{(t)}=\langle g^{(t)} , x \rangle$ 
>- Special case when $\hat{\phi}$ is constant is important.
>- for normal-form strategy space $\mathcal{X}=\Delta(A)$ , $\Phi$ could be all stochastic matrices (**swap regret**), or just moving probability mass from one to another action (**internal regret**)
>- **billinear saddle point solving** $\max_{x\in \mathcal{X}}\min_{y\in \mathcal{Y}} x^T Uy$ by learning competing $x^{(t)}$ and $y^{(t)}$ with $u_{\mathcal{X}}^{(t)}:x\rightarrow (Uy^{(t)})^T x\quad u_{\mathcal{Y}}^{(t)}: y\rightarrow -(U^T x^{(t)})^T y$ . **saddle-point gap** $\gamma(x,y)=\max_{\hat{x}\in\mathcal{X}}x^T Uy-\min_{\hat{y}\in \mathcal{Y}} x^T U \hat{y} \ge 0$ actually satisfies $\gamma(\overline{x}^{(T)}, \overline{y}^{(T)})=\frac{\text{Reg}_{\mathcal{X}}^{(T)} +\text{Reg}_{\mathcal{Y}}^{(T)} }{T}$ 
>- **minimax thm**: if we have a sub-linear regret minimizer $\mathcal{R}_{\mathcal{X}}$  $\max_{x\in \mathcal{X}} \min_{y\in \mathcal{Y}} x^T Uy\le \min_{y\in \mathcal{Y}} \max_{x\in \mathcal{X}} x^T Uy$ obvious, and assume $y^{(t)}$ adverserial $\in \arg \min_{y\in \mathcal{Y}}(x^{(t)})^T Uy$ . then LHS at least $\frac{1}{T}\sum_{t=1}^T (x^{(t)})^T Uy^{(t)}\ge -\frac{\text{Reg}_{\mathcal{X}}^{(T)}}{T}+\min_{y\in\mathcal{Y}}\max_{x\in\mathcal{X}}x^T Uy$ 
>- **correlated/coarse-correlated convergence** - $\mu^{(T)}\equiv \frac{1}{T}\sum_{t=1}^T x_1^{(t)}\otimes\dots x_n^{(t)}$ , then $\max_{\phi\in \Phi}\mathbb{E}_{a\sim \mu^{(T)}} u_i(\phi(a_i),a_{-i})-u_i(a_i,a_{-i})\le \Phi\text{-Reg}_i^{(T)}/T$ 

>**linear regret algos**
>- $r^{(t)}\equiv \sum_{\tau=1}^t (g^{(\tau)} - \langle g^{(t)}, x^{(t)} \rangle 1  )$ 
>- **regret matching strategy**: if $\left[ r^{(t-1)} \right]^+\neq 0$, $x^{(t)} \leftarrow \frac{\left[ r^{(t-1)} \right]^+}{||\left[ r^{(t-1)} \right]^+||_1}$ ; $r^{(t)}\leftarrow r^{(t-1)}+g^{(t)}-\langle g^{(t)},x^{(t)}\rangle 1$; otherwise $x^{(t)}$ arbitrary. achieves $\text{Reg}^{(T)}\le \sqrt{T\cdot |A|} \cdot ||g^{(t)}||_{\infty}$ 
>- **regret matching+** is add a + to the $r^{(t)}$ update
>- **multiplicative weights update**  - $x_a^{(t)}=\text{softmax}_a(\eta r^{(t-1)})$  
>- **strong convexity** $(\nabla f(x) - \nabla f(y))^T (x-y) \ge m||x-y||_2^2$ 
>- **Follow-the-regularized-leader algorithm** $x^{(t)}\equiv \arg \max_{\hat{x}\in\mathcal{X}} \langle r^{(t-1)}, \hat{x} \rangle - \frac{1}{\eta} \psi(\hat{x})$ for strong convex $\psi$ of $m= 1$
>- **online mirror descent** - $x^{(t)}\equiv \arg \max_{\hat{x}\in\mathcal{X}} \langle g^{(t-1)},\hat{x}\rangle -\frac{1}{\eta}D_{\psi}(\hat{x}||x^{(t-1)})$ 
>- **online projected gradient ascent** - replace $D_\psi$ with $||\hat{x}-x^{(t-1)}||^2$ 

>**optimism**
>- predictive version inserts a predicted gradient $m^{(t)}=g^{(t-1)}$ 
>- for **Legendre regularizers** $\psi$'s gradients infinite at $\mathcal{X}$'s boundary, FTRL, non-reflected OMD, reflected OMD coincide
>- **non-reflected** $z^{(t+1)}\equiv \arg\max_{z\in \mathcal{X}} \langle g^{(t)},z\rangle - \frac{1}{\eta} D_{\psi}(z || z^{(t)})$ , $x^{(t+1)}\equiv \arg\max_{x\in \mathcal{X}} \langle m^{(t+1)},x\rangle - \frac{1}{\eta} D_{\psi}(x || z^{(t+1)})$ 
>- **reflected** $x^{(t+1)}\equiv \arg\max_{x\in \mathcal{X}} \langle g^{(t)}+m^{(t+1)}-m^{(t)},x\rangle - \frac{1}{\eta} D_{\psi}(x || x^{(t)})$
>- **optimistic MWU** $x^{(t+1)}\propto \exp(\eta r^{(t)} + \eta(r^{(t)} - r^{(t-1)}))$ 

>**bandit regret minimizer** assumes only $w^{(t)}\equiv \langle g^{(t)}, x^{(t)}\rangle$ is known
>- PseudoReg$^{(T)}\equiv \max_{\hat{x}\in\mathcal{X}} \mathbb{E}\left[ \sum_{t=1}^T \langle g^{(t)},\hat{x}\rangle - \sum_{t=1}^T \langle g^{(t)},x^{(t)}\rangle \right]$ 
>- expected regret $\mathbb{E}\left[ \max_{\hat{x}\in\mathcal{X}}\sum_{t=1}^T \langle g^{(t)},\hat{x}\rangle - \sum_{t=1}^T \langle g^{(t)},x^{(t)}\rangle  \right]$
>- high-probability regret guarantees $\mathbb{P} \max_{\hat{x}\in\mathcal{X}}\sum_{t=1}^T \langle g^{(t)},\hat{x}\rangle - \sum_{t=1}^T \langle g^{(t)},x^{(t)}\rangle \le o(T)\sqrt{\log \frac{1}{\delta}}\ge 1-\delta$ 
>- estimate the gradient $\tilde{g}^{(t)}$ , use that to compute the full-info regret minimizer to output $y^{(t)}$ , add some exploration noise $\xi^{(t)}$ , get some action distribution $p^{(t)}$, sample some action according to $x^{(t)}$ , and make our unbiased gradient estimator $\frac{w^{(t)}}{p_{a^{(t-1)}}^{(t-1)}}e_{a^{(t-1)}}$ 
>- **Exp3** applies **MWU** with learning rate $\eta=\sqrt{ \log |A| / T(|A|) }$ guaranteeing PseudoReg$^{(T)}=O(\sqrt{T|A|\log |A|})$ 

>**General $\Phi$-regret minimizer** 
>- ObserveUtility($u^{(t)}$) constructs $U^{(t)}(\phi)=u^{(t)}(\phi(x^{(t-1)}))$ and feeds this utility function to an external regret minimizer achieving regret Reg$_\Phi^{(T)}$ which outputs $\phi^{(t)}\in \Phi$ (view the transformations, which are linear, as actions). We have a fixed point oracle that outputs a fixed point $x^{(t)}\in \mathcal{X}$ that's our action. The claim is $\Phi$-Reg$^{(T)}$ is equal to Reg$_\Phi^{(T)}$ 

>**kuhn poker** - 3 cards, players take turn checking and folding
>at each decision point $j_1,\dots j_6$ CFR will use six local regret minimizer $\mathcal{R}_1,\dots \mathcal{R}_6$ that outputs local strategy $b_j\in \Delta(A_j)$ 
>let Reg$_j^{(T)}$ be regret cumulated to time $T$ by regret minimizer $\mathcal{R}_j$ ; then $\text{Reg}^{(T)}\le \sum_{j\in \mathcal{J}}\max(0,\text{Reg}_j^{(T)})$ 
>CFR:
>- the tree-form decision process has decision points, observation points, transition functions
>- NextStrategy: construct sequence-form representation $x^{(t)}\left[ja\right]$ for each decision point $j$ , query $\mathcal{R}_j$ for actions $b^{(t)}_j$ , get the parent $p_j$ and do $x^{(t)}\left[ja\right]=x^{(t)}\left[p_j\right]\cdot b_j^{{t}}$ 
>- ObserveUtility: for each node, if it is action node $j$ , $V^{(t)}\left[j\right]=\sum_{a\in A_j} b_j^{(t)}\left[a\right] \cdot (u^{(t)}\left[ja\right]  + V^{(t)}\left[ \rho(j,a) \right])$ , or $V^{(t)}\left[j\right]=\sum_{s\in S_k} V^{(t)}\left[ \rho(k,s) \right]$ , and each local $\mathcal{R}_j$ observes utility $u^{(t)}_j=u^{(t)}\left[ja\right]+V^{(t)}\left[\rho(j,a)\right]$ and calculates the next $b_j^{(t)}$ 

>**sequential irrationality and perfect equilibria** - some parts of game tree are unreachable, so various nashes can do irrational things there.
>**Extensive-form perfect equilibrium (EFPE)** forces $x_{ja}\ge \epsilon x_{p_j}$ 
>**Quasi-perfect equilibrium (QPE)** - bound each sequence $x_{ja}\ge \epsilon^{|A_j|}$ 
>in a 2-player zer-sum game, the **Trembling Linear Program** TLP is $\max_{x} c(\epsilon)^T x,\quad A(\epsilon)x=b(\epsilon), x\ge 0$ 
>- **stable basis** $B$ if $B$ is optimal for the TLP for all $0<\epsilon<\overline{\epsilon}$ for some $\overline{\epsilon}$  called **Negligible positive perturbation NPP**, and they always exist and efficiently computed
>- alternatively we can use an oracle to tell us if an $\epsilon$ -basis is stable and keep making $\epsilon$ smaller; NPP bit-size is poly

>**magnetic mirror descent** updates a policy by $\pi_{t+1}=\arg\max_{\pi}\langle q, \pi \rangle - \frac{1}{\eta} \text{KL}(\pi, \pi_t)-\alpha \text{KL} (\pi, \rho)$ 
>**Dec-POMDP** - Decentralized Partially Observable Markov Decision Processes1

>**combinatorial games** - for each player $i$, set of deterministic strategies can be represented as set $\mathcal{V}_i$ of bit strings; utility of every player is multilinear function of strategies, including normal-form and extensive-form
>**kernelized vertex MWU/OMWU**
>- NextStrategy play $x^{(t)}=\sum_{v\in \mathcal{V}_i} (\lambda^{(t)}\left[v\right]\cdot v)$ 
>- ObserveUtility($g^{(t)}$) - if optimistic, $\tilde{g}^{(t)}=2g^{(t)}-g^{(t-1)}$ otherwise use $g^{(t)}$ , and update $\lambda^{(t+1)}\left[v\right]\propto \lambda^{(t)}\left[v\right]\cdot\exp(\eta\cdot\langle \tilde{g}^{(t)},v\rangle)$ 
>- exists kernel function $K_{\mathcal{V}}:\mathbb{R}^d\times \mathbb{R}^d \rightarrow \mathbb{R}$ that can implement with $d+1$ evaluations per iteration
>- **0/1-polyhedral feature map** - $\phi_{\mathcal{V}}:\mathbb{R}^d\rightarrow \mathbb{R}^{|\mathcal{V}|}$ satisfies $\phi_{\mathcal{V}}(x)\left[v\right]\equiv \prod\limits_{k:v\left[k\right]=1}x\left[k\right]$ 
>- **0/1-polyhedral kernel** - $K_{\mathcal{V}}:\mathbb{R}^d \times\mathbb{R}^d\rightarrow\mathbb{R} = \langle \phi_{\mathcal{V}}(x),\phi_{\mathcal{V}}(y)\rangle$ 
>- $\lambda^{(t)}=\frac{\phi_{\mathcal{V}}(b^{(t)})}{K_{\mathcal{V}}(b^{(t)}, 1)}$ where $b^{(t)}=\exp(\eta\sum_{\tau=1}^{t-1} \tilde{g}^{(\tau)} )$ 
>$$(\sum_{v\in\mathcal{V}}\lambda^{(t)}\left[v\right]\cdot v)\left[k\right]=(1-\frac{K_{\mathcal{V}}(b^{(t)},1-e_k)}{K_{\mathcal{V}}(b^{(t)},1)})$$
>- **extensive-form games** can be computed efficiently by DP with $K_I(x,y)\equiv\sum_{v\in \mathcal{V}_{\succcurlyeq I}} \prod_{k:v\left[k\right]=1}x\left[k\right]y\left[k\right]$

>**search**
>- $\alpha$ is minimum score that maximizing player is assured of, and $\beta$ is maximum score that minimum player is assured of. used for cutoffs.
>-parallelizing $\alpha$-$\beta$ is hard because you have to balance getting good $\alpha-\beta$ estimates first in order to do more pruning, and also parallelizing each node. 

### Optimization
>maxcut can be solved with a cool relaxation efficiently
>**separation oracle** for $\Omega= \{M\in \mathbb{R}^{n\times n}:M \ge 0\}$ is if $Y$ isn't symmetric, $Y\not\in \Omega, E_{ij}-E_{ji},0$ and otherwise for some bad eigenvector $w$ $Y\not\in \Omega, ww^T , 0$ Ellipsoid can be used for finding feasibility and optimization for a convex/compact set and function.
>LP is in **co-NP** (for all false instances there's a polynomially-sized certificate to a polynomially-sized verification algorithm).
>- **Farkas lemma** if $Ax\le b$ does not have a solution, exist $y\ge 0$ with $A^T y=0$ and $b^T y<0$ 
>**cones** satisfy for any $x\in S$ and $\lambda\ge 0$ , $\lambda x\in S$ . the **normal cone** is $\{\sum_{j\in I(x)} \lambda_j a_j : \lambda_j \ge 0 \}$
>**Karush-Kuhn-Tucker** conditions say at some minimization optimum where $g_i(x)\le0$ and $h_i(x)=0$ , $-\nabla f(x)$ should be a non-negative linear combo at the binding $i$ (where $g_i(x)=0$ ) of $\nabla g_i(x)$ plus linear combo of $\nabla h_i(x)$ . however, this KKT can be a failure if the optimal point occurs at some "sharp" vertex where the gradients are linearly dependent! KKT holds at a minimizer if
>- if $g_j$ for critical $j$ are concave around $x$ and $h_i$ are affine
>- if they're linearly independent
>- if $g_j$ are convex and $h_i$ are affine and exists a point $x_0$ such that $g_j(x_0)<0$ for all binding $j$ 

>**Second-order conic programming/Lorentz cone** satisfies $z\ge ||x||_{n-1}$ 
>**Semidefinite programming** is $\min_x f(x), Ax=b$ and $x$ is positive semidefinite
>**Co-positive programming** is when $x$ satisfies $a^T x a \ge 0$ for all $a \ge 0$ , for dimensions at most 4 such $x$ is the sum of positive matrices and positive semidefinite matrices for dimension up to $4$. Optimization is more than poly time, since max independent set can be converted.
>**Slater's condition** says the normal cone of an intersection of convex sets is the sum of the normal cones of each individual convex set whenever the intersection has some point in the interior i.e. **strictly feasible**, to rule out pathological conditions
>**Polar cone of $\mathcal{K}^{\perp}=\mathcal{N}_{\mathcal{K}}(0)=\{d:\langle d,y\rangle\le 0\forall y\in \mathcal{K}\}$**  , **dual cone** is $\mathcal{K}^*=\mathcal{N}_{\mathcal{K}}(0)=\{d:\langle d,y\rangle\ge 0\forall y\in \mathcal{K}\}$ . Lorentz, non-negative, semidefinite cones are self-dual. dual cone of co-positive cone is **totally positive cone** $\mathcal{P}^n = \{ \sum_{i=1}^k z_i z_i^T: z_i \in \mathbb{R}^n_{\ge 0} \}$ 
>the normal cone at $x$ is just $\{d\in \mathbb{R}^n : \langle d, x\rangle = 0, d\in \mathcal{N}_{\mathcal{K}}(0) \}$ 
>Dual is $\max \langle b, \lambda \rangle$ where $z=c-A^T \lambda,\quad\lambda\in\mathbb{R}^n,z\in\mathcal{K}^*$ 
>if strict feasibility of primal not satisfied, supremum of dual could match primal but dual doesn't have maximizer, or dual has a different optimal

>**Gradient descent**
>- **L-smooth function** $||\nabla f(x) - \nabla f(y)||_2 \le L||x-y||_2$ or $||\nabla^2 f||\le LI$  and clearly $f(y)\le f(x)+\langle \nabla f(x),y-x\rangle + L/2||y-x||_2^2$ 
>- **gradient descent lemma** - for L-smooth functions, if $\eta\le 1/L$, $f(x_{t+1})\le f(x_t)-\eta/2||\nabla f(x_t)||_2^2$
>- adding up and telescoping, we get $||\nabla f(x_t)||_2 \le \sqrt{\frac{2}{\eta T}\cdot (f(x_0)-f_*)}$ 
>- **Euclidean mirror descent lemma** says if $f$ is convex, any consecutive $x_t$, $x_{t+1}$ produced by gradient descent $f(x_t)\le f(y)+1/(2\eta)\left( ||y-x_t||_2^2-||y-x_{t+1}||_2^2+||x_{t+1}-x_t||_2^2 \right)$ 
>- assuming additionally $f$ is L-smooth with $\eta \le 1/L$ , we can telescope to get $f(x_t)-f(x_*)\le  ||x_*-x_0||_2^2 / (2t\eta)$ . however, convergence of $||x_t-x_*||$ can still be arbitrarily slow
>**Faster convergence with PL condition**
>- **strongly convex** means $\nabla^2 f(x) \ge \mu I$ 
>- **PL condition** means $1/2||\nabla f(x)||^2 \ge \mu(f(x)-f_*)$ 
>- $\frac{1}{2}||\nabla f(x)||^2 \ge \mu(f(x)- f_*)$ where $f_*$ is global min, then $\eta=\frac{1}{L}$, $f(x_t)-f_* \le (1-\mu/L)^t (f(x_0)-f_*)$ , and GD converges to $x_*$ with $||x_t-x_*||_2^2 \le 8\eta L^2/\mu^2(1-\mu/L)^{t-1}(f(x_0)-f_*)$ 
>**Momentum**
>$x_{t+1}=(1-\tau)y_t+\tau z_t$
>$y_{t+1}=x_{t+1}-1/L\nabla f(x_{t+1})$ short gradient step
>$z_{t+1}=z_t-\alpha\nabla f(x_{t+1})$ long gradient step
>$f(x_{t+1})-f_* \le 1/(2\alpha)( ||x_* - x_t||_2^2 - ||x_* - z_{t+1}||_2^2 + ||z_t - z_{t+1}||_2^2 ) + \frac{1-\tau}{\tau} (f(y_t)-f(x_{t+1}))$ 
>if we choose $\alpha=\frac{||x_*-z_0||_2}{\sqrt{2(f(x_0)-f_*)}}$ and $\tau=1/(1+\alpha L)$ then within $T_{1/2}= \frac{2||x_* - x_0||_2\sqrt{2L}}{\sqrt{f(x_0)-f_*}}$ iterations (this is square root of the above) we get some iterate which is twice as close

>$\varphi: \Omega\rightarrow \mathbb{R}$ is differentiable and $\mu$-strongly convex if $\varphi(x)\ge \varphi(y)+ \langle \nabla\varphi(y),x-y\rangle+\mu/2||x-y||^2$ 
>the **bregman divergence** is $D_{\varphi}(x||y)= \varphi(x)-\varphi(y)-\langle \nabla \varphi(y), x-y\rangle$ and for negative entropy it is KL divergence, and for half squared norm it is half squared distance. $D_\varphi(x||y)$  is $\mu$-strongly convex wrt $x$ 
>**mirror descent** is $x_{t+1}= \arg\min_{x\in \Omega} \eta\langle \nabla f(x_t), x\rangle +D_{\varphi}(x || x_t)$ ; for half squared norm equivalent to Euclidean projection, and negative entropy it is softmax update $x_{t+1}\propto x_t \odot \exp(-\eta \nabla f(x_t))$ 
>**three point inequality** - for some generic proximal set $x'=\text{Prox}_{\varphi}(g, x)$ , $\langle -g, y-x'\rangle \le -D_{\varphi}(y || x') + D_{\varphi}(y || x) - D_{\varphi} (x' || x)$ for all $y\in \Omega$ 
>**mirror descent lemma** if $f$ is convex,  $f(x_t) \le f(y) + \langle \nabla f(x_t), x_t-x_{t+1}\rangle - \frac{1}{\eta}D_{\varphi}(y||x_{t+1}) + \frac{1}{\eta}D_{\varphi}(y||x_{t}) - \frac{1}{\eta}D_{\varphi}(x_{t+1}||x_t)$ 
>it follows by telescoping if $x_*\in \Omega$ is minimizer, then $f(x_t)-f(x_*) \le D_{\varphi}(x_* || x_0) / (\eta t)$ for some $t$ 
>**general gradient descent lemma** - $0<\eta \le \mu/L$ , and $f$ is L-smooth with respect to norm $||\cdot ||$ for which $\varphi$ is strongly convex, each step of mirror descent satisfies $f(x_{t+1})\le f(x_t)-\frac{\mu}{2\eta}||x_t-x_{t+1}||^2$ 

>**stochastic gradient descent lemma** - for L-smooth function,
>$\mathbb{E}\left[f(x_{t+1})\right] \le f(x_t) - \eta||\nabla f(x_t)||_2^2 + \frac{L}{2}\eta^2 \mathbb{E}_t\left[ ||\tilde{\nabla} f(x_t)||_2^2 \right]$ 
>if that last variance term is at most $G$ , we can sum these gradients up over all $T$ and picking $\eta=\frac{1}{\sqrt{T}}$ gives a $\mathbb{E}\left[||\nabla f(x_t)|| \right]=\frac{1}{\sqrt{T}}$ same as deterministic case
> **stochastic euclidean mirror descent lemma** - $f$ convex, two consecutive iterates produced by SGD
> $f(x_t)\le f(y) + \frac{1}{2\eta} \mathbb{E}_t \left[ ||x_t-y||_2^2-||x_{t+1}-y||_2^2+||x_t-x_{t+1}||_2^2 \right]$ 
> similarly as before, if $f$ is L-smooth, we choose $\eta=\frac{1}{GT}$ to destroy that gradient term, we get some $t$ with $\mathbb{E}\left[f(x_t)-f(x_*)\right]\le \frac{\sqrt{G}}{2\sqrt{T}}(1+||x_0-x_*||_2^2)$ 
> if $f$ is now strongly-convex, we can get a $\frac{1}{T}$ bound choosing a good stepsize

>**distributed optimization and ADMM** - goal is to minimize $f(x)=1/m\sum_{j=1}^m f^j(x)$ 
>choose learning rate $\eta>0$ and multiplier $\lambda>0$ and optimize
>$v=\max_{\alpha^i}\min_{x,x^j}  1/m\sum_{j=1}^m f^j(x^j)+\lambda||x_j-x||_2^2 + \langle \alpha^j, x^j-x\rangle$ 
>- assuming fixed $x_{t-1}$ and $x_{t-1}^j$ , optimize $\alpha_t^j= \alpha_{t-1}^j + \eta(x_{t-1}^j - x_{t-1})$
>- assuming fixed $\alpha_t^j$ and $x_t$, each machine solves
>$x_{t+1}^j=\arg\min_{x^j\in\mathbb{R}^n} f^j(x^j)+\lambda||x^j-x_t||_2^2+\langle \alpha_t^j, x^j-x_t\rangle$ 
>- $x_{t+1}=1/m\sum_{j=1}^m x_{t+1}^j + 1/(2m\lambda)\sum_{j=1}^m \alpha_t^j$ 

>**Newton's method**
>- $x_{t+1}=x_t-\eta (\nabla^2 f(x_t))^{-1} \nabla f(x_t)$  (minimizer with 2nd order approximation) can be unstable if Hessian ill-conditioned
>$x_{t+1}-x_*=(I-\eta H_t)(x_t-x_*)$ where $H_t=(\nabla^2 f(x_t))^{-1}\int_0^1 \nabla^2f(x_*+\lambda(x_t-x_*))d\lambda$ and $x_*$ is some local min
>**local convergence**
>- $x_*$ is local min with strong curvature, $\nabla^2 f(x_*)\ge \mu I$ 
>- Hessian is **M-Lipschitz continuous**, $||\nabla^2 f(x) - \nabla^2 f(y)||_2 \le M\cdot ||x-y||_2$ 
>- if we start from $||x_0-x_*||_2\le \mu/(2M)$ then our distance exponentially decays $||x_{t+1}-x_*||_2/(\mu/M)\le (||x_{t}-x_*||_2/(\mu/M))^2$ - **quadratic convergence**
>**global convergence** if $f$ is both $\mu$-strongly convex and $L$-smooth, then if $\eta \le \mu/L$ , we get $||x_{t+1}-x_*||_2 \le (1-\eta \mu/L)||x_t-x_*||_2$ 
>- cubic-regularized $x_{t+1}\in \arg\min_{x} \eta(\langle \nabla f(x), x-x_t\rangle +1/2 \langle x-x_t, \nabla^2f(x_t) (x-x_t)\rangle) + 1/6 ||x-x_t||^3$

>**AdaGrad and ADAM**
>$s_t=\sqrt{\sum_{\tau=0}^t (\nabla f(x_\tau))^2}$ and $x_{t+1}=x_t-\eta s_t^{-1} \nabla f(x_t)$
>ADAM additionally does $g_t=\gamma g_{t-1} + (1-\gamma)\nabla f(x_t)$ and $x_{t+1}=x_t-\eta s_t^{-1}g_t$ 
>**competitiveness with pre-conditioning** for convex $f$ , and $\lambda_i\ge0$ , $\eta=D/\sqrt{2}$ , $1/T\sum_{t=0}^{T-1}f(x_t)\le f(x_*)+\sqrt{2n}D/T\sqrt{\min_{||\lambda||_1=n} \sum_{t=0}^{T-1} \nabla f(x_t)^T \lambda^{-1} \nabla f(x_t) }$ where $D=\max_{t=0}^T ||x_t-x_*||_{\infty}$ 
>- view AdaGrad update as mirror descent with $\varphi_t(x)=1/2 x s_t x$ , apply mirror descent lemma and do telescoping
>**sign descent as steepest descent under max-of-max norm**
>- $\arg\min_{\Delta w\in\mathbb{R}^n}g^T\Delta w+\lambda/2 ||\Delta w||_{\infty}^2=-\frac{||g||_1}{\lambda}\text{sign}(g)$  
>- $\arg\min_{\Delta W_1\dots \Delta W_L} \sum\langle G_{\ell},\Delta W_{\ell}\rangle+\lambda/2\max ||\Delta W_{\ell}||_{\ell_1\rightarrow \ell_{\infty}}^2= -\eta\cdot\text{sign } G$  
>- without EMA, adam is $x_{t+1}=x_t-\eta\text{ sign}(\nabla f(x_t))$ 

>**Shampoo**
>initialize $W=0_{n_1\times\dots\times n_k}$ and $H_0^i=\epsilon I_{n_i}$ . for each step, get a gradient $G_t = \nabla f_t(W_t)$
>for $i=1,\dots k$ , set $H_t^i=H_{t-1}^i+G_t^{(i)}$ where $G_t^{(i)}$ is a $n_i\times n_i$ contraction, and $\tilde{G}_t*=(H_t^i)^{-\frac{1}{2k}}$ . update $W_{t+1}=W_t-\eta \tilde{G}_t$ 
>**shampoo is steepest descent under the max spectral norm** 
>- $\Delta W= \arg\min_{\Delta W_1\dots \Delta W_L}\sum_{\ell} \langle G_{\ell}, \Delta W_{\ell}\rangle +\lambda/2 \max_{\ell} ||\Delta W_{\ell}||_{\ell_2\rightarrow \ell_2}^2$ 
>- solved when $\eta=1/\lambda\sum \text{trace}  \Sigma_{\ell}$ , $\Delta W_{\ell}=-\eta U_{\ell}V_{\ell}^T$ 
>- disabling preconditioner accumulation, we'd get $W_{t+1}=W_t-\eta (G_tG_t^T)^{-1/4}G_t(G_t^TG_t)^{-1/4}=W_t-\eta U_tV_t^T$  
>- can bound MSE loss $L(W)=\frac{1}{2n}\sum_{i=1}^n 1/d_{out}||y_i-Wx_i||_2$  by $L(W+\Delta W)\le L(W) + \langle \nabla L(W), \nabla W\rangle + \frac{d_{in}}{2d_{out}}||\Delta W||_{\ell_2\rightarrow \ell_2}^2$ 

>**Prodigy**
>$m_t=\beta_1 m_{t-1}+(1-\beta_1)\eta_tg_t$ 
>$v_t=\beta_2v_{t-1}+(1-\beta_2)\eta_t^2g_t^2$ 
>$r_t=\sqrt{\beta_2}r_{t-1}+(1-\sqrt{\beta_2})\eta_t^2g_t^T(w_0-w_t)$
>$s_t=\sqrt{\beta_2}s_{t-1}+(1-\sqrt{\beta_2})\eta_t^2g_t$
>$\eta_{t+1}=\max(\eta_t, \frac{r_t}{||s_t||_1})$ 
>$w_{t+1}=w_t-\eta_tm_t/\sqrt{v_t}$ 
>Removing EMA gives
>$\eta_{t+1}=\max(\eta_t, \frac{g_t^T(w_0-w_t)}{||g_t||_1})$
>$w_{t+1}=w_t-\eta_t\text{sign}(g_t)$ 
>claim there exists an escape velocity $\eta_*$ , and $\eta_{t+1}$ roughly doubles if we haven't reached there

>**modular optimization**
>the **dual norm** and **duality map** is $||g||^{\dagger}=\max_{t\in\mathbb{R}^n,||t||=1}g^Tt$  and $\text{dualize}_{||\cdot ||}g=\arg\max_{t\in\mathbb{R}^n,||t||=1}g^T t$ 
>**induced operator norm** $||M||_{\alpha\rightarrow\beta}=\max_{x\in\mathbb{R}^{d_{in}}} \frac{||Mx||_{\beta}}{||x||_{\alpha}}$ 
>**optimal update** is $\arg\min_{\Delta w\in \mathbb{R}^n}g^T\Delta w + \lambda /2 ||\Delta w||^2=-\frac{||g||^{\dagger}}{\lambda}\text{dualize}_{||\cdot ||}g$  , and as long as $L(w+\Delta w)$ lower bounds this, the update will reduce loss
>**modular norm** of the entire neural network is $||(w_1\dots w_L)||_{W}=\max(s_1||w_1||_{W_1}\dots)$ , and normalization works as you'd expect and leads to better gradient updates with stable hyperparameters
>**steepest descent under modular norm** $\arg\min_{\Delta W_1,\dots } \sum \langle G_{\ell}, \Delta W_{\ell}\rangle +\lambda/2 \max s_{\ell}^2 ||\Delta W_{\ell}||_{\ell}^2$ 
>- $\eta=1/\lambda \sum_{k} 1/s_k ||G_k||_k^{\dagger}$ , then $\Delta W_{\ell}=-\frac{\eta}{s_{\ell}}\arg\max_{||T_\ell||_{\ell}=1}\langle G_{\ell},T_{\ell}\rangle$ 
>RMS norm is $||v||_{RMS}=||v||_2/\sqrt{d}$  and is nice because we still have $||\Delta W x||_{RMS}\le ||\Delta W||_{RMS-RMS}\cdot ||x||_{RMS}$ 
>Modular norm - given input vector space $X$, $Y$, and $W$ 
>- function M.forward $W\times X\rightarrow Y$ 
>- M.mass $\ge 0$ set proportion of feature learning that module contributes to supermodule
>- M.sensitivity $\ge 0$ estimates module's sensitivity to input perturbations
>- M.norm $W\rightarrow \mathbb{R}_{\ge 0}$ a norm over weight space
>**well-normed module** satisfies $||\nabla_w \text{M.forward}(w, x)\cdot \nabla w||_{Y}\le \text{M.norm}(\nabla w)$ and $||\nabla_x \text{M.forward}(w, x)\cdot \nabla x||_Y \le \text{M.sensitivity}||\nabla x||_X$ 
>- composing in sequence, masses add, sensitivities multiply, norms some other formula
>- same input space, output space concatenated, masses add, sensitivities add, norms some formula
>**Linear module** has sensitivity 1, mass $\mu$ and uses norm $||W||_{RMS\rightarrow RMS}$ , so the dualize of a gradient $G$ is $\sqrt{\frac{d_{out}}{d_{in}}}\cdot UV^T$ 
>**Embed module** has norm $||W||_{\ell_1\rightarrow RMS}$ and dualize maps each column $j$ of $G$ to $\frac{col_j(G)}{||col_j(G)||_{RMS}}$ 
>We can compute $UV^T$ quickly using Newton-Schulz iteration that does $X_0=G/||G||_F$ and $X_{t+1}=1.5X_t-0.5X_tX_t^TX_t$ 

>**intrinsic norm** $||v||_x=\sqrt{\langle v, \nabla^2 f(x)v\rangle}$ is invariant to transformations $x'=Ax$ 
>**strong non-degenerate self-concordance** is:
>- $\Omega$ is open and convex. Hessian $f$ positive definite everywhere. $W(x)=\{y\in \mathbb{R}^n | ||y-x||_x^2<1\}\subset \Omega \forall x\in\Omega$ 
>- inside $W(x)$ , $(1-||y-x||_x)||v||_x\le ||v||_y\le  ||v||_x /(1-||y-x||_x)$ 
>- alternatively, **barrier property** - f diverges on boundary + **differential inequality** - the restriction $\varphi(\gamma)$ of $f$ to some direction satisfies $\varphi'''(\gamma)\le 2\varphi''(\gamma)^{3/2}$
>- $c^tx-\sum_{i=1}^m\log(a_i^Tx-b_i)$ is self-concordant on the polytope
>- if $f$ is self-concordant and lower-bounded, it attains a unique minimum
>- **Newton decrement** is intrinsic norm of the descent direction $n(x)=-(\nabla^2 f(x))^{-1}\nabla f(x)$ 
>- if a point $||n(x)||_x\le 1/9$ , there exists a minimum $z$ with $||z-x||_x \le 3\cdot ||n(x)||_x$ 
>- if a point $x_t\in\Omega$ with $||n(x_t)||_{x_t}<1$ then $||n(x_{t+1})||_{x_{t+1}}\le \left( \frac{||n(x_t)||_{x_t}}{1-||n(x_t)||_{x_t}} \right)^2$ 

>**interior point**
>**central path** $\pi(\gamma)=\arg\min_x \gamma\langle c, x\rangle +f(x)\quad x\in\Omega$ and $\Omega$ is closure of open convex set, $f$ is strong nondegenerate self-concordance with **complexity parameter** $\theta_f=\sup_{x\in\Omega}||n(x)||_x^2$ and if it's finite it's called a **barrier function**
>- **optimality gap bound** - $\langle c, \pi(\gamma)\rangle \le \min_{x\in \overline{\Omega}} \langle c, x\rangle + 1/\gamma \theta_f$ 
>- **approximate optimality gap bound** - if $||x-\pi(\gamma)||_x \le 1/6$ , $\langle c, x \rangle \le \min_{x\in \overline{\Omega}} \langle c, x\rangle + 6/(5\gamma)\cdot \theta_f$ 
>- **short step barrier method** $\gamma_{t+1}=\beta\gamma_t$ and $x_{t+1}=x_t-(\nabla^2f(x_t))^{-1}(\gamma_{t+1}c+\nabla f(x_t))$ 
>- let $n_{\gamma}(x)=-(\nabla^2 f(x))^{-1}(\gamma c+\nabla f(x))$
>- if $||n_{\gamma_t}(x_t)||_{x_t}\le \frac{1}{9}$, $\gamma_{t+1}=(1+\frac{1}{8\sqrt{\theta_f}})$ , $||n_{\gamma_{t+1}}(x_{t+1})||_{x_{t+1}}\le 1/9$ 
>generically, we can divide $n_{\gamma_{t+1}}(x_t)$ into two terms, one containing $n_{\gamma_t}(x_t)$ and the other containing $n_0(x_t)$ , and get $||n_{\gamma_{t+1}}(x_t)||_{x_t}\le 1/4$ . then apply the quadratic convergence
>- finding a point $\zeta=\pi(0)$ to initialize as $x_1$ is fine; then $||n_{\gamma}(\zeta)||_{\zeta}=\gamma\cdot ||(\nabla^2 f(\zeta))^{-1}c||_{\zeta}$ so we choose $\gamma_1=1/(9||(\nabla^2 f(\zeta))^{-1}c||_{\zeta})$
>- we show $\gamma_1$ is pretty large by showing $||(\nabla^2 f(\zeta))^{-1}c||_{\zeta}\le \langle c,\zeta\rangle-\min_{x\in\overline{\Omega}}\langle c,x\rangle$ 
>consider $x(\lambda)=\zeta-\lambda\cdot ||(\nabla^2 f(\zeta))^{-1}c||_{\zeta}^2$ and choose $\lambda$ to be largest possible so that it's still inside $\Omega$ . We do so by noting that if $||\zeta-x(\lambda)||_{\zeta}<1$ , then $x(\lambda)\in\Omega$ 
>- we approximate $\zeta$ by solving $\pi'(\nu)=\arg\min_{x}-\nu\langle \nabla f(x'), x\rangle + f(x)$ with $\nu\rightarrow 0$ 

>**gaussian process**
>$m(x)$ is $\mathbb{E} f(x)$ and $K(x,y)$ is covariance between $f(x)$ and $f(y)$ . 
>**RBF** is $K(x,y)=k\cdot \exp(-\frac{||x-y||_2^2}{2\sigma^2})$ and is **universal** meaning any continuous function can be approximated
>if $f(x_1)\dots f(x_t)$ are observed values, posterior is
>$\mu(x)=m(x)+ K(x,x_i) (K(x_i, x_j))^{-1} (f-m)$ 
>$\sigma^2(x)=K(x,x)-K(x,x_i)(K(x_i,x_j))^{-1}K(x,x_i)$ 
>**Acquisition functions** (for minimization) are **Expected improvemenet** $EI(x)=\mathbb{E}_{\tilde{f}}\left[ \max(0, f_* - \tilde{f}(x)) \right]$ where $f_*$ is min value so far observed, or **LCB** $LCB(x)=\mu(x)-\beta\sigma(x)$ 
### Geometry
>**Range queries**
>$\log^d{n}$ query time, $n\log^{d-1}n$ space, $n\log^d n$ construction for finding statistics about $d$-dimensional points in rectangles
>fractional cascading reduces by $\log n$ 

>**Chan's algorithm** solves in $n\log h$ where $h$ is number of points on convex hull. It divides $n$ points into groups of size $m$, computes each group in $m\log m$ with vertical sweepline, and then does rotational sweepline. It increase $m$ from $1$ until hits $h$ 

>**Duality** maps points to halfspaces, so intersections of halfspaces corresponds to convex hull

>**Voronoi diagram** can be constructed by projecting $x,y\rightarrow x,y,x^2+y^2$ and doing 3D convex hull in $n\log n$ 
>we do a horizontal sweepline moving down, and maintain sorted active points which form the frontier of parabolas
>parabolas appear when new points are added, called site events, and parabolas disappear when when three consecutive parabolas intersect at some point, called circle events
>we maintain sorted list of events, initialize with site events
>Has a linear number of edges and vertices and $n$ faces by Euler's formula
>Dual forms delauney triangulation, which maximizes the minimum angle
### Memory
>M total memory, block size B, problem size N
>matmuls can be done by splitting NxN into $(N/\sqrt{M})^2$ blocks and multiplying 

