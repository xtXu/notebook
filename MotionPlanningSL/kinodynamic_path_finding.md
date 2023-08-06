# Kinodynamic Path Finding
**Kinodynamic : Kinematic + Dynamic
+ Kinematic constraints: avoiding obstacle
+ Dynamic constrains: velocity, acceleration, force
+ **Differentially constrained**
+ **Up to force (acceleration)**

**Why kinodynamic planning?**
+ Coarse-to-fine
+ Trajectory only optimize locally
+ Infeasible path means nothing to nonholonomic system
+ ![200](../Resource/kinodynamic_path_finding_img_1.png)

**Unicycle drive model:**
![](../Resource/kinodynamic_path_finding_img_2.png)
$$
\left(\begin{array}{c}
\dot{x} \\
\dot{y} \\
\dot{\theta}
\end{array}\right)=\left(\begin{array}{c}
\cos \theta \\
\sin \theta \\
0
\end{array}\right) \cdot v+\left(\begin{array}{l}
0 \\
0 \\
1
\end{array}\right) \cdot \omega
$$
**Differential drive model:**
![](../Resource/kinodynamic_path_finding_img_3.png)
$$
\left(\begin{array}{c}
\dot{x} \\
\dot{y} \\
\dot{\theta}
\end{array}\right)=\left(\begin{array}{c}
\frac{r}{2}\left(\omega_l+\omega_r\right) \cos \theta \\
\frac{r}{2}\left(\omega_l+\omega_r\right) \sin \theta \\
\frac{r}{L}\left(\omega_r-\omega_l\right)
\end{array}\right)
$$

**Simplified car model:**
![](../Resource/kinodynamic_path_finding_img_4.png)
$$
\left(\begin{array}{c}
\dot{x} \\
\dot{y} \\
\dot{\theta}
\end{array}\right)=\left(\begin{array}{c}
v \cos \theta \\
v \sin \theta \\
\frac{v}{L} \tan \emptyset
\end{array}\right)
$$
+ Simple car model: $|v| \leq v_{\max }, \quad|\emptyset| \leq \emptyset_{\max }<\frac{\pi}{2}$
+ Reeds & Shepp's car: $v \in\left\{-v_{\max }, v_{\max }\right\}, \quad|\emptyset| \leq \emptyset_{\max }<\frac{\pi}{2}$
+ Dubin's car: $v =v_{\max }, \quad|\emptyset| \leq \emptyset_{\max }<\frac{\pi}{2}$

## State Lattice Planning

**Basic Idea:**
+ Require a graph with feasible motion connections
+ Create a graph with all edges executable
	+ **Forward:** discrete in control space
	+ **Reverse:** discrete in state space

For a robot model:
$$
\dot{s}=f(s, u)
$$
$s$ is the state, $u$ is the input.  
+ The robot is differentially driven
+ The initial state is $s_0$
+ Generate feasible local motion by:
	+ Select $u$, fix time $t$, forward simulate the system (numerical integration)
		+ Forward simulation
		+ Fixed $u$, $T$
		+ Easy to implement
		+ *No mission guidance*
		+ *Low planning efficiency*
	+ Select $s_f$, find the connection between $s_0$ and $s_f$
		+ Backward simulation
		+ Good mission guidence
		+ *Need calculate $u$, $T$*
		+ *Hard to implement*

### Sample in control space
For a UAV model:  
State: $\mathrm{s}=\left(\begin{array}{c}x \\ y \\ z \\ \dot{x} \\ \dot{y} \\ \dot{z}\end{array}\right) \quad$ Input: $u=\left(\begin{array}{c}\ddot{x} \\ \ddot{y} \\ \ddot{z}\end{array}\right)$,  
System equation: $\dot{s}=A \cdot s+B \cdot u$,  $A=\left[\begin{array}{llllll}0 & 0 & 0 & 1 & 0 & 0 \\0 & 0 & 0 & 0 & 1 & 0 \\0 & 0 & 0 & 0 & 0 & 1 \\0& 0 & 0 & 0 & 0 & 0 \\0 & 0 & 0 & 0 & 0 & 0 \\0 & 0 & 0 & 0 & 0 & 0\end{array}\right] \quad B=\left[\begin{array}{lll}0 & 0 & 0 \\0 & 0 & 0 \\0 & 0 & 0 \\1 & 0 & 0 \\0 & 1 & 0 \\0 & 0 & 1\end{array}\right]$  
+ Several-order integrator
+ $A$ is **nilpotent** (密邻矩阵): $\exists k>0, \forall i>k, A^i=0$

**State Transition:** $s(t)=e^{At}s_0+[\int_{0}^{t} e^{A(t-\sigma)}B d\sigma] u_m$  
+ Zero-input response $F(t)=e^{At}s_0$
+ Zero-state response $G(t)=[\int_{0}^{t} e^{A(t-\sigma)}B d\sigma] u_m$
+ $e^{AT}:$ state transition matrix, critical to integration
	+ $e^{A t}=I+\frac{A t}{1 !}+\frac{(A t)^2}{2 !}+\frac{(A t)^3}{3 !}+\cdots+\frac{(A t)^k}{k !}+\cdots$
	+ Since $A$ is nilpotent, if $A^n=0$, then $e^{At}$ has a closed-form expression in the form of an (n-1) degree matrix polynomial in $t$.

Sample example:![](../Resource/kinodynamic_path_finding_img_5.png)

The lattice graph obtained by searching:
![](../Resource/kinodynamic_path_finding_img_6.png)
+ During searching, the graph can be built when necessary
+ Create nodes and connection when newly discovered
+ Save computation time/space

### Sample in state place

**Build the lattice graph for a Reeds-Shepp Car Model:**
+ Given an origin
+ for 8 neighbor nodes around the origin, feasible paths are found
+ extend outward to 24 neighbors
+ complete lattice
![500](../Resource/kinodynamic_path_finding_img_7.png)

**Another example:**
+ Two layer lattice graph
+ Only first layer is different
+ Different initial states
![](../Resource/kinodynamic_path_finding_img_8.png)

