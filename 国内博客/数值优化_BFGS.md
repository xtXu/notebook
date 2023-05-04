# BFGS法
根据前文的拟牛顿法，我们希望求得矩阵 $B$ 来近似海森阵的逆，从而还原出海森阵中的曲率信息，满足
$$
\Delta x \approx B^{k+1}\Delta g, M^{k+1}B^{k+1}=I
$$
因为满足要求的矩阵有无数多个，因此通过构建优化问题，来求得一个最优的 $B$：
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
## BFGS更新

我们可通过**BFGS更新**公式来求得满足要求的矩阵 $B$：
$$
\begin{aligned}
& B^{k+1}=\left(I-\frac{\Delta x \Delta g^T}{\Delta g^T \Delta x}\right) B^k\left(I-\frac{\Delta g \Delta x^T}{\Delta g^T \Delta x}\right)+\frac{\Delta x \Delta x^T}{\Delta g^T \Delta x} \\
& B^0=I, \Delta x=x^{k+1}-x^k, \Delta g=\nabla f\left(x^{k+1}\right)-\nabla f\left(x^k\right)
\end{aligned}
$$
这样， $M=B^{-1}$ 即为海森阵的逆矩阵的近似，且满足:
+ [x] 无需求二阶导数
+ [x] 线性方程组有闭式解（求出 $B$ 后，即可直接求出 $d=-Bg$）
+ [x] 轻量，便于存储（$O(n^2)$）
+ [ ] **保证下降方向**
+ [x] **包含曲率信息**

为了**保证下降方向**，$B=M^{-1}$ 需正定。
我们可以证明：若满足
$$
\Delta g^\top\Delta x > 0
$$
则**BFGS更新可以保留 $B$ 的正定**。  
证明见文章最后。

## BFGS用于严格凸的光滑函数
对于**严格凸函数**，
$$
\langle y-x,\nabla f(y)-\nabla f(x)\rangle >0\Rightarrow \Delta g^\top\Delta x > 0
$$
因此一定能保证 $d$ 为下降方向。  
用于**严格凸函数**的BFGS算法如下：
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

## BFGS用于非凸函数
对于**非凸函数**，我们可以在**线搜索**中使用 **Wolfe Condition** 来保证 $\Delta g^\top\Delta x > 0$，从而保证下降方向：
$$
\text{Wolfe}\rightarrow \Delta g^{T}\Delta x>0\rightarrow B\text{ is PD}\rightarrow d\text{ 为下降方向}
$$
### Wolfe Condition
#### weak wolfe condition
给定参数 $0<c_{1}<c_{2}<1$, 通常取 $c_{1}=10^{-4},c_{2}=0.9$, **weak wolfe condition** 的表述如下：
$$
\begin{cases}
f\left(x^k\right)-f\left(x^k+\alpha d\right) \geq-c_{1} \cdot \alpha d^{\mathrm{T}} \nabla f\left(x^k\right) \\\\
d^{\mathrm{T}}\nabla f(x^{k}+\alpha d) \geq c_{2}\cdot d^{\mathrm{T}}\nabla f(x^{k})
\end{cases}
$$
其中，第一个条件为**充分下降条件**，与 Armijo 条件相同。  
第二个条件为**曲率条件**。$d^\mathrm{T}\nabla f(x^k)$ 为函数 $\phi(\alpha)=f(x^{k}+\alpha d)$ 在 $\alpha=0$ 时的导数，即对应切线的斜率。由于方向 $d$ 为下降方向，则在遇到第一个极小值前 $\phi'(\alpha)<0$，对于小于0的数乘上一个常数，则 $c_{2}\cdot d^{\mathrm{T}}\nabla f(x^{k})>d^{\mathrm{T}}\nabla f(x^{k})$。当 $f(x^{k}+\alpha d)$ 接近局部极小值时，$\phi'(\alpha)$ 变大并趋于0。因此，该条件保证了导数变化（增大）的足够多，从而防止 $x^k$ 到 $x^{k+1}$ 变化的太少，下降的太慢，同时也使得 $x^{k+1}$ 趋近于局部极小。
![|500](../Resources/bfgs_method_img_2.png)
如图所示，上方红色的直线表示曲率条件可以接受的导数（切线斜率）范围。当步长较大导致越过了局部极小值时，导数为正，此时也满足条件，因为此时的 $x^{k+1}$ 也可以防止变化过少，虽然对于这轮迭代来说，沿下降方向没有接近极小值，但在总体上来说，这一步迭代仍是有效的。
#### strong wolfe condition
$$
\begin{cases}
f\left(x^k\right)-f\left(x^k+\alpha d\right) \geq-c_{1} \cdot \alpha d^{\mathrm{T}} \nabla f\left(x^k\right) \\\\
\Vert d^{\mathrm{T}}\nabla f(x^{k}+\alpha d)\Vert \geq c_{2}\cdot \Vert d^{\mathrm{T}}\nabla f(x^{k})\Vert
\end{cases}
$$
其中第二个条件称为**强曲率条件**，它保证了 $x^{k}+\alpha d$ 在极小值附近，防止了步长太大，如图所示。
![500](../Resources/bfgs_method_img_3.png)
Strong wolfe condition可以抑制振荡。

综上，
$$
\text{Strong Wolfe}\rightarrow \text{Weak Wolfe}\rightarrow \Delta g^{T}\Delta x>0\rightarrow B\text{ is PD}\rightarrow \text{descent}
$$
在实际中，**weak wolfe**已经足够用来保证算法的鲁棒性了。

### Cautious Update
在一些情况下，wolfe condition 无法保证 BFGS 一定收敛。因此，可以通过 **cautious update** 来保证收敛：
$$
B^{k+1}=
\begin{cases}
&\left(I-\frac{\Delta x \Delta g^T}{\Delta g^T \Delta x}\right) B^k\left(I-\frac{\Delta g \Delta x^T}{\Delta g^T \Delta x}\right)+\frac{\Delta x \Delta x^T}{\Delta g^{T} \Delta x}\quad &\text{if}\,\Delta g^T\Delta x>\epsilon\Vert g_k\Vert\Delta x^T\Delta x,\epsilon=10^{-6}\\
&B^k &\text{otherwise}
\end{cases}
$$
上述更新 $B$ 的条件也叫做 Li-Fukushima 条件。  

综上，对于可能**非凸的函数**，BFGS算法如下：
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
在多数情况下，BFGS足够鲁棒，无需 cautious-BFGS。  
每次迭代的复杂度：$O(n^2)$
## Limited-memory BFGS (L-BFGS)
**动机:**
+ 在 BFGS 算法中，$B^k$ 包含了 $\Delta x_i, \Delta g_i, (i=1,\dots,k-1)$ 的所有信息，其中多次迭代前的信息对于当前迭代几乎没有作用。但随着迭代次数的增加，$B^k$ 会变得越来越稠密，秩越来越大，不利于计算。
+ 进一步降低单轮迭代所需的 $O(n^2)$ 复杂度。

因此，可以**只利用 m+1 对 $\{x^k, g^k\}$ 来还原当前点的曲率信息.**

**Limited-memory BFGS（有限内存的BFGS）:**
$$
s^k=\Delta x^{k+1}=x^{k+1}-x^k,y^k=\Delta g^{k+1}=g^{k+1}-g^k, \rho^k=\frac{1}{\langle s^k,y^k\rangle}
$$
我们不直接存储 $B^k$，而是存储 $\mathrm{m}$ 对
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
\begin{aligned}
&l\leftarrow0\\
&u\leftarrow+\infty\\
&\alpha\leftarrow1\\
&\textbf{repeat}\\
&\qquad \textbf{if}~~S(\alpha)~\text{fails}\\
&\qquad \qquad u\leftarrow \alpha\\
&\qquad \textbf{else if}~~C(\alpha)~\text{fails}\\
&\qquad \qquad l\leftarrow \alpha\\
&\qquad \textbf{else}\\
&\qquad \qquad \textbf{return}~~\alpha\\
&\qquad \textbf{if}~~u<+\infty\\
&\qquad \qquad \alpha\leftarrow (l+u)/2\\
&\qquad \textbf{else}\\
&\qquad \qquad \alpha\leftarrow 2l\\
&\textbf{end}
\end{aligned}
$$

**L-BFGS method for possibly nonsmooth functions:**
$$
\begin{aligned}
&\textbf{initialize}~~ x^{0},g^{0}\leftarrow f(x^{0}),\,B^{0}\leftarrow I,\,k\leftarrow 0\\
&\textbf{while}~~ \Vert g^{k}\Vert>\delta\quad\textbf{do}\\
&\qquad d\leftarrow -B^{k}g^{k}\\
&\qquad t\leftarrow \text{Lewis Overton line search}\\
&\qquad x^{k+1}\leftarrow x^{k}+td\\
&\qquad g^{k+1}\leftarrow \nabla f(x^{k+1})\\
&\qquad B^{k+1}\leftarrow \text{Cautious-Limited-Memory-BFGS}(g^{k+1}-g^{k},x^{k+1}-x^{k})\\
&\qquad k\leftarrow k+1\\
&\textbf{end while}\\
&\textbf{return}
\end{aligned}
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

