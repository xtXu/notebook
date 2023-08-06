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


