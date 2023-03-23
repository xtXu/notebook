# Level Set

#digital_image #level_set
## Curve Evolution
### Concept
We have a curve, every point has a velocity that defines how that curve is moving, 
$$C_t=\overrightarrow{V}$$
written in exlicit form:
$$
\frac{\partial C(p)}{\partial t}=\overrightarrow{V}(p,t)
$$

### Property
+ Tangential components do not affect the geometry of an evolving curve
$$C_t=\overrightarrow{V} \Leftrightarrow C_t=\left\langle\overrightarrow{V},\overrightarrow{n}\right\rangle \overrightarrow{n}$$



## Level Sets
### Implicit representation
A closed planar curve: 
$$C(p):\mathrm{S^1}\rightarrow\mathrm{R^2}$$
whose geometric trace can be repersented implicitly as
$$C=\left\{(x,y) | \phi(x,y)=0   \right\}$$
By doing this, we can define the curve as the zero level set of function $\phi(x,y)$.  

For example, we can define the $\phi$ to be positive inside, and negative outside. One example is to define that as the distance to the curve. Every point on the curve has a 0 distance. Every point inside has a positive distance, and point outside has a negative distance. 

### Properties of level sets
| notion  | meaning   |
|-------------- | -------------- |
| $\overrightarrow{T}$    | Tangent     |
| $\overrightarrow{N}$    | Normal      |

#### The level set normal
$$
\overrightarrow{N}=-\frac{\nabla\phi}{|\nabla\phi|}\quad\left(\overrightarrow{T}=\frac{\overline\nabla\phi}{|\nabla\phi|}\right)
$$
The - means the normal is defined to point to the inside.

***Proof.***  
Along the level sets, we have zero change, that is $\phi_s=0$, so by the chain rule
$$
\phi_s(x, y)=\phi_x x_s+\phi_y y_s=\langle\nabla \phi, \vec{T}\rangle
$$
So,
$$
\left\langle\frac{\nabla \phi}{|\nabla \phi|}, \vec{T}\right\rangle=0 \Rightarrow \frac{\nabla \phi}{|\nabla \phi|} \perp \vec{T} \Rightarrow \vec{N}=-\frac{\nabla \phi}{|\nabla \phi|}
$$
