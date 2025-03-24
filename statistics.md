### Bayesian inference
$\min\limits_{f} \max\limits_{p\in \left[0,1\right]} \varphi(p, f) = \max\limits_{p\in \left[0,1\right]}\min\limits_{f} \varphi(p, f)$ 
this is called a Bayesian decision rule
### Non-Bayesian Inference
Sometimes it's unnatural to treat latent variables of interest as random, and instead to treat them as deterministic.
In this case, our observation model becomes parametrized, not conditioned, on the latent variable.
$b_{\hat{x}}(x) = \mathbb{E}(\hat{x}(y) - x)$
$\Lambda_e(x) = \mathbb{E}\left[(e(y)-b_{\hat{x}}(x))(e(y)-b_{\hat{x}}(x))^T\right]$
Minimum variance estimator is an unbiased and admissible (not depending on $x$) estimator satisfying
$\hat{x}_{MVU}(y) = \arg\min\limits_{\hat{x}} \Lambda_{\hat{x}}(x)$
$J_y(x)$ is the Fisher information in $y$ about $x$. Score function is vector $S(y;x) = \frac{d}{dx}\ln(p_y(y;x))$
$J_y(x) = \mathbb{E}\left[S(y;x)S(y;x)^T\right]$
$J_y(x) = -\mathbb{E} \frac{d^2}{dx^2}\ln(p_y(y;x))$
If $\int \ln p_y(y;x) = 0$, for any unbiased $\hat{x}$, $\Lambda_{\hat{x}}(x) \ge \frac{1}{J_y(x)}$
An unbiased estimator is efficient if it satisfies the Cramer Rao bound with equality.
Maximum likelihood estimator $\hat{x}_{ML}$ is efficient when efficient estimator exists
There's also Cramer-Rao type bounds for random $x$ (Bayesian) and biased $x$.
### Exponential families
$p_{y}(\cdot;x) = \exp\left(\lambda(x)t(y)-\alpha(x)+\beta(y) \right)$
$\lambda$, $t$, $\beta$ are known as the natural parameter, natural statistic, and log base function.
$Z(x) = \exp\left(\alpha(x)\right)$ is the partition function.
$p_y(y;x) \propto q(y)\exp{\lambda(x)t(y)}$, $q(y)$ is base distribution
$\dot{\alpha}(x) = \dot{\lambda}(x)\mathbb{E}(t(y))$
$\ddot{\alpha}(x) = \ddot{\lambda}(x)\mathbb{E}(t(y)) + (\dot{\lambda}(x))^2\text{Var}(t(y))$
$J_y(x) = \mathbb{E} \left(\frac{d}{dx}\ln(p_y(y;x))\right)^2 = \dot{\lambda}(x)\frac{d}{dx}\mathbb{E} t(y)$
Regular exponential families have support independent of $x$. Linear exponential families have $\lambda(x)=x$. Canonical have also that $t(y)=y$.
$p_y(y;x) = \exp\left(x\ln\frac{p_1(y)}{p_2(y)}+\ln p_2(y) - \ln Z(x)\right)$
Any linear exponential family is a geometric mean of two probability distributions $p_1(y)$ and $p_2(y)$.
    $p_y(y;x) = \frac{q(y)e^{xy}}{Z(x)}$ 
An efficient estimator exists for estimating $x$ from $y$ iff $p_y(y;x)$ is an exponential family 
and $\lambda(x) = cJ_y(x)$ for some constant $c$. Further, if it exists, we get $x_{eff}(y) = ct(y)+b$.
$\ln(p_y(y;x)) = \sum_{i=1}^k \lambda_i(x)t_i()$
Linear means $\lambda = x$. Full means $\text{rank}(\frac{d\lambda}{dx})=L=k$. Curved means rank is $L$ and less than $k$. Overparametrized is rank is less than $L$ and $L<k$.

### Sufficient statistics
$t(\cdot)$ is a sufficient statistic wrt $\left\{p_y(\cdot;x)| x\in X\right\}$ if 
$p_{y|T}(y|t(y);x)=\frac{p_y(y;x)}{p_T(t(y);x)}$ is not a function of $x$ for all $y\in Y$.
$t(y)$ is sufficient if $p_{y|T}(y|t(y);x)$ is not a function of $x$ for all $y\in Y$.
$t(\cdot)$ is sufficient iff exist $a,b$ such that 
$p_y(y;x) = a(t(y),x)b(y)$
$S(\cdot)$ is minimal if for any other suff. statistic $t(\cdot)$, $S=g(T)$.
SS t is complete if for any $\phi : t(Y)\rightarrow R$ , $E \phi t(y)=0$ for all $x$ implies that $P(\phi(t(y))=0)=1$. Almost surely. If $t$ is complete, it is minimal.

