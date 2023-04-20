# Review of Linear Algebra

#math #linear #algebra

## Vectors
![](Resources/review_linear_algebra_img_1.png)

- Usually written as $\vec{a}$ or in bold $\mathbf{a}$
- Or using start and end points $\overrightarrow{AB}=B-A$
- Magnitude of a vector $\left\Vert \vec{a} \right\Vert$
- Unit vector: $\hat{a}=\vec{a}/\left\Vert \vec{a} \right\Vert$

### Cartesian Coordinates

![](Resources/review_linear_algebra_img_2.png)
- X and Y can be any (usually **orthogonal unit**) vectors
    - $\mathbf{A}=\begin{pmatrix}x \\ y\end{pmatrix}$, $\mathbf{A}^\top=(x,y)$, $\left\Vert \mathbf{A} \right\Vert = \sqrt{x^2+y^2}$

### Dot Product

![](Resources/review_linear_algebra_img_3.png)
$\vec{a} \cdot \vec{b}=\left\Vert \vec{a} \right\Vert \Vert \vec{b} \Vert \cos\theta$ ,     $\cos\theta=\frac{\vec{a}\cdot\vec{b}}{\Vert a \Vert \Vert b \Vert}$

#### Dot Production in Cartesian Coordinates

Component-wise multiplication, then adding up

- In 2D

$$
\vec{a} \cdot \vec{b}=\begin{pmatrix}x_a \\y_a\end{pmatrix} \cdot\begin{pmatrix}x_b \\y_b\end{pmatrix}=x_a x_b+y_a y_b
$$

- In 3D
    
    $$
    \vec{a} \cdot \vec{b}=\begin{pmatrix}x_a \\y_a\\z_a\end{pmatrix} \cdot\begin{pmatrix}x_b \\y_b\\z_b\end{pmatrix}=x_a x_b+y_a y_b+z_a z_b
    $$
    

#### Dot Product in Graphics

- Find angle between two vectors
- Finding projection of one vector on another
    - $\vec{b}_\perp$: projection of $\vec{b}$ onto $\vec{a}$
    - $\vec{b}_\perp = k\hat{a}$, $k=\Vert \vec{b}_\perp \Vert=\Vert\vec{b}\Vert\cos\theta$
![](Resources/review_linear_algebra_img_4.png)
- Measure how close two directions are
- Decompose a vector
- Determine forward / backward (dot product >0 or <0)

### Cross Product

![](Resources/review_linear_algebra_img_5.png)
- Cross product is orthogonal to two initial vectors
- Direction determined by right-hand rule
- Useful in contructing coordinate systems

#### Properties

$$
\begin{gathered}\vec{a} \times \vec{b}=-\vec{b} \times \vec{a} \\\vec{a} \times \vec{a}=\overrightarrow{0} \\\vec{a} \times(\vec{b}+\vec{c})=\vec{a} \times \vec{b}+\vec{a} \times \vec{c} \\\vec{a} \times(k \vec{b})=k(\vec{a} \times \vec{b})\end{gathered}
$$

#### Cartesian Formula

$$
\vec{a} \times \vec{b}=\left(\begin{array}{c}y_a z_b-y_b z_a \\z_a x_b-x_a z_b \\x_a y_b-y_a x_b\end{array}\right)
$$

$$
\vec{a} \times \vec{b}=A^* b=\left(\begin{array}{ccc}0 & -z_a & y_a \\z_a & 0 & -x_a \\-y_a & x_a & 0\end{array}\right)\left(\begin{array}{l}x_b \\y_b \\z_b\end{array}\right)
$$

#### Cross Product in Graphics

- Determine left / right ($(\vec{a}\times\vec{b})_z$ >0 or <0)
- Determine inside / outside
![](Resources/review_linear_algebra_img_6.png)


## Orthonormal Coordinate Frames

- Any set of 3 vectors (in 3D) that

$$
\begin{gathered}\|\vec{u}\|=\|\vec{v}\|=\|\vec{w}\|=1 \\\vec{u} \cdot \vec{v}=\vec{v} \cdot \vec{w}=\vec{u} \cdot \vec{w}=0 \\\vec{w}=\vec{u} \times \vec{v} \\\vec{p}=(\vec{p} \cdot \vec{u}) \vec{u}+(\vec{p} \cdot \vec{v}) \vec{v}+(\vec{p} \cdot \vec{w}) \vec{w}\end{gathered}
$$

## Matrices

### Matrix-Vector Multiplication

- Treat vector as a column matrix
- Key for transforming points

### Inverses

$$
\begin{aligned} AA^{-1}&=A^{-1}A=I \\ (AB)^{-1}&=B^{-1} A^{-1} \end{aligned}
$$

### Vector multiplication in Matrix form

- Dot product: $\vec{a}\cdot\vec{b}=\vec{a}^\top\vec{b}$
- Cross product: $A$ is known as **dual matrix** of $\vec{a}$

$$
\vec{a} \times \vec{b}=A^* b=\left(\begin{array}{ccc}0 & -z_a & y_a \\z_a & 0 & -x_a \\-y_a & x_a & 0\end{array}\right)\left(\begin{array}{l}x_b \\y_b \\z_b\end{array}\right)
$$
