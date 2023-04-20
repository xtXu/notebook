# Transformation

#transform #graphics

## 2D Transformations

### Scale
![](Resources/transformation_img_1.png)

$$
\begin{aligned} x'&=sx \\ y'&=sy\\\left[\begin{array}{l}
x^{\prime} \\
y^{\prime}
\end{array}\right]&=\left[\begin{array}{ll}
s & 0 \\
0 & s
\end{array}\right]\left[\begin{array}{l}
x \\
y
\end{array}\right] \end{aligned}
$$
![](Resources/transformation_img_2.png)


$$
\left[\begin{array}{l}
x^{\prime} \\
y^{\prime}
\end{array}\right]=\left[\begin{array}{ll}
s_x & 0 \\
0 & s_y  
\end{array}\right]\left[\begin{array}{l}
x \\
y
\end{array}\right] 
$$

### Reflection

- Horizontal reflection
![](Resources/transformation_img_3.png)
$$
\begin{aligned} x'&=-x \\ y'&=y\\\left[\begin{array}{l}
x^{\prime} \\
y^{\prime}
\end{array}\right]&=\left[\begin{array}{ll}
-1 & 0 \\
0 & 1
\end{array}\right]\left[\begin{array}{l}
x \\
y
\end{array}\right] \end{aligned}
$$

### Shear Matrix
![](Resources/transformation_img_4.png)
$$
\left[\begin{array}{l}
x^{\prime} \\
y^{\prime}
\end{array}\right]=\left[\begin{array}{ll}
1 & a \\
0 & 1  
\end{array}\right]\left[\begin{array}{l}
x \\
y
\end{array}\right] 
$$

### Rotate (origin(0, 0), CCW by default)
![](Resources/transformation_img_5.png)
$$
\begin{aligned}&\mathbf{R}_\theta=\left[\begin{array}{cc}\cos \theta & -\sin \theta \\\sin \theta & \cos \theta\end{array}\right] \\ &\mathbf{R}_{-\theta}=\mathbf{R}_\theta^\top=\mathbf{R}_\theta^{-1}\end{aligned}
$$

### Linear Transforms = Matrices

$$
\begin{aligned}x^{\prime} & =a x+b y \\y^{\prime} & =c x+d y \\{\left[\begin{array}{l}x^{\prime} \\y^{\prime}\end{array}\right] } & =\left[\begin{array}{ll}a & b \\c & d\end{array}\right]\left[\begin{array}{l}x \\y\end{array}\right] \\\mathbf{x}^{\prime} & =\mathbf{M} \mathbf{x}\end{aligned}
$$

### Translation
![](Resources/transformation_img_6.png)
$$
\begin{aligned}x'&=x+t_x\\y'&=y+t_y  \end{aligned}
$$

- Translations cannot be repersented in matrix form

$$
\left[\begin{array}{l}
x^{\prime} \\
y^{\prime}
\end{array}\right]=\left[\begin{array}{ll}
a & b \\
c & d  
\end{array}\right]\left[\begin{array}{l}
x \\
y
\end{array}\right]+\left[\begin{array}{l}t_x\\t_y \end{array}\right]
$$

- But we do not want translation to be a special case…
- Solution: **Homogenous Coordinate**

## Homogenous Coordinates

- Add a third coordiante
    - 2D point $(x,y,1)^\top$
    - 2D vector $(x,y,0)^\top$ (vector’s direction matters, the translations don’t change it)
- Matrix representation of translations

$$
\left(\begin{array}{c}x^{\prime} \\y^{\prime} \\w^{\prime}\end{array}\right)=\left(\begin{array}{ccc}1 & 0 & t_x \\0 & 1 & t_y \\0 & 0 & 1\end{array}\right) \cdot\left(\begin{array}{l}x \\y \\1\end{array}\right)=\left(\begin{array}{c}x+t_x \\y+t_y \\1\end{array}\right)
$$

- In homogeneous coordinates, $\begin{pmatrix}x\\y\\w\end{pmatrix}$ is the 2D point $\begin{pmatrix}x/w\\y/w\\1\end{pmatrix}$, $w\neq 0$, so point + point will return the midpoint.

### 2D transformation in homogenous coordinates

#### Scale

$$
\mathbf{S}(s_x,s_y)=\begin{pmatrix} sx & 0 & 0 \\0 & sy & 0\\0 & 0 & 1\end{pmatrix}
$$

#### Rotation