Super cool. Consider $t$ to be complete sufficient statistic and $s$ to be a minimal sufficient statistic.
$E(t) =E(E(t|s))$. $E(t|s(y)=s')$ is actually a function of $t$, so $E(t|s(y)=s')-t$ is zero-mean over $x$. This means 
$E(t|s(y)=s')=t$ with probability 1, so $t$ is a function of $s$. 

Bayesian version: $p_{y|T,x}(y|t(y),x) = p_{y|T}(y|t(y))$
Belief characterization: $p_{x|y}(x|y) = p_{x|t}(x|t(y))$
**Neyman** **pearson**
$p_{y|x}(y|x) = p_{t|x}(t(y)|x)p_{y|t}(y|t(y))$ for all $x,y$.
Markov structure. Model $T = t(y)$. $X \leftrightarrow Y \leftrightarrow T$
if $T$ is sufficient, $X \leftrightarrow T \leftrightarrow Y$
A statistic is sufficient iff for any $y_1,y_2$ with $t(y_1)=t(y_2)$, $L_{y_1}(x)\propto L_{y_2}(x)$.
A sufficient statistic is minimal iff for any $y_1,y_2\in Y$ s.t. $L_{y_1}(x) \propto L_{y_2}(x)$, $t(y_1)=t(y_2)$.
These statements give a nice picture.
### EM algorithm
Iteratively approximate $\hat{x}(y)\equiv \text{argmax}_{x\in X} p_y(y;x)$
$U(x;\hat{x}^{(\ell)})=\mathbb{E}_{p_{z|y}(\cdot|y;\hat{x}^{(\ell)})}\left[\ln p_z(z;x)\right]$
$\hat{x}^{(\ell+1)} = \text{argmax}_{x\in X} U(x;\hat{x}^{(\ell)})$
### Information Theory
Generalized bayesian decision theory assigns probability distribution to predicted values of target variable.
Proper cost function satisfies, for all $y$, $p_{x|y}(\cdot | y) = \text{argmin}_{q} \mathbb{E}\left[C(x,q)|y=y\right]$
$I(x;y) = \mathbb{E}_{p_{x,y}}\left[\ln\frac{p_{x,y}(x,y)}{p_x(x)p_y(y)}\right]$ where $I(x;y)$ is the mutual information between $x$ and $y$*

$\mathbb{E}C(x,p_{x})= H$ where specifically what this $H$ is depends on how we're defining $p$.

Let $p^*$ be the projection of $q$ onto the convex set $P$. Then $D(p||q) \ge D(p^*||q) + D(p||p^*)$
Let $p^*\in \mathcal{P}$ be an arbitrary distribution. Then $p^*$ is an $I$-projection of a distribution
$q$ onto $L_t(p^*)\equiv \{ p | \mathbb{E}_p(t(y)) = \mathbb{E}_{p^*}(t(y))\}$ iff $q$ is in the set $\{p(y^*)\exp\left(x^Tt(y)-\alpha(x)\right) | x\in \mathbb{R}^k \}$.
### Modeling
Let $\{p_{y|x}(\cdot ; x)\}$ be a family of probability distributions. There exists a mixture model $q$ such that 
for all other models $p$,
$D(p_{y|x}(\cdot ; x) || q) \le D(p_{y|x}(\cdot ; x) || p)$ for all $x$.
We choose $q$ to minimize the max of KL divergences over each possible probability distribution.
$q = \text{argmin}_{q'} \max\limits_{x} D(p(\cdot ;x)||q')$
Redundacy capacity theorem says 
$\min\limits_{q} \max\limits_{w} \sum w(x) D(p(\cdot ; x) || q) = \max\limits_{w} \min\limits_{q} \sum w(x)  D(p(\cdot ; x) || q)$
This means that we can just minimize the max KL divergence over all priors $w$. 
Turns out we want our model to be a mixture with weights $w$, so we can actually simplify to get
$R^{-} = \max\limits_{p_x} I(x;y)$

### Asymptotics of Inference and Universality
Let $y^N=y_1,\dots y_N$ be generated iid and let |X| be finite. Let all $x\in X$ be identifiable and $p(y|x)$ always positive.
Then  $D(p(\cdot|x)||p_{y^N}) = - \log p_x(x) + o(1)$, $I(x;y^N) = H(p_x)$, $C_N = \log (|X|) + o(1)$

### Model order selection
We approximate $p(y|H) = \int p(y|x,H)p(x|H)dx$
Compute a partition function associated with $p(\cdot | y)$
$q(x) \propto p(x|y,H) = q(\hat{x})\exp\left(-\frac{1}{2}J_{y=y}(\hat{x})(x-\hat{x})^2\right)$
We plug this in to get
$p(y|H) = p(y|\hat{x},H)p(\hat{x}|H)\sqrt{2\pi J_{y=y}^{-1}(\hat{x})}$
Use for large $n$
$p(\cdot | y^N) = N(\hat{x}_n(y^N), \frac{1}{NJ_y(x)})$
$D(p_{y^N|x}||p_{y^N}) = \frac{1}{2}\log\frac{NJ_y(x)}{2\pi e}  - \log(p_x) = o(J)$
Universal inference
For iid source $y^N$ whose distribuiton depends on $1$ parameter, universal inference is possible if
channel capacity reaches end.