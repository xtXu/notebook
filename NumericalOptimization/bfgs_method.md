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

To **preserve the descent directions**, $B=M^{-1}$ must be PD.  
We can prove that **BFGS update preserves PD if**
$$
\Delta g^\top\Delta x > 0
$$
The proof is in the appendix at the end of the article.  

## For Strict Convex Smooth Function
For **strict convex** function, 
$$
\langle y-x,\nabla f(y)-\nabla f(x)\rangle >0\Rightarrow \Delta g^\top\Delta x > 0
$$
which means the $d$ can preserve the descent direction if the function is strict convex.  
The BFGS for the **strict convex funtion** can be
$$
\begin{aligned}
&\textbf{initialize}~~ x^{0},g^{0}\leftarrow f(x^{0}),\,B^{0}\leftarrow I,\,k\leftarrow 0\\
&\textbf{while}~~ \Vert g^{k}\Vert>\delta\quad\textbf{do}\\
&\qquad d\leftarrow -B^{k}g^{k}\\
&\qquad t\leftarrow \text{backtracking line serach (Armijo)}\\
&\qquad x^{k+1}\leftarrow x^{k}+td\\
&\qquad g^{k+1}\leftarrow \nabla f(x^{k+1})\\
&\qquad B^{k+1}\leftarrow \text{BFGS}(B^{k},g^{k+1}-g^{k},x^{k+1}-x^{k})\\
&\qquad k\leftarrow k+1\\
&\textbf{end while}\\
&\textbf{return}
\end{aligned}
$$

## For Non-Convex Smooth Function
For **non strict convex function**, we can use **Wolfe Condition** in line search.
$$
\text{Wolfe}\rightarrow \Delta g^{T}\Delta x>0\rightarrow B\text{ is PD}\rightarrow d\text{ is descent direction}
$$
### Wolfe Condition
#### weak wolfe condition
Given parameters $0<c_{1}<c_{2}<1$, typically $c_{1}=10^{-4},c_{2}=0.9$, the **weak wolfe condition** can be formulated as
$$
\begin{cases}
f\left(x^k\right)-f\left(x^k+\alpha d\right) \geq-c_{1} \cdot \alpha d^{\mathrm{T}} \nabla f\left(x^k\right) \\\\
d^{\mathrm{T}}\nabla f(x^{k}+\alpha d) \geq c_{2}\cdot d^{\mathrm{T}}\nabla f(x^{k})
\end{cases}
$$
The first condition is the sufficient decrease condition, which is the same as Armijo.  
The second condition is the **curvature condition**. The $d^\mathrm{T}\nabla f(x^k)$ is the derivative of $\phi(\alpha)=f(x^{k}+\alpha d)$ at $\alpha=0$. The $d$ is a descent direction, so $\phi'(\alpha)<0$ before the first minima, and $c_{2}\cdot d^{\mathrm{T}}\nabla f(x^{k})>d^{\mathrm{T}}\nabla f(x^{k})$. As $f(x^{k}+\alpha d)$ move near the local minima, $\phi'(\alpha)$ increase and  move near $0$.  Thus, the condition means that we want **the derivatives to increase sufficiently**, which **can prevent the slow progress** and make $x^{k+1}$ near to the local minima. 
![|500](../Resources/bfgs_method_img_2.png)
As shown in the figure, the top red lines is the derivatives allowed by the curvature condition. Note that if the descent step exceeds the local minima, the derivative become positive, which can also prevent the slow progress and also meets the curvature condition.  
****
#### strong wolfe condition
$$
\begin{cases}
f\left(x^k\right)-f\left(x^k+\alpha d\right) \geq-c_{1} \cdot \alpha d^{\mathrm{T}} \nabla f\left(x^k\right) \\\\
\Vert d^{\mathrm{T}}\nabla f(x^{k}+\alpha d)\Vert \geq c_{2}\cdot \Vert d^{\mathrm{T}}\nabla f(x^{k})\Vert
\end{cases}
$$
The second condition is the **strong curvature condition**. It make $x^{k}+\alpha d$ near the local minima, and prevent one step over too far, as shown in the figure below.
![500](../Resources/bfgs_method_img_3.png)
The strong wolfe condition can suppress the oscillation.

We can summarize that
$$
\text{Strong Wolfe}\rightarrow \text{Weak Wolfe}\rightarrow \Delta g^{T}\Delta x>0\rightarrow B\text{ is PD}\rightarrow \text{descent}
$$
In practical, **weak wolfe** is used more often to keep the robust.
### Cautious Update
Wolfe condition **cannot guarantee the convergence** of BFGS in some cases.  
So the **cautious update** is introduced:
$$
B^{k+1}=
\begin{cases}
&\left(I-\frac{\Delta x \Delta g^T}{\Delta g^T \Delta x}\right) B^k\left(I-\frac{\Delta g \Delta x^T}{\Delta g^T \Delta x}\right)+\frac{\Delta x \Delta x^T}{\Delta g^{T} \Delta x}\quad &\text{if}\,\Delta g^T\Delta x>\epsilon\Vert g_k\Vert\Delta x^T\Delta x,\epsilon=10^{-6}\\
&B^k &\text{otherwise}
\end{cases}
$$
Then the convergence can be guaranteed if
+ the function has bounded sub-level set;
+ the function has Lipschitz continuous gradient.

In summary, the **BFGS** for the **possibly non-convex function** can be:
$$
\begin{aligned}
&\textbf{initialize}~~ x^{0},g^{0}\leftarrow f(x^{0}),\,B^{0}\leftarrow I,\,k\leftarrow 0\\
&\textbf{while}~~ \Vert g^{k}\Vert>\delta\quad\textbf{do}\\
&\qquad d\leftarrow -B^{k}g^{k}\\
&\qquad t\leftarrow \text{inexact line serach (Wolfe)}\\
&\qquad x^{k+1}\leftarrow x^{k}+td\\
&\qquad g^{k+1}\leftarrow \nabla f(x^{k+1})\\
&\qquad B^{k+1}\leftarrow \text{cautious-BFGS}(B^{k},g^{k+1}-g^{k},x^{k+1}-x^{k})\\
&\qquad k\leftarrow k+1\\
&\textbf{end while}\\
&\textbf{return}
\end{aligned}
$$

In most cases, BFGS is robustness enough, so cautious-BFGS is not necessary.  
In many libraries, BFGS is applied easily without cautious update.  
The cost per iteration: $O(n^2)$

## Limited-memory BFGS (L-BFGS)
**Motivation:**
+ For BFGS method, $B^k$ contains all information of $\Delta x_i, \Delta g_i, (i=1,\dots,k-1)$. However, the information before many iterations is not useful, and $B^k$ can be dense and the rank can become too large.
+ We want to reduce $O(n^2)$ cost per iteration, but after too many iterations.

**Only exploit last m+1 pairs of $\{x^k, g^k\}$.**

**Limited-memory BFGS:**
$$
s^k=\Delta x^{k+1}=x^{k+1}-x^k,y^k=\Delta g^{k+1}=g^{k+1}-g^k, \rho^k=\frac{1}{\langle s^k,y^k\rangle}
$$
Instead of store $B^k$ explicitly, we store up to $\mathrm{m}$ values of $s^k,y^k,\rho^k$.  
Then in every iteration, we can obtain $B^k$ as:
$$
\begin{aligned}
&\textbf{for}~~ i=k-m,k-m+1,\dots,k\\
&\qquad B^{i+1}\leftarrow\text{BFGS}(B^i, g^{i+1}-g^i,x^{i+1}-x^i)\\
&\textbf{end}
\end{aligned}
$$
However, the cost to calculate $B^k$ is $O(mn^2)$.

Instead, we can use the algorithm below, whose result is same:
$$
\begin{aligned}
&d^k\leftarrow g^k\\
&\textbf{for}~~ i=k-1,k-2,\dots,k-m\\
&\qquad \alpha^i\leftarrow\rho^i\langle s^i,d\rangle\\
&\qquad d\leftarrow d-\alpha^i y^i\\
&\textbf{end}\\
&\gamma\leftarrow\rho^{k-1}\langle y^{k-1},y^{k-1}\rangle\\
&d\leftarrow d/\gamma\\
&\textbf{for}~~ i=k-m,k-m+1,\dots,k-1\\
&\qquad \beta\leftarrow\rho^i\langle y^i,d\rangle\\
&\qquad d\leftarrow d+s^i(\alpha^i-\beta)\\
&\textbf{end}\\
&\textbf{return search direction}~d
\end{aligned}
$$
![](../Resources/bfgs_method_img_4.png)

|  | Newtons | BFGS | L-BFGS |
| :---: | :---: | :---: | :---: |
| Work per iter | $O(n^3)$ | $O(n^2)$ | $O(mn)$ |

**L-BFGS is almost the 1st choice for efficient smooth nonconvex optimization.**  
To guarantee the convergence, we can store the $s^k,y^k,\rho^k$ only when meeting the Li-Fukushima condition (i.e. Cautious Update).

## For Non-Smooth Function
Trouble with nonsmoothness:
+ Gradient may not exist
+ Negative sub-grad does not descent
+ Curvature can be vary large