$$
\mathbf{R}(\alpha)=\begin{pmatrix} \cos\alpha & -\sin\alpha & 0 \\ \sin\alpha & \cos\alpha & 0 \\ 0 & 0 & 1 \\\end{pmatrix}
$$

#### Translation

$$
\mathbf{T}(t_x,t_y)=\begin{pmatrix}1 & 0 & t_x \\ 0 & 1 & t_y \\ 0 & 0 & 1\end{pmatrix}
$$

## Affine Transformations

- Affine map = linear map + translation

$$
\left[\begin{array}{l}
x^{\prime} \\
y^{\prime}
\end{array}\right]=\left[\begin{array}{ll}
a & b \\
c & d  
\end{array}\right]\left[\begin{array}{l}
x \\
y
\end{array}\right]+\left[\begin{array}{l}t_x\\t_y \end{array}\right]
$$

- Using homogenous coordinates:

$$
\left[\begin{array}{l}
x^{\prime} \\
y^{\prime} \\ 1
\end{array}\right]=\left[\begin{array}{lll}
a & b & t_x\\
c & d & t_y\\ 0 & 0 & 1
\end{array}\right]\left[\begin{array}{l}
x \\
y \\ 1
\end{array}\right]
$$

### Inverse Transform

- $\mathbf{M}^{-1}$ is the inverse of transform $\mathbf{M}$ in both a matrix and geometric sense.
![](Resources/transformation_img_7.png)
### Composing Transform

- **Transform Ordering Matters!**
- Sequence of affine transforms $\mathbf{A}_1,\mathbf{A}_2,\mathbf{A}_3,\dots,$

$$
A_n\left(\ldots A_2\left(A_1(\mathbf{x})\right)\right)=\mathbf{A}_n \cdots \mathbf{A}_2 \cdot \mathbf{A}_1 \cdot\left(\begin{array}{l}x \\y \\1\end{array}\right)
$$

### Decomposing Complex Transforms

How to rotate around a given point $\mathbf{c}$ ?

1. Translate center to origin
2. Rotate
3. Translate back

Matrix repersentation:

$$
\mathbf{T}(\mathbf{c})\cdot\mathbf{R}(\alpha)\cdot\mathbf{T}(-\mathbf{c})
$$

## 3D Transforms

- Use homogeneous coordinates again:
    - 3D point  = $(x,y,z,1)^\top$
    - 3D vector  = $(x,y,z,0)^\top$
- In general, $(x,y,z,w)(w\neq 0)$ is the 3D point:

$$
(x/w, y/w, z/w)
$$

- Use 4x4 matrices for affine transformations

$$
\left(\begin{array}{c}x^{\prime} \\y^{\prime} \\z^{\prime} \\1\end{array}\right)=\left(\begin{array}{llll}a & b & c & t_x \\d & e & f & t_y \\g & h & i & t_z \\0 & 0 & 0 & 1\end{array}\right) \cdot\left(\begin{array}{l}x \\y \\z \\1\end{array}\right)
$$

### Scale

$$
\mathbf{S}\left(s_x, s_y, s_z\right)=\left(\begin{array}{cccc}
s_x & 0 & 0 & 0 \\
0 & s_y & 0 & 0 \\
0 & 0 & s_z & 0 \\
0 & 0 & 0 & 1
\end{array}\right)
$$

### Translation

$$
\mathbf{S}\left(s_x, s_y, s_z\right)=\left(\begin{array}{cccc}
1 & 0 & 0 & t_x \\
0 & 1 & 0 & t_y \\
0 & 0 & 1 & t_z \\
0 & 0 & 0 & 1
\end{array}\right)
$$

### Rotation

#### Rotation around x-, y-, or z- axis
![](Resources/transformation_img_8.png)
$$
\begin{aligned}& \mathbf{R}_x(\alpha)=\left(\begin{array}{cccc}1 & 0 & 0 & 0 \\0 & \cos \alpha & -\sin \alpha & 0 \\0 & \sin \alpha & \cos \alpha & 0 \\0 & 0 & 0 & 1\end{array}\right) \\& \mathbf{R}_y(\alpha)=\left(\begin{array}{cccc}\cos \alpha & 0 & \sin \alpha & 0 \\0 & 1 & 0 & 0 \\-\sin \alpha & 0 & \cos \alpha & 0 \\0 & 0 & 0 & 1\end{array}\right) \\& \mathbf{R}_z(\alpha)=\left(\begin{array}{cccc}\cos \alpha & -\sin \alpha & 0 & 0 \\\sin \alpha & \cos \alpha & 0 & 0 \\0 & 0 & 1 & 0 \\0 & 0 & 0 & 1\end{array}\right)\end{aligned}
$$

