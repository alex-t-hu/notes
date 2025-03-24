### Radial wavefunction of hydrogen
$\frac{1}{\sin \theta} \frac{d}{d\theta} \sin \theta \frac{d\psi}{d\theta} + \frac{1}{ \sin^2 \theta} \frac{d^2}{d^2\phi} = -\frac{L^2}{\hbar^2}$ 
$-\frac{\hbar^2}{2m_e} \frac{1}{r} \frac{d^2}{dr^2} (r\psi) + \frac{L^2}{2m_er^2}\psi+V(r)\psi = E\psi$
Solving eigenvalue equations for $\psi_{lm}$, we get
$Y_{lm} \varpropto e^{im\phi}P_l^m(\cos(\theta))$
$L_z Y_{lm} = \hbar m Y_{lm}$
$L^2 Y_{lm} = \hbar^2 \ell(\ell+1) Y_{lm}$
Now we can write
$\psi(r,\theta,\phi) = R_{E\ell}(r)Y_{lm}(\theta,\phi)$
Defining $(u_{E\ell}(r)\equiv rR_{E\ell}(r))$, we get $-\frac{\hbar^2}{2m} \frac{d^2}{dr^2} (u_{E\ell}) + \left(\frac{\hbar^2\ell(\ell+1)}{2m_er^2}+V(r)\right)u_{E\ell} = E u_{E\ell}$
$m$ doesn't appear, but $\ell$ does. We get $u_{E\ell} \sim cr^{\ell+1}$ as $r\rightarrow 0$.
### Perturbation theory
$E_{n}^{(k)} = \bra{n^{(0)}}\delta H \ket{n^{(k-1)}}$
$\ket{n^{(1)}}=-\sum\limits_{k\neq n} \frac{\delta H_{kn}}{E_k^{(0)} - E_n^{(0)}}\ket{k^{(0)}}$
Good basis diagonalizes $\left[\delta H\right]$, but not necessarily eigenvectors of $\delta H$.

### Hydrogen
$\hat{H} = \hat{H}^{(0)} - \frac{\hat{p}^4}{8m^3c^2} + \frac{1}{2m^2c^2}\frac{1}{r} \frac{dV}{dr}\hat{S}\cdot \hat{L} + \frac{\hbar^2}{8m^2c^2}\nabla^2V$
For relativistic correction, we use the schrodinger equation to transform calculation into computing 
expectation of $\frac{1}{r}$, $\frac{1}{r^2}$. Also we find that both coupled and uncoupled are good basis, since corrections only depend on $n$ and $l$.

For Darwin correction, where we view electron as ball of radius $\frac{\hbar}{mc}$, we get it laplacian
is delta function, so only for $\ell=0$ does it not vanish.

For spin-orbit, we use $J^2 = L^2 + S^2 + 2LS$. Coupled basis is good, since $L\cdot S$ commuting with $L^2$ and $J^2$ and $J_z$.
This follows from $L^2$ commutes with $L_i$ and $S_i$, and $J_z$ and $J^2$. 
$$E_{n\ell j m_j ; \text{f.s.}}^{(1)} = -\alpha^4mc^2 \frac{1}{2n^4}\left(\frac{n}{j+\frac{1}{2}}-\frac{3}{4}\right)$$

Magnetic field of a dipole is $\vec{B} = \frac{\mu_0}{4\pi} \left( \frac{3}{r^3} \vec{r} \times \vec{p} - \frac{1}{r^2} \vec{p} \right)$

Hyperfine splitting occurs
$H_{HFS} = \frac{g_eg_pe^2}{4m_em_pc^2}\left(\frac{8\pi}{3}S_e \cdot S_p \delta^3(r) + \frac{3(S_p \cdot r)(S_e \cdot r) - S_p\cdot S_e}{r^3}\right)$

Zeeman effect $\delta H_z = \frac{eB}{2mc} (L_z + 2S_z)$
Coupled basis states. Say we want to transition $(n, \ell, j, m_j)$ to $(n, \ell, m_{\ell}, m_s)$.

### WKB
$\psi(x,t) = \exp\left(\frac{i S}{\hbar}\right)$
$J(x,t) = \rho \frac{\nabla \Re S(x,t)}{m}$
$S'(x)^2-i\hbar S''(x)=p^2(x)$
$S(x) = S_0 + \hbar S_1 + \dots$
$S_0 = \pm \int\limits_{x_0}^x p(x')dx'$
$S_1 = \frac{i}{2}\log p(x) + C$
Probability current is constant, as this is a time independent solution.
$|\frac{d\lambda}{dx}| \ll 1$ 
Near turning points, this is impossible, but we can solve approximately another way and stitch together solutions.

**WKB Approximation**
    For $V(x) > E$,
    $\psi(x) \simeq  \frac{A}{\sqrt{\kappa(x)}} \exp\left( -\int\limits_{x_0}^xdx'\kappa(x')\right) + \frac{B}{\sqrt{\kappa(x)}} \exp\left( \int\limits_{x_0}^xdx'\kappa(x')\right)$
    $\psi(x) \simeq  \frac{C}{\sqrt{k(x)}} \cos\left(\int\limits_{x_0}^xdx'k(x') - \frac{\pi}{4}\right) + \frac{D}{\sqrt{k(x)}} \sin\left( \int\limits_{x_0}^xdx'k(x') - \frac{\pi}{4}\right)$

**Airy Approximation**
    Set $V(x) - E = g(x-a)$ for turning point $a$. Then set $u=(\frac{2mg}{\hbar^2})^{\frac{1}{3}}(x-a)$. Then 
    $\frac{d^2\psi}{du^2}= u\psi(u)$ is the Airy equation with two solution $Ai(u)$ and $Bi(u)$.