**When applying strong wolfe condition to nonsmooth function,** it may be unable to meet the strong curvature condition, since there is no $\alpha$ where the gradient is close to 0.
![](../Resources/bfgs_method_img_5.png)
Therefore, **we should use weak wolfe condition.**

Generally for smooth function, we use interpolation to search the step size that meets the weak wolfe conditions. 

For nonsmooth function, we use **Lewis & Overton line search** to obtain the feasible step size. In weak wolfe condition, let
$$
\begin{cases}
S(\alpha)=f\left(x^k\right)-f\left(x^k+\alpha d\right) \geq-c_{1} \cdot \alpha d^{\mathrm{T}} \nabla f\left(x^k\right) \\\\
C(\alpha)=d^{\mathrm{T}}\nabla f(x^{k}+\alpha d) \geq c_{2}\cdot d^{\mathrm{T}}\nabla f(x^{k})
\end{cases}
$$
**Lewis & Overton line search:**
$$
\begin
$$




## Appendix
BFGS update preserves PD if $\Delta g^T\Delta x>0$.    
**Proof:** It is equivalent to prove that if $B^k\succ 0$ and $\Delta g^T\Delta x>0$, then $B^{k+1}\succ 0$.   
For $\forall y$, 
$$
\begin{aligned}
y^T B^{k+1}y&=y^T\left[\left(I-\frac{\Delta x \Delta g^T}{\Delta g^T \Delta x}\right) B^k\left(I-\frac{\Delta g \Delta x^T}{\Delta g^T \Delta x}\right)+\frac{\Delta x \Delta x^T}{\Delta g^T \Delta x}\right]y\\
&=y^T \left(I-\frac{\Delta x \Delta g^T}{\Delta g^T \Delta x}\right) B^k\left(I-\frac{\Delta g \Delta x^T}{\Delta g^T \Delta x}\right)y+y^T \frac{\Delta x \Delta x^T}{\Delta g^T \Delta x}y
\end{aligned}
$$
Firstly consider the right part:
$$
\text{Right}=y^T \frac{\Delta x \Delta x^T}{\Delta g^T \Delta x}y=\frac{y^T\Delta x \Delta x^T y}{\Delta g^T \Delta x}=\frac{(\Delta x^T y)^2(\geq 0)}{\Delta g^T \Delta x(>0)}\geq 0
$$
Consider the left part:
$$
\text{Left}=y^T \left(I-\frac{\Delta x \Delta g^T}{\Delta g^T \Delta x}\right) B^k\left(I-\frac{\Delta g \Delta x^T}{\Delta g^T \Delta x}\right)y
$$
Let $A=I-\frac{\Delta g \Delta x^T}{\Delta g^T \Delta x}$, then
$$
\text{Left}=y^T A^T B^k A y=(Ay)^T B^k Ay
$$
Because $B^k \succ 0$ and $Ay\in\mathbb{R}^n$, 
$$
\text{Left}=(Ay)^T B^k Ay\geq 0
$$
So $B^{k+1}=\text{Left}+\text{Right}\geq0$, i.e. $B^{k+1}$ is PSD.  
Then let's consider whether $B^{k+1}$ is PD.  We discuss this in three cases.  
**First:** $y=0$, then $\text{Left}=0,\text{Right}=0\Rightarrow y^T B^{k+1} y=0$.  
**Second:** $y\neq0,Ay\neq0$, then $\text{Left}>0\Rightarrow y^T B^{k+1} y>0$.  
**Third:** $y\neq0, Ay=0$, then $\text{Left}=0$, 
$$
Ay=\left(I-\frac{\Delta g \Delta x^T}{\Delta g^T \Delta x}\right)y=0\Rightarrow y=\frac{\Delta g \Delta x^T}{\Delta g^T \Delta x}y=\frac{\Delta g(\Delta x^T y)}{\Delta g^T \Delta x}
$$
Because $y\neq0\Rightarrow \Delta x^T y\neq0$,
$$
\begin{aligned}
\text{Right}&=y^T \frac{\Delta x \Delta x^T}{\Delta g^T \Delta x}y=\frac{(y^T \Delta x) (\Delta x^T y)}{\Delta g^T \Delta x}
=\frac{(\Delta x^T y)^2(>0)}{\Delta g^T \Delta x}>0
\end{aligned}
$$
Thus, $\text{Right}>0\Rightarrow y^T B^{k+1}y>0$.

In summary, for $\forall y\neq0$, $y^T B^{k+1}y>0$, $B^{k+1}\succ 0$.  
Thus, given $B^0=I\succ 0$, we can preserve PD if $\Delta g^T\Delta x>0$.

