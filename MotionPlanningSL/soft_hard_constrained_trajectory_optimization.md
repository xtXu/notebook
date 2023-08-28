# Soft and Hard Constrained Trajectory Optimization
**Minimum snap**: good for smooth, not for collision avoidance
+ only constrain the intermediate waypoints
+ computational cheap and easy to implenmentation
+ no constraints on the trajectory itself
+ "overshoot" unavoidable

**Minimum snap with safety constraints:**
+ Adding forces
+ Adding bounds
![](../Resource/soft_hard_constrained_trajectory_optimization_img_1.png)

## hard/soft constraints

**hard constraints:** The constraints are required to be strictly satisfied
$$
\begin{array}{ll}
\min & f(x) \\
\text { s.t. } & g_i(x)=c_i, \quad i=1, \cdots, n \quad \text { Equality constraints } \\
& h_j(x) \geq d_j, \quad j=1, \cdots, n \quad \text { Inequality constraints }
\end{array}
$$

**soft constraints:**
$$
\min  f(x)+\lambda_1\cdot g(x)+\lambda_2\cdot h(x)
$$
+ Penalty terms / loss functions
+ Constraints which are preferred but not strictly required
+ Various kind of loss function
![](../Resource/soft_hard_constrained_trajectory_optimization_img_2.png)


## Corridor-based Trajectory Optimization
![](../Resource/soft_hard_constrained_trajectory_optimization_img_3.png)
+ Cost function: $J=\sum_{\mu \in\{x, y, z\}} \int_0^T\left(\frac{d^k f_\mu(t)}{d t^k}\right)^2 d t$
+ Boundary constraints
+ Continiuty constraints
+ Safety constraints