**Intersecting Region**
    $V(x)-E = g(x-a) + \frac{1}{2}V''(a)(x-a)^2$ should be $g(x-a)$. We get
    $u\ll$ something and $u\gg 1$ for Airy asymptotics to exist. This is possible when assuming $\hbar$ is small.
    So, we do have a linear region of potential where Airy approximation is valid. We match coefficients to get relationship between 
    $A, B, C, D$.
**Boundary conditions**
    When $V(x)>E$ on the right, we get 
    $\frac{2}{\sqrt{k(x)}} \cos\left(\int\limits_{x}^{a} k(x')dx' - \frac{\pi}{4}\right) \Leftarrow \frac{1}{\sqrt{\kappa(x)}} \exp\left( -\int\limits_{a}^xdx'\kappa(x')\right)$
    $-\frac{1}{\sqrt{k(x)}} \sin\left(\int\limits_{x}^{a} k(x')dx' - \frac{\pi}{4}\right) \Rightarrow \frac{1}{\sqrt{\kappa(x)}} \exp\left( \int\limits_{a}^xdx'\kappa(x')\right)$
    Otherwise, we get 
    $\frac{1}{\sqrt{\kappa(x)}} \exp\left( -\int\limits_{x}^adx'\kappa(x')\right) \Rightarrow \frac{2}{\sqrt{k(x)}} \cos\left(\int\limits_{a}^{x} k(x')dx' - \frac{\pi}{4}\right)$
    $-\frac{1}{\sqrt{\kappa(x)}} \exp\left( \int\limits_{x}^adx'\kappa(x')\right) \Leftarrow \frac{1}{\sqrt{k(x)}} \sin\left(\int\limits_{a}^{x} k(x')dx' - \frac{\pi}{4}\right)$

A particle is bouncing back and forth. We can approximate the the time it takes to go from $a$ to $c$ and back with integrating, 
and we calculate transmission coefficient by taking ratio of square amplitudes, which gives $e^{2\theta}$ where $\theta$ is some integral of $k(x)$ from $a$ to $b$.
Adding a $\frac{-i\Gamma}{2}$ term to the Hamiltonian gives exponential damping to the probability.

### Gauge Invariance
$B = \nabla \times A\quad E = -\nabla \phi - \frac{1}{c}\frac{\partial A}{\partial t}\quad\nabla \times E = -\frac{1}{c} \frac{\partial B}{\partial t}$ 
Changing $A$ by $\nabla \Lambda$ and $\phi$ by $-\frac{1}{c}\frac{\partial \Lambda}{\partial t}$ is a gauge transformation.
There's pairs of $\phi,A$ giving same EM fields not related by gauge transformation, and also $E,B$ satisfying Maxwell's equations but can't be written in terms of gauge potentials.
$H=\frac{1}{2m}\left(p - \frac{q}{c}A(x,t)\right)^2+q\phi$
It's more natural to work with $v = \frac{p}{m} - \frac{qA}{mc}$, and then we get current density $J=\Re(\psi^*v\psi)$ satisfying continuity equation.
$\psi'= \exp\left(\frac{iq\Lambda}{\hbar c}\right)\psi$ solves the new Schrodinger equation upon gauge transformation.

Gauge-covariant observable $O$ is Hermitian and $O'=O\left[A',\phi'\right]=UO\left[A,\phi\right]U^{-1}$.
$O=p-\frac{q}{c}A$ is gauge-covariant. 

Sort of like Heisenberg formalism, where we have
$\frac{dA_H}{dt}=\frac{i}{\hbar}\left[H_H(t),A_H(t)\right] + \frac{\partial A_s}{\partial t}_H$
Heisenberg equation of motion for $x$ gives $m\dot{x_H}=p_H-\frac{q}{c}A(x_H,t)$.
Computing Heisenberg equation of motion for $v$ gives
$m\dot{v_H} = qE_H + \frac{q}{2}\left(\frac{v_H}{c}\times B_H - B_H \times \frac{v_H}{c}\right)$

### Time-dependent perturbation theory

Interaction picture wave function $\ket{\tilde{\psi}(t)} = e^{iH^{(0)}t/\hbar}\ket{\psi(t)}$
$i\hbar \frac{\delta}{\delta t} \ket{\tilde{\psi}(t)} = \tilde{\delta H}(t)\ket{\tilde{\psi}(t)}$
We expand $\ket{\tilde{\psi}(t)}$ in terms of basis states of $H^{(0)}$, the time independent part, and get
$i\hbar \dot{c_m} = \sum_n e^{i\omega_{mn}t}\delta H_{mn}(t)c_n(t)$
for example, Nuclear magnetic resonance. $H=\omega \cdot s$ where $\omega$ is the Larmor frequency and $s$ is the spin operator. $\mu = \gamma S$, $H = -(\gamma B) \cdot S$, a spin state rotates about the same axis as the spin with frequency $-\gamma B$.
$\delta H(t)=\Omega (S_x \cos(\omega_0 t) + S_y \sin (\omega_0 t))$ where $\omega_0$ is the Larmor frequency.
Solving this in the the natural frame, we get $x$ rotation as well as z rotation.
$\ket{\tilde{\psi}(t)} = \ket{\tilde{\psi}^{(0)}(t)}+\lambda \cdots$
We can define $\ket{\tilde{\psi}^{(k)}(t)} = \sum_n c_{n}^{(k)}(t)\ket{n}$