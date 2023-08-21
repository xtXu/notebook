# Minimum Snap Trajectory Generation

**Why smooth trajectory:**
+ Good for autonomous moving
+ Velocity/higher order dynamics can't change immediately
+ The robot should not stop at turns
+ Save energy

**Why trajectory generation/optimizaton:**
+ Path quality
+ Time efficiency 

## Smooth trajectory generation
+ Boundary condition: start, goal position (orientations)
+ Intermediate condition: waypoint positions (orientations)
	+ found by path finding
+ Smoothness criteria
	+ minimizing rate of change of "input"

## Differential Flatness
The states and inputs of a quadrotor can be written as **algebraic functions** of **four carefully selected flat outputs and their derivatives.**
+ Any smooth trajectory in the space of flat outputs (with reasonably bounded derivatives) can be followed by the under-actuated quadrotor (geometric control)
+ A possible choice: $\sigma=[x,y,z,\psi]^T$
+ Trajectory in the space of flat outputs: $\sigma(t)=[T_0,T_M]\rightarrow \mathbb{R}^3\times SO(2)$ 

**Quadrotor states:** 
$$
\mathbf{X}=\left[x, y, z, \phi, \theta, \psi, \dot{x}, \dot{y}, \dot{z}, \omega_x, \omega_y, \omega_z\right]^T
$$
**Nonlinear dynamics:**
+ Newtow Equation: 
$$
\begin{aligned}
&m \ddot{\boldsymbol{p}}=\left[\begin{array}{c}0 \\0 \\-m g\end{array}\right]+\boldsymbol{R}\left[\begin{array}{c}0 \\0 \\F_1+F_2+F_3+F_4\end{array}\right]=\left[\begin{array}{c}0 \\0 \\-m g\end{array}\right]+\boldsymbol{R}\left[\begin{array}{c}0\\0\\u_1\end{array}\right]\\
&m \ddot{\boldsymbol{p}}=-m g \mathbf{z}_W+u_1 \mathbf{z}_B
\end{aligned}
$$
+ Euler Equation: 
$$
\begin{aligned}
&\boldsymbol{I} \cdot\left[\begin{array}{c}\dot{\omega}_x \\\dot{\omega}_y \\\dot{\omega}_z\end{array}\right]+\left[\begin{array}{c}\omega_x \\\omega_y \\\omega_z\end{array}\right] \times \boldsymbol{I} \cdot\left[\begin{array}{c}\omega_x \\\omega_y \\\omega_z\end{array}\right]=\left[\begin{array}{c}l\left(F_2-F_4\right) \\l\left(F_3-F_1\right) \\M_1-M_2+M_3-M_4\end{array}\right]=\left[\begin{array}{c}u_2\\u_3\\u_4\end{array}\right]\\
&\boldsymbol{\omega}_B=\left[\begin{array}{l}
\omega_x \\
\omega_y \\
\omega_z
\end{array}\right], \quad \boldsymbol{\dot{\omega}}_B=\boldsymbol{I}^{-1}\left[-\boldsymbol{\omega}_B \times \boldsymbol{I} \cdot \boldsymbol{\omega}_B+\left[\begin{array}{l}
u_2 \\
u_3 \\
u_4
\end{array}\right]\right]
\end{aligned}
$$
Choose the **flat output** $\sigma=[x,y,z,\psi]^T$,
![](../Resource/minimum_snap_img_1.png)
**Orientaion:**
+ From the equation of motion: $$\mathbf{z}_B=\frac{\mathbf{t}}{\|\mathbf{t}\|}, \mathbf{t}=\left[\ddot{\boldsymbol{\sigma}}_1, \ddot{\boldsymbol{\sigma}}_2, \ddot{\boldsymbol{\sigma}}_3+g\right]^T$$
+ Define the yaw vector (Z-X-Y Euler):$$\mathbf{x}_C=\left[\cos \boldsymbol{\sigma}_4, \sin \boldsymbol{\sigma}_4, 0\right]^T$$
+ Orientation can be expressed:$$\mathbf{y}_B=\frac{\mathbf{z}_B \times \mathbf{x}_C}{\left\|\mathbf{z}_B \times \mathbf{x}_C\right\|}, \quad \mathbf{x}_B=\mathbf{y}_B \times \mathbf{z}_B \quad \boldsymbol{R}_B=\left[\begin{array}{lll}\mathbf{x}_B & \mathbf{y}_B & \mathbf{z}_B\end{array}\right]$$
**Angular Velocity:**
+ Take the derivative of the motion equation:$$m \ddot{\boldsymbol{p}}=-m g \mathbf{z}_W+u_1 \mathbf{z}_B . \quad \longrightarrow \quad m \dot{\boldsymbol{a}}=\dot{u}_1 \mathbf{z}_B+\boldsymbol{\omega}_{B W} \times u_1 \mathbf{z}_B$$The $\omega_{BW}$ is the body angular velocity viewed in the world framen  
 