#### Compose any 3D rotation from $\mathbf{R}_x,\mathbf{R}_y,\mathbf{R}_z$?

$$
\mathbf{R}_{xyz}=\mathbf{R}_x(\alpha)\mathbf{R}_y(\beta)\mathbf{R}_z(\gamma)
$$

- So called **Euler angles**
- In flight simulator: roll, pitch, yaw

#### Rodrigues’Rotation Formula

- Rotation by angle $\alpha$ around axis $\mathbf{n}$
    
    $$
    \mathbf{R}(\mathbf{n}, \alpha)=\cos (\alpha) \mathbf{I}+(1-\cos (\alpha)) \mathbf{n} \mathbf{n}^T+\sin (\alpha) \underbrace{\left(\begin{array}{ccc}0 & -n_z & n_y \\n_z & 0 & -n_x \\-n_y & n_x & 0\end{array}\right)}_{\mathbf{N}}
    $$
    

## Viewing Transformation

### View / Camera Transformation

- Define the camera
    - Position $\vec{e}$
    - Look-at / gaze direction $\hat{g}$
    - Up direction $\hat{t}$
    
    ![](Resources/transformation_img_9.png)
    
- Key observation
    - camera and objects move together → the “photo”will be the same
- We always transform the camera to
    - The origin, up at Y, look at -Z
    - And transform the objects along with the camera
    
    ![](Resources/transformation_img_10.png)
    
    - Transform by $M_{view}=R_{view}T_{view}$, (first translate to origin, then rotate)
        
        $$
        \mathbf{T}_{view}=\left(\begin{array}{cccc}
        1 & 0 & 0 & -x_e \\
        0 & 1 & 0 & -y_e \\
        0 & 0 & 1 & -z_e \\
        0 & 0 & 0 & 1
        \end{array}\right)
        $$
        
    - Rotate g to -Z, t to Y, (g x t) to X, first consider its inverse rotation:
        
        $$
        R_{view}^{-1}=\left[\begin{array}{cccc}x_{\hat{g} \times \hat{t}} & x_t & x_{-g} & 0 \\y_{\hat{g} \times \hat{t}} & y_t & y_{-g} & 0 \\z_{\hat{g} \times \hat{t}} & z_t & z_{-g} & 0 \\0 & 0 & 0 & 1\end{array}\right]
        $$
        
    - Then according to $R^{-1}=R^\top$
        
        $$
        R_{view}=\left[\begin{array}{cccc}x_{\hat{g} \times \hat{t}} & y_{\hat{g} \times \hat{t}} & z_{\hat{g} \times \hat{t}} & 0 \\x_t & y_t & z_t & 0 \\x_{-g} & y_{-g} & z_{-g} & 0 \\0 & 0 & 0 & 1\end{array}\right]
        $$
        

### Projection Transformation

- Projection in Computer Graphics
    - 3D to 2D
    - Orthographic projection
    - Perspective projection
    ![](Resources/transformation_img_11.png)
    
    - Perpective projection vs. orthographic projection
        
        ![](Resources/transformation_img_12.png)

#### Orthographic  Projection

- A simple way of understanding
    - camera located at origin, up at Y, look at -Z
    - drop Z coordinate
    - Translate and scale the resulting rectangle to $[-1,1]^2$
- In general
    - map a cuboid $[l,r]\times[b,t]\times[f,n]$ to the canonical cube $[-1,1]^3$
    
    ![](Resources/transformation_img_13.png)
    
    - Transformation: translate fisrt, then scale
    
    $$
    M_{\text {ortho }}=\left[\begin{array}{cccc}\frac{2}{r-l} & 0 & 0 & 0 \\0 & \frac{2}{t-b} & 0 & 0 \\0 & 0 & \frac{2}{n-f} & 0 \\0 & 0 & 0 & 1\end{array}\right]\left[\begin{array}{cccc}1 & 0 & 0 & -\frac{r+l}{2} \\0 & 1 & 0 & -\frac{t+b}{2} \\0 & 0 & 1 & -\frac{n+f}{2} \\0 & 0 & 0 & 1\end{array}\right]
    $$
    
