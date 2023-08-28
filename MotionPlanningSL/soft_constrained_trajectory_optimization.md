# Soft Constrained Trajectory Optimization
## Distance-based Trajectory Optimization
### Motivation
![](../Resource/soft_constrained_trajectory_optimization_img_1.png)
For a vision-based drone:
+ Limited sensing range and quality
+ Noisy depth estimation
Hard-constrained method:
+ Treat all free space equally
+ Solution space is sensitive to noise

### Problem Formulation

**Piecewise Polynomial Trajectory**
$$
f_\mu(t)=\left\{\begin{array}{cc}
\sum_{j=0}^N p_{1 j}\left(t-T_0\right)^j & T_0 \leq t \leq T_1 \\
\sum_{j=0}^N p_{2 j}\left(t-T_1\right)^j & T_0 \leq t \leq T_1 \\
\vdots & \vdots \\
\sum_{j=0}^N p_{M j}\left(t-T_{M-1}\right)^j & T_0 \leq t \leq T_1
\end{array}\right.
$$

**Objective Function**
$$
\begin{aligned}
 J&=J_s+J_c+J_d \\
& =\lambda_1 J_1+\lambda_2 J_2+\lambda_3 J_3
\end{aligned}
$$
where $J_s$ is smooth cost, $J_c$ is collision cost, $J_d$ is dynamic cost.

**Smoothness Cost**: minimum snap formulation 
$$
\begin{aligned}
J_s & =\sum_{\mu \in\{x, y, z\}} \int_0^T\left(\frac{d^k f_\mu(t)}{d t^k}\right)^2 d t \\
& =\left[\begin{array}{l}\boldsymbol{d}_F \\\boldsymbol{d}_P\end{array}\right]^T \boldsymbol{C}^T \boldsymbol{M}^{-T} \boldsymbol{Q} \boldsymbol{M}^{-1} \boldsymbol{C}\left[\begin{array}{l}\boldsymbol{d}_F \\
\boldsymbol{d}_P\end{array}\right]=\left[\begin{array}{l}\boldsymbol{d}_F \\
\boldsymbol{d}_P\end{array}\right]^T\left[\begin{array}{ll}\boldsymbol{R}_{F F} & \boldsymbol{R}_{F P} \\
\boldsymbol{R}_{P F} & \boldsymbol{R}_{P P}\end{array}\right]\left[\begin{array}{l}\boldsymbol{d}_F \\
\boldsymbol{d}_P\end{array}\right]
\end{aligned}
$$
To solve the nonlinear optimization problem, the gradient is needed.  
The Jacobian with respect to free derivatives $\boldsymbol{d}_{p\mu}$ is
$$
\frac{\alpha J_s}{\alpha \boldsymbol{d}_{p \mu}}=2 \boldsymbol{d}_F^T \boldsymbol{R}_{F P}+2 \boldsymbol{d}_P^T \boldsymbol{R}_{P P}
$$

**Collision Cost**: penalize on the distance to nearest obstacle 
$$
\begin{aligned}
J_c & =\int_{T_0}^{T_M} c(p(t)) d s \\
& =\sum_{k=0}^{T / \delta t} c\left(p\left(T_k\right)\right)\|v(t)\| \delta t, T_k=T_0+k \delta t
\end{aligned}
$$
Here we should integrate with respect to the curve $s$ instead of $t$.  
With respect to $s$, the trajectory far away to the obstacle is preferred.  
With respect to $t$, 

The Jacobian with respect to free derivatives $\boldsymbol{d}_{p\mu}$ is
$$
\frac{\alpha J_c}{\alpha \boldsymbol{d}_{p \mu}}=\sum_{k=0}^{T / \delta t}\left\{\forall_\mu c\left(p\left(T_k\right)\right)\|v\| \boldsymbol{F}+c\left(p\left(T_k\right)\right) \frac{v_\mu}{\|v\|} \boldsymbol{G}\right\} \delta t, \mu \in\{x, y, z\}
$$
$L_{dp}$ is the right block of matrix $\boldsymbol{M}^{-1}\boldsymbol{C}$ which corresponds to the free derivatives on the $\mu$ axis of the $\boldsymbol{d}_{p\mu}$, then
$$
F=TL_{dp}\qquad G=TV_{m}L_{dp}
$$
$\forall_\mu c(\cdot)$ is the gradient in $\mu$ axis of the collision cost.

$V_m$ maps the coefficients of the position to the coefficients of the velocity.

$T=[T^0_k,T^1_k,\dots,T^n_k]$

The Hessian matrix
$$
\begin{aligned}
& \mathbf{H}_o=\left[\frac{\partial^2 f_o}{\partial \mathbf{d}_{P_x}^2}, \frac{\partial^2 f_o}{\partial \mathbf{d}_{P_y}^2}, \frac{\partial^2 f_o}{\partial \mathbf{d}_{P_z}^2}\right] \\
& \frac{\partial^2 f_o}{\partial \mathbf{d}_{P \mu}^2}=\sum_{k=0}^{\tau / \delta t}\left\{\mathbf { F } ^ { T } \nabla _ { \mu } c \left(p\left(\mathcal{T}_k\right) \frac{v_\mu}{\|v\|} \mathbf{G}+\mathbf{F}^T \nabla_\mu^2 c\left(p\left(\mathcal{T}_k\right)\right)\|v\| \mathbf{F}\right.\right. \\
& \left.+\mathbf{G}^T \nabla_\mu c\left(p\left(\mathcal{T}_k\right)\right) \frac{v_\mu}{\|v\|} \mathbf{F}+\mathbf{G}^T c\left(p\left(\mathcal{T}_k\right)\right) \frac{v_\mu^2}{\|v\|^3} \mathbf{G}\right\} \delta t
\end{aligned}
$$

**Dynamic Cost**: penalize on the velocity and acceleration where exceeds limits (similar to collision term)