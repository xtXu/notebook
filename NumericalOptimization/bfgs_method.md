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
where $M$ should satisfy:
+ [x] not need full 2nd-order derivatives
+ [x] linear equations have closed-form solutions
+ [x] lightweight and compact to store
+ [ ] **preserve descent directions**
+ [x] **contaion curvature info (local quadratic approx.)**

To preserve the descent directions, $M$ must be PD.
We can proof that **BFGS update preserves PD if $\Delta g^\top\Delta x > 0$**, and for **strict convex** function, $\langle y-x,\nabla f(y)-\nabla f(x)\rangle >0\Rightarrow \Delta g^\top\Delta x > 0$ .
So, the BFGS for the strict convex funtion can be
![|550](../Resources/bfgs_method_img_1.png)


