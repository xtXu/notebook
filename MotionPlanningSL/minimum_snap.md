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