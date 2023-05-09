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

