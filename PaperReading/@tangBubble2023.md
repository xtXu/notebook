# Bubble Explorer: Fast UAV Exploration in Large-Scale and Cluttered 3D-Environments using Occlusion-Free Spheres

## Metadata
- **CiteKey**: tangBubble2023
- **Type**: Preprint
- **Title**: Bubble Explorer: Fast UAV Exploration in Large-Scale and Cluttered 3D-Environments using Occlusion-Free Spheres
- **Author**: Tang, Benxu; Ren, Yunfan; Zhu, Fangcheng; He, Rui; Liang, Siqi; Kong, Fanze; Zhang, Fu 
- **Year**: 2023 


## Abstract
Autonomous exploration is a crucial aspect of robotics that has numerous applications. Most of the existing methods greedily choose goals that maximize immediate reward. This strategy is computationally efficient but insufficient for overall exploration efficiency. In recent years, some state-of-the-art methods are proposed, which generate a global coverage path and significantly improve overall exploration efficiency. However, global optimization produces high computational overhead, leading to low-frequency planner updates and inconsistent planning motion. In this work, we propose a novel method to support fast UAV exploration in large-scale and cluttered 3-D environments. We introduce a computationally low-cost viewpoints generation method using novel occlusion-free spheres. Additionally, we combine greedy strategy with global optimization, which considers both computational and exploration efficiency. We benchmark our method against state-of-the-art methods to showcase its superiority in terms of exploration efficiency and computational time. We conduct various real-world experiments to demonstrate the excellent performance of our method in large-scale and cluttered environments.
## Files and Links
- **Local Library**: [Zotero](zotero://select/library/items/JDGIH3DQ)
- **File**:[arXiv.org Snapshot](zotero://open-pdf/library/items/GN3TVR4R); [Tang_2023_Bubble_Explorer.pdf](zotero://open-pdf/library/items/IHEWAUZJ)

## Tags and Collections
- **Keywords**: /unread; Computer Science - Robotics
- **Collections**: Temp


---

## Comments
*   Exploration
*   Frontier-based, similar to FUEL
*   A novel concept: occlusion-free sphere to generate high-quality viewpoints around the frontiers.
*   A novel strategy that combines greedy and global optimization: choose a subset of viewpoints with greatest gain, and plan a global path via ATSP.


---

## Method
### Occlusion-Free Sphere
An occlusion-free sphere is defined by its center $\mathbf{p}_{c}\in \mathbb{R}^{3}$, which lies on the target frontier, and the radius: $r=\Vert \mathrm{p}_{c}-\mathrm{p}_{o}\Vert_{2}$, where $\mathrm{p}_{o}\in\mathbb{R}^{3}$ is the nearest neighbor obstacle point (NN point).  
Since the sphere is free and convex, any segments inside the sphere is occlusion-free. By employing a viewpoint sampling strategy on the sphere’s surface, no ray-casting is needed to check the visibility to the frontier cells from the viewpoints, which save the computation time.
### Viewpoints Generation
1. Incrementally search frontier cells, and down-sample to generate the set of sphere center candidates $\mathbf{C}$
2. 

---

## Extracted Annotations
