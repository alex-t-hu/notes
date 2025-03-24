>**CUDA**
>block-level scheduling, so need to wait for all warps to have enough registers and smem
>Matmul 
>- Each block computes some block of the output matrix
>- loop along the K direction, and can optimize smem load using 4-wide and transposition
>- Each warp computes an independent tile of the output block, called the **warp tile**
>- Each thread most generally computes some grid of rectangles of the warp tile
>- cutlass uses warp mma fragments to do this fast, for H100 is 16x16x16
>- HGEMM (fp16), SGEMM (fp32),  IGEMM (integer), DGEMM (fp64), WGEMM (mixed)

>**MCTS**
>

>**Formal reasoning**
>Learning Formal Mathematics From Intrinsic Motivation
>- compared to games, theorem proving requires intrinsic rewards when conjectures are proven
>- can train a LLM that is both a conjecturer, generating statements to prove, a policy, and a value function
>- conjecturing uses constrained decoding to output conjectures following the language
>- when the LLM is proving with MCTS, we can extract true conjectures along the way with hindsight relabeling
>- value function is trained to be 1 if along proof path and 0 otherwise
>- conjecture difficulty measured by log-likelihood and we aim to make it increase
>- how is decoding performed with the value function?

>**Redteaming**
>- goal for attack $g=(instruction, criteria)\sim A_{G}$, example attack $x_g$ , and rule-based reward $x_r$. Try to increase diversity of generated attacks using cosine similarity, except we orthogonalize by goal similarity

>**Architecture**
>- DeepSeek MLA (multi-head latent attention). combine key and value into single latent
>- DeepSeek MOE - segment experts into finer granularity for higher specialization and more accurate knowledge acquisition, outperforms GShard

>**SSM**
 $\dot{x}(t)=Ax+Bu, y(t)=Cx+Du$
 $x_k=\overline{A}x_{k-1}+\overline{B}u_k,y_k=\overline{C}x_k$ 
 $y=\overline{CA^iB}*u$ 
 "Liquid version"
 $x_k=(\overline{A}+\overline{B}u_k)x_{k-1}+\overline{B}u_k,y_k=\overline{C}x_k$
 >Toeplitz: $A_{ij}=a_{i-j}$ Cauchy: $A_{ij}=1/(x_i-y_j)$ Vandermonde: $A_{ij}=x_i^j$ 

