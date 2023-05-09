# 基于NeRF的三维内容生成
对应视频：[深蓝NeRF公开课](https://www.shenlanxueyuan.com/course/504)

3D contents = shape + (material + lighting)(Appearance)  
3D contents => Rendering => Images (computer graphics)  
Images => Inverse Rendering => 3D contents (computer vision)
![](../Resources/基于NeRF的三维内容生成_img_1.png)

3D Reconstruction: high quality geometric structure, no appearance (material or lighting).

3 key factors in inverse rendering:
+ shape representation
+ appearance representation
+ render operation

**Shape representation:**
+ Mesh
+ Point Cloud
+ Occupancy field
+ Signed distance field

**Appearance representation:**
+ Material texture map & environmental lighting.   
  (separate texture and light, ideal but hard to solve)
+ Radiance field (surface light field).   
  (hard to edit, hard to solve in new environment)

![](../Resources/基于NeRF的三维内容生成_img_2.png)

**Rendering operators:**
+ Depend on the specific shape and appearance representations.
+ Need to be fully differentiable. (can optimize the L2 loss between the rendered image and the groudtruth image)

## NeRF
NeRF can generate highly photo-realistic novel views.

Break NeRF in 3 key factors:
+ shape: soft opacity field (fog) $(x,y,z)\rightarrow \sigma$.   
  $\sigma$:光在空间传播单位距离能量衰减的速度（材质对光照的吸收速率）。
  ![](../Resources/基于NeRF的三维内容生成_img_3.png)
+ appearance: radiance field $(x,y,z,\theta,\phi)\rightarrow rgb$
+ rendering

**Soft shape:**  
Compare with hard geometry:
+ Not require object segmentation masks: genus issue.
+ No boundary discontinuity: easy for differential rendering.
+ Rendering is more expensive, editing is hard.

**Fourier features fixed the spectral bias of MLPs.**
$$
\gamma(\mathbf{v})=\left[\ldots, \cos \left(2 \pi \sigma^{j / m} \mathbf{v}\right), \sin \left(2 \pi \sigma^{j / m} \mathbf{v}\right), \ldots\right]^{\mathrm{T}} \text { for } j=0, \ldots, m-1
$$

**Neural fields for inverse rendering:**
+ Compact: each NeRF is only ~10MB
+ Continuous: scenes are not discretized
+ Flexible and easy for optimization: implicit regularization from neural networks.
![](../Resources/基于NeRF的三维内容生成_img_4.png)

**NeRF geometry quality: the surface normal is noisy ( low local geometry quality), but the depth map does not capture details.**



 