# Frenet坐标系和Cartesian坐标系

**龙格现象**：高次多项式的拟合可能出现振荡，尽可能用分段低次多项式

## 问题描述
![](../Resource/frenet_cartesian_img_1.png)

车: host vehicle
已知车在Cartesian坐标系下的位置 $r_h$, 速度 $v_h$, 加速度 $a_h$, 曲率 $k_h$，求车以道路为坐标轴的frenet坐标系下的坐标:  $$s,\dot{s},\ddot{s},l,l',l'',\dot{l},\ddot{l}$$
其中，$$\dot{s}=\frac{ds}{dt},\dot{l}=\frac{dl}{dt},l'=\frac{dl}{ds}$$
对于 EM Planner，求 $s,\dot{s},\ddot{s},l,l',l''$  
对于 Lattice Planner，求 $s,\dot{s},\ddot{s},l,\dot{l},\ddot{l}$