>**Reinforcement Learning**
>$R(\tau)=\sum_{t=0}^{\infty}\gamma^t r_t$ or $R(\tau)=\sum_{t=0}^{T}r_t$ 
>On-Policy value function $V^{\pi}(s)=\mathbb{E}_{\tau\sim\pi}\left[R(\tau)| s_0=s \right]$ 
>On-Policy action-value function $Q^{\pi}(s,a)=\mathbb{E}_{\tau\in\pi}\left[R(\tau)|s_0=s,a_0=a \right]$ 
>Optimal Value function $V^*(s)=\max_{\pi} \mathbb{E}_{\tau\in\pi} \left[ R(\tau)|s_0=s \right]$ 
>Optimal Action-Value Function $Q^*(s,a)=\max_{\pi} \mathbb{E}_{\tau\sim\pi}\left[ R(\tau)|s_0=s,a_0=a \right]$ 
>Advantage Function $A^{\pi}(s,a)=Q^{\pi}(s,a)-V^{\pi}(s)$ 
>Maximize $J(\pi_{\theta})=\mathbb{E}_{\tau\sim\pi_{\theta}} \left[ R(\tau) \right]$ 
>- Model-free RL
>	- Policy Optimization
>		- Policy Gradient
> $\nabla J(\pi_{\theta})= \mathbb{E}_{\tau\sim \pi_{\theta}} \left[ \sum_{t=0}^T \nabla \log \pi_{\theta}(a_t|s_t)\Phi_t \right]$
> where $\Phi_t=R(\tau), \sum_{t'=t}^T R(s_{t'},a_{t'},s_{t'+1})-b(s_t),Q^{\pi_{\theta}}(s_t,a_t),A^{\pi_{\theta}}(s_t,a_t)$
> $\phi_{k+1}=\arg\min_{\phi} 1/(|D_k|T)\sum_{\tau\in D_k} \sum_{t=0}^T (V_{\phi_k}(s_t)- \hat{R_t})^2$ 
> Two approximations for Advantage: $A^{\pi}(s_t,a_t)=\sum_{t'=t}^T \gamma^{t'-t}r_{t'} = r_t+\gamma V^{\pi}(s_{t+1})-V^{\pi}(s_t)$ that can be approximated like $A_t^{GAE}=\sum_{k=0}^{\infty}(\gamma\lambda)^k \delta_{t+k}$ where $0\le\lambda\le 1$ is a bias-variance tradeoff
>		- A2C/A3C
>		- PPO
> $\theta_{k+1}=\arg\max_{\theta}\mathbb{E}_{s,a\sim \pi_{\theta_k}} \left[ L(s,a,\theta_k,\theta) \right]$ and $L(s,a,\theta_k,\theta)=\min\left( \frac{\pi_{\theta}(a|s)}{\pi_{\theta_k}(a|s)}A^{\pi_{\theta_k}}(s,a), \text{clip}( \frac{\pi_{\theta}(a|s)}{\pi_{\theta_k}(a|s)} ,1-\epsilon,1+\epsilon) A^{\pi_{\theta_k}}(s,a) \right)$ , early stopping if mean KL-divergence too large
> alternatively, $L(s,a,\theta_k,\theta)=  \frac{\pi_{\theta}(a|s)}{\pi_{\theta_k}(a|s)}A^{\pi_{\theta_k}}(s,a)-\beta \text{KL}\left[ \pi_{\theta_{k}}(\cdot | s), \pi_{\theta}(\cdot | s) \right]$  (seems to be flipped between Spinning up and paper formulas), and if $\mathbb{E}\left[ KL \right]< \frac{d_{targ}}{1.5}$ $\beta\leftarrow \beta/2$ etc...
>		- TRPO
> $\theta_{k+1}=\arg\max_{\theta} L(\theta_k,\theta)= \mathbb{E}_{s,a\sim \pi_{\theta_k}} \left[ \frac{\pi_{\theta}(a|s)}{\pi_{\theta_k}(a|s)}A^{\pi_{\theta_k}}(s,a) \right]$ 
> such that $\overline{D}_{KL}(\theta||\theta_k)= \mathbb{E}_{s\sim \pi_{\theta_k}} \left[ D_{KL}(\pi_{\theta}(\cdot|s)|| \pi_{\theta_k}(\cdot | s)) \right] \le \delta$ 
> in practice, $L(\theta_k, \theta)=g^T(\theta-\theta_k)$ and $\overline{D}_{KL}(\theta||\theta_k)=\frac{1}{2}(\theta-\theta_k)^TH(\theta-\theta_k)$ where $g$ is the policy gradient
> $\theta_{k+1}=\theta_k+\alpha^j\sqrt{\frac{2\delta}{g^T H^{-1} g }}H^{-1}g$ since when $j=0$, might not satisfy KL constraint or improve surrogate advantage
> $H$ is large so we solve for $x=H^{-1}g$ by computing $Hx=\nabla_{\theta}((\nabla_{\theta}\overline{D}_{KL}(\theta||\theta_k))^Tx)$ and using conjugate gradient algorithm
>	- Q-Learning
>		- DQN
>		- C51
>		- QR-DQN
>		- HER
>	- Both
>		- Deep Deterministic Policy Gradient
>observe $a=clip(\mu_{\theta}(s)+clip(\epsilon,-c,c),a_{Low},a_{High})$ where $\epsilon$ is normal (at beginning can sample randomly)
>execute $a$ to get $s', r, d$ and store $(s,a,r,s',d)$ in $D$ 
>if it's time to update, randomly sample batch $B$ from $D$, compute $y(r,s',d)=r+\gamma(1-d)Q_{\phi_{targ}}(s',\mu_{\theta_{targ}}(s'))$ 
>update Q-function one step of GD using $\sum_{(s,a,r,s',d)\in B} (Q_{\phi}(s,a)-y(r,s',d))^2$ 
>update policy with $\sum_{s\in B} Q_{\phi}(s,\mu_{\theta}(s))$ 
>update $\phi_{targ}=\rho\phi_{targ}+(1-\rho)\phi$ and $\theta_{targ}=\rho\theta_{targ}+(1-\rho)\theta$ 
>		- TD3 (Twin Delayed DDPG)
>same as above except there's two parameters $\phi_1$ and $\phi_2$ with $y(r,s',d)=r+\gamma(1-d)\min_{i=1,2}Q_{\phi_{targ,i}}(s',a'(s'))$ and updating $\theta$ uses $\phi_1$ 
>$a'(s')=clip(\mu_{\theta_{targ}}(s')+clip(\epsilon,-c,c),a_{Low},a_{High})$ 
>there is a policy delay where we update $\phi_{targ,i},\theta_{targ},\theta$ less frequently than $\phi_i$ 
>		- Soft Actor Critic
>learns a stochastic policy with entropy reward and no-need for target policy
>$\pi^*=\arg\max_{\pi}\mathbb{E}_{\tau\sim\pi}\left[ \sum_{t=0}^{\infty} \gamma^t(R(s_t,a_t,s_{t+1}+\alpha H(\pi(\cdot | s_t))) \right]$ 
>$V^{\pi}(s)=\mathbb{E}_{a\sim \pi}\left[ Q^{\pi}(s,a) \right]+\alpha H(\pi(\cdot|s'))$ 
>observe $s$ and select $a\sim\pi_{\theta}(\cdot | s)$ and execute and put in buffer
>when it's time to update
>- compute targets for current batch, which are now stochastic $y(r, s', d)=r+\gamma(1-d)\left( \min_{i=1,2}Q_{\phi_{targ,i}}(s',\tilde{a}') - \alpha\log \pi(\tilde{a}'|s') \right)$ , $\tilde{a}'\sim \pi_{\theta}(\cdot | s')$ 
>- update Q-functions $\phi_1,\phi_2$ 
>- Update policy with reparametrization trick so $\tilde{a}_{\theta}(s)\sim \pi_{\theta}(\cdot | s)$ is differentiable with $\nabla_{\theta} 1/|B| \sum_{s\in B}\left(\min_{i=1,2}Q_{\phi_i}(s,\tilde{a}_{\theta}(s))-\alpha\log\pi_{\theta}(\tilde{a}_{\theta}(s)|s)\right)$  
>- Update target networks with $\phi_{targ,i}=\rho\phi_{targ,i}+(1-\rho)\phi_i$ 
>- Model-based RL
>	- Learn the Model
>		- World Models
>		- I2A
>		- MBMF
>		- MBVE
>	- Given the Model
>		- AlphaZero