- Caveat
    - Looking at / along -Z makes near and far not intuitive ($n>f$)

#### Perspective Projection

- Recall: property of homogeneous coordinates
    - $(x,y,z,1)$, $(kx,ky,kz,k\neq 0)$, $(xz, yz, z^2, z\neq 0)$ all repersent the same point $(x,y,z)$ in 3D
- How to do perspective projection
    - First “squish” the frustum into a cuboid (n→n, f→f), ($\mathbf{M}_{persp→ortho}$)
    - Second do orthographic projection($\mathbf{M}_{ortho}$)

    ![](Resources/transformation_img_14.png)
    
- The “squish” operation require:
    - n is constant
    - the Z of f is constant
    - the center of f is constant
- In order to find a transformation, find the relationship between transformed points $(x^\prime, y^\prime, z^\prime)$ and the original points $(x,y,z)$
    ![](Resources/transformation_img_15.png)
    $$
    x^\prime=\frac{n}{z}x, y^\prime=\frac{n}{z}y
    $$
    
    So we can get
    
    $$
    \mathbf{M}_{persp->ortho}\begin{pmatrix}x\\y\\z\\1\end{pmatrix}=\begin{pmatrix}x^\prime\\y^\prime\\z^\prime\\1\end{pmatrix}=\begin{pmatrix}nx/z\\ny/z\\z^\prime\\1\end{pmatrix}
    $$
    
    According to “ $(x,y,z,1)$, $(kx,ky,kz,k\neq 0)$ represent the same point ”
    
    $$
    \begin{pmatrix}nx/z\\ny/z\\z^\prime\\1\end{pmatrix}==\begin{pmatrix}nx\\ny\\z^\prime z\\z\end{pmatrix}
    $$
    
    So
    
    $$
    \mathbf{M}_{persp->ortho}\begin{pmatrix}x\\y\\z\\1\end{pmatrix}=\begin{pmatrix}nx\\ny\\z^\prime z\\z\end{pmatrix}
    $$
    
    We expect the $\mathbf{M}_{persp->ortho}$ be a constant matrix, but $z^\prime$ is unknown, so
    
    $$
    \mathbf{M}_{persp->ortho}=\left(\begin{array}{cccc}n & 0 & 0 & 0 \\0 & n & 0 & 0 \\? & ? & ? & ? \\0 & 0 & 1 & 0\end{array}\right)
    $$
    
    Let $a^\top=\mathbf{M}_{persp->ortho}(3,:)$, then 
    
    $$
    a^\top\begin{pmatrix}x\\y\\z\\1\end{pmatrix}=z^\prime z
    $$
    
    According to “n is constant”, when $z=n$, $z^\prime=n$, $a$ is a constant vector, so
    
    $$
    a^\top\begin{pmatrix}x\\y\\n\\1\end{pmatrix}=n^2 \longrightarrow \begin{array}{l}a^\top=\begin{pmatrix} 0 &0& A &B\end{pmatrix} \\ An+B=n^2\end{array} 
    $$
    
    According to “the Z of f is constant”, when $z=f$, $z^\prime=f$, so
    
    $$
    a^\top\begin{pmatrix}x\\y\\f\\1\end{pmatrix}=f^2 \longrightarrow Af+B=f^2
    $$
    
    Then we can obtain 
    
    $$
    \begin{array}{l}An+B=n^2\\Af+B=f^2 \end{array} \longrightarrow \begin{array}{l}A=n+f\\B=-nf \end{array} \longrightarrow a^\top = \begin{pmatrix}0&0&n+f&-nf \end{pmatrix}
    $$
    
    Finally, The transform $\mathbf{M}_{persp->ortho}$ is as follows: 
    
    $$
    \mathbf{M}_{persp->ortho}=\left(\begin{array}{cccc}n & 0 & 0 & 0 \\0 & n & 0 & 0 \\0 & 0 & n+f & -nf \\0 & 0 & 1 & 0\end{array}\right)
    $$
    
- $\mathbf{M}_{persp}=\mathbf{M}_{ortho}\mathbf{M}_{persp->ortho}$
- Camera model:
    
  ![](Resources/transformation_img_16.png)
    
    Convert from fovY and aspect to l, r, b, t ?
    
    ![](Resources/transformation_img_17.png)
    
    $$
    \begin{aligned}\tan\frac{fovY}{2}&=\frac{t}{\vert n \vert} \\aspect&=\frac{r}{t}\end{aligned}
    $$
