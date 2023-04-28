# Modified Damped Newton's Method

#math #optimization #newton

## Newton's Method

Newton's method approximates the original function by a second-order Taylor expansion at $\boldsymbol{x}$,
$$
f(\boldsymbol{x}) \approx \hat{f}(\boldsymbol{x}) \triangleq f\left(\boldsymbol{x}_k\right)+\nabla f\left(\boldsymbol{x}_k\right)^T\left(\boldsymbol{x}-\boldsymbol{x}_k\right)+\frac{1}{2}\left(\boldsymbol{x}-\boldsymbol{x}_k\right)^T \nabla^2 f\left(\boldsymbol{x}_k\right)\left(\boldsymbol{x}-\boldsymbol{x}_k\right)
$$
Minimizing quadratic approximation,
$$
\begin{aligned}
& \nabla \hat{f}(\boldsymbol{x})=\nabla^2 f\left(\boldsymbol{x}_k\right)\left(\boldsymbol{x}-\boldsymbol{x}_k\right)+\nabla f\left(\boldsymbol{x}_k\right)=\mathbf{0} \\
& \Longrightarrow \boldsymbol{x}=\boldsymbol{x}_k-\left[\nabla^2 f\left(\boldsymbol{x}_k\right)\right]^{-1} \nabla f\left(\boldsymbol{x}_k\right)
\end{aligned}
$$
where the Hessian should be **Positive Definite (PD)**: $\nabla^2 f\left(\boldsymbol{x}_k\right) \succ \boldsymbol{O}$.
So the **Newton Step** is 
$$
\boldsymbol{x}_{k+1}=\boldsymbol{x}_k-\left[\nabla^2 f\left(\boldsymbol{x}_k\right)\right]^{-1} \nabla f\left(\boldsymbol{x}_k\right)
$$
![](../Resources/damped_newton_method_img_1.png)There are 3 aspects to evaluate a numerical optimization method:
+ Convergence speed
+ Stability
+ Computation work per iteration

**For Newton's method, there are much fewer iterations, but each iteration is more expensive.**

**Drawbacks:** Hessian can be **singular** and **indefinite**.
![](../Resources/damped_newton_method_img_3.png)

## Practical Newton's Method

The common pipeline of the Newton's method:
$$
\begin{align}
&\text{initialize}\quad\boldsymbol{x}\leftarrow\boldsymbol{x}_{0}\in\mathbb{R}_{n} \\
&\text{while}\quad \Vert\nabla f(\boldsymbol{x})\Vert>\delta\quad\text{do} \\
&\qquad \boldsymbol{d}\leftarrow-\boldsymbol{M}^{-1}\nabla f(\boldsymbol{x}) \\
&\qquad t\leftarrow\text{backtracking line search} \\
&\qquad \boldsymbol{x}\leftarrow \boldsymbol{x}+t\boldsymbol{d}\\
&\text{end while} \\
&\text{return}
&&
\end{align}
$$
**To obtain $\boldsymbol{d}$, solving $\boldsymbol{Md}=\nabla f(\boldsymbol{x})$ is more efficient than solve$\boldsymbol{M}^{-1}$, as**
$$
\left[\nabla^2 f(\boldsymbol{x})\right] \boldsymbol{d}=-\nabla f(\boldsymbol{x})
$$
However, it is bad conditioned when Hessian is positive semi-definite (PSD) or indefinite. To make sure the Hessian is PD, we can choose a $\boldsymbol{M}$ that is close to Hessian.
+ If the function is **convex**, the Hessian must be PSD, then$$\boldsymbol{M}=\nabla^2 f(\boldsymbol{x})+\epsilon \boldsymbol{I}, \epsilon=\min \left(1,\|\nabla f(\boldsymbol{x})\|_{\infty}\right) / 10$$ $\boldsymbol{M}$ is PD, since the $\nabla^2 f(\boldsymbol{x})$ is PSD, then $\forall \boldsymbol{x}\neq 0$, $\boldsymbol{x}^\top \nabla^2 f(\boldsymbol{x}) \boldsymbol{x}\geq 0$, $$\boldsymbol{x}^\top\boldsymbol{M}\boldsymbol{x}=\boldsymbol{x}^\top(\nabla^2 f(\boldsymbol{x})+\epsilon\boldsymbol{I})\boldsymbol{x}=\boldsymbol{x}^\top \nabla^2 f(\boldsymbol{x}) \boldsymbol{x}+\boldsymbol{x}^\top \epsilon\boldsymbol{I}\boldsymbol{x}> 0$$ Then the search direction is solved by Cholesky factorization$$\boldsymbol{M} \boldsymbol{d}=-\nabla f(\boldsymbol{x}), \boldsymbol{M}=\boldsymbol{L} \boldsymbol{L}^{\mathrm{T}}$$
+ If the function is **nonconvex**, the Hessian is indefinite. We compute $\boldsymbol{M}$ through Bunch-Kaufman Factorization$$\boldsymbol{M d}=-\nabla f(\boldsymbol{x}), \boldsymbol{M}=\boldsymbol{L} \boldsymbol{B} \boldsymbol{L}^{\mathrm{T}}$$ where $\boldsymbol{B}$ is block diagonal matrix $[b_1, b_2, \cdots, b_n]$ with block size $1\times 1$ and $2\times 2$. $b_i \in R^+ \geq 0$, or $b_i\in R^{2\times 2}$ has one negative and one positive eigenvalue.  