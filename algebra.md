>**universal property**: given sets $M$ and $N$ a product is $P$ with maps $\mu: P\rightarrow M$ and $\nu: P\rightarrow N$ , for any set $P'$ with maps $\mu': P'\rightarrow M$ and $\nu': P' \rightarrow N$ , there is a unique map $f: P' \rightarrow P$ with $\mu' = \mu \circ f$ and $\nu'=\nu\circ f$ 
>**category**: set of objects, and for each pair of objects $A$ and $B$ , a set of morphisms $M(A, B)$ that are **associative**. for each object there is an **identity morphism**: $id_A: A\rightarrow A$ 
>examples are sets, vector spaces, groups
>**groupoid**: generalizes group to have more than 1 identity element. every morphism has an inverse. $\Pi_1(X)$ is fundamental groupoid of topological space where objects are points in $X$ and morphisms are homotopy classes of paths from $x$ to $y$ 
>**$A$-modules** (vector space generalization) form category; objects are all $A$-modules and morphisms are homomorphisms
>**Topological Spaces** : objects are topological spaces and morphisms are continuous functions
>**Open sets**: there is a morphism $U\rightarrow V$ iff $U\subset V$ 
>**Posets**: single morphism btwn $x$ and $y$ iff $x\ge y$ 
>**covariant functor** $F$ maps category $A$ to $B$, where for each morphism $m: A_1 \rightarrow A_2$ , $F(m): F(A_1)\rightarrow F(A_2)$ and $F(id_A)=id_{F(A)}$ and $F(m_2\circ m_1)=F(m_2)\circ F(m_1)$ . **forgetful functor** like $A$-module to abelian group
>**zariski topology** $\mathbb{A}^n$  over field $k$, closed sets are **algebraic varieties**, $V(f_1,\dots f_m) = \{(a_1,\dots a_n)\in\mathbb{A}^n|f_i(a_1,\dots a_n)=0\}$ - fewer open sets 

> **cohomology groups** $H^

>**colimit** 

>**double complex** collection of $A$-modules $E^{p,q}$ (p,q ints) and morphisms $d_{\rightarrow}^{p,q}:E^{p,q}\rightarrow E^{p+1,q}$ and $d_{\uparrow}^{p,q}:E^{p,q}\rightarrow E^{p,q+1}$  up and right's square to 0 and up and right commute/anti-commute
>$E^k$ is diagonal sum, and $d=d_{\rightarrow}+d_{\uparrow}$  
>**spectral sequence** is $\rightarrow E_i^{p,q}$  along with $d_r^{p,q}:  E_r^{p,q} \rightarrow E_r^{p-r+1,q+r}$ with $d_r^{p,q}\circ d_r^{p+r-1,q-r}=0$ ; isomorphism of **cohomology** 

>the **ring** of differentiable functions on $X=\mathbb{R}^n$  on some open set $U$ is $\mathcal{O}(U$)
>**germs** are objects of the form $\{(f, \text{open } U) : p\in U, f\in \mathcal{O}(U) \}$ , where if for some open set $W\subset U,V$ with $f|_W=g|_W$ then $(f,U)\sim (g, V)$ . the set of germs is **stalk** $\mathcal{O}_p$ 
>$O_p$ is **local ring** (commutative, unique maximal ideal) because maximal ideal $m_p$ is all germs vanishing at $p$ . $m_p/m_p^2$ is a module over $\frac{\mathcal{O}_p}{m_p}= \mathbb{R}$
> **presheaf** $\mathscr{F}$ on topological space $X$ has $\mathscr{F}(U)=\Gamma(U,\mathscr{F})=H^0(U,\mathscr{F})$ , a set containing sections of $\mathscr{F}$ over $U$ , and for each inclusion $U \hookrightarrow V$, **restriction map**  $\text{res}_{V,U}:\mathscr{F}(V)\rightarrow \mathscr{F}(U)$ with $\text{res}_{U,U}=id_{\mathscr{F}(U)}$ and if $U\hookrightarrow V \hookrightarrow W$ then the restriction maps commute
> 


algebraic subset $V(S)=\{a_1,\dots a_n \in k^n | f(a_1,\dots a_n) = 0 \forall f \in S\}$ 
