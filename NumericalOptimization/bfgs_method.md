# BFGS Method

#math #optimization #quasi-newton #bfgs

From the  [quasi_newton_method](quasi_newton_method.md), we want $B$ that satisfy
$$
\Delta x \approx B^{k+1}\Delta g, M^{k+1}B^{k+1}=I
$$
To choose a better $B$, an optimization problem is formulated
$$
\begin{array}{c}
\min _B \left\| H^\frac{1}{2}\left(B - B^k\right)H^\frac{1}{2}\right\|^2 \\
\begin{aligned}
\text { s.t. }  B&=B^{\mathrm{T}} \\
\Delta x &=B \Delta g
\end{aligned}\\
H=\int_0^1 \nabla^2 f\left[(1-\tau) x^k+\tau x^{k+1}\right] d \tau
\end{array}
$$
## BFGS update

Recover the curvature (approx. inv. Hessian) from gradients
$$
\begin{aligned}
& B^{k+1}=\left(I-\frac{\Delta x \Delta g^T}{\Delta g^T \Delta x}\right) B^k\left(I-\frac{\Delta g \Delta x^T}{\Delta g^T \Delta x}\right)+\frac{\Delta x \Delta x^T}{\Delta g^T \Delta x} \\
& B^0=I, \Delta x=x^{k+1}-x^k, \Delta g=\nabla f\left(x^{k+1}\right)-\nabla f\left(x^k\right)
\end{aligned}
$$
Then $M=B^{-1}$ can satisfy:
+ [x] not need full 2nd-order derivatives
+ [x] linear equations have closed-form solutions
+ [x] lightweight and compact to store
+ [ ] **preserve descent directions**
+ [x] **contaion curvature info (local quadratic approx.)**

To preserve the descent directions, $M$ must be PD.  
We can prove that **BFGS update preserves PD if**
$$
\Delta g^\top\Delta x > 0
$$
The proof is in the appendix at the end of the article.  

For **strict convex** function, 
$$
\langle y-x,\nabla f(y)-\nabla f(x)\rangle >0\Rightarrow \Delta g^\top\Delta x > 0
$$
which means the $d$ can preserve the descent direction if the function is strict convex.  
The BFGS for the **strict convex funtion** can be
$$
\begin{aligned}
&\text{initialize}\quad x^{0},g^{0}\leftarrow f(x^{0}),\,B^{0}\leftarrow I,\,k\leftarrow 0\\
&\text{while}\quad \Vert g^{k}\Vert>\delta\quad\text{do}\\
&\qquad d\leftarrow -B^{k}g^{k}\\
&\qquad t\leftarrow \text{backtracking line serach (Armijo)}\\
&\qquad x^{k+1}\leftarrow x^{k}+td\\
&\qquad g^{k+1}\leftarrow \nabla f(x^{k+1})\\
&\qquad B^{k+1}\leftarrow \text{BFGS}(B^{k},g^{k+1}-g^{k},x^{k+1}-x^{k})\\
&\qquad k\leftarrow k+1\\
&\text{end while}\\
&\text{return}
\end{aligned}
$$

How to preserve the descent direction if the function is **not strict convex** ?  
Use **Wolfe Condition** in line search.
$$
\text{Wolfe}\rightarrow \Delta g^{T}\Delta x>0\rightarrow B\text{ is PD}\rightarrow d\text{ is descent direction}
$$
### Wolfe Condition
#### weak wolfe condition
Given parameters $0<c_{1}<c_{2}<1$, typically $c_{1}=10^{-4},c_{2}=0.9$, the **weak wolfe condition** can be formulated as
$$
\begin{cases}
f\left(x^k\right)-f\left(x^k+\alpha d\right) \geq-c_{1} \cdot \alpha d^{\mathrm{T}} \nabla f\left(x^k\right) \\
d^{\mathrm{T}}\nabla f(x^{k}+\alpha d) \geq 
\end{cases}
$$












In summary, the **BFGS** for the **possibly non-convex function** can be:
$$
\begin{aligned}
&\text{initialize}\quad x^{0},g^{0}\leftarrow f(x^{0}),\,B^{0}\leftarrow I,\,k\leftarrow 0\\
&\text{while}\quad \Vert g^{k}\Vert>\delta\quad\text{do}\\
&\qquad d\leftarrow -B^{k}g^{k}\\
&\qquad t\leftarrow \text{inexact line serach (Wolfe)}\\
&\qquad x^{k+1}\leftarrow x^{k}+td\\
&\qquad g^{k+1}\leftarrow \nabla f(x^{k+1})\\
&\qquad B^{k+1}\leftarrow \text{cautious-BFGS}(B^{k},g^{k+1}-g^{k},x^{k+1}-x^{k})\\
&\qquad k\leftarrow k+1\\
&\text{end while}\\
&\text{return}
\end{aligned}
$$


