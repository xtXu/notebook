# Exploration Method using Graph
#UAV #exploration #planning #paper #graph
## Graph-based Path Planning for Autonomous Robotic Exploration in Subterranean Environments

> **author**: TungDang, FrankMascarich, ShehryarKhattak, ChristosPapachristos, KostasAlexis  
> **journal**:   
> **conference**: 2019 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)  
> **link**: [dangGraphbased2019](zotero://select/library/items/SVLFRL4Z)  

**graph building**: similar to following [Graph‐based subterranean exploration path planning using aerial and legged robots](#Graph‐based%20subterranean%20exploration%20path%20planning%20using%20aerial%20and%20legged%20robots).

## Graph‐based subterranean exploration path planning using aerial and legged robots

> **author**: TungDang, MarcoTranzatto, ShehryarKhattak, FrankMascarich, KostasAlexis, MarcoHutter  
> **journal**: Journal of Field Robotics  
> **link**: [dangGraph2020](zotero://select/library/items/D3K32WEX)  

### Local Planner - Rapidly Exploring Random Graph
**graph building:**
1. Sample a collision-free configuration $\xi_{\text{{rand}}}$.
2. Find closest vertex $\xi_{\text{nearest}}$ in the graph.
3. Check the straight connection between $\xi_{\text{rand}}$ and $\xi_{\text{nearest}}$, add the sample and path to the graph.
4. Connect denser collision-free edges from the new vertex to the neighbor vertices within a radius.  


### Global Planner - Sparse Graph from Local Graph
**graph building:**
1. Add the best path by introducing new vertices and edges along such paths, and add extra edges from the new vertex to the neighbors.
2. Add additional paths from the list of shortest paths of the local graph which contain vertices with high volumetric gain. To reduce the number of paths, cluster the high-gain paths by DTW similarity metric and only the longest paths from each cluster is added.
3. The leaf vertices from the added paths are marked as "frontiers". The volumetric gain of each frontier is re-evaluated periodically.

## DSVP: Dual-Stage Viewpoint Planner for Rapid Exploration by Dynamic Expansion

> **author**: HongbiaoZhu, ChaoCao, YukunXia, SebastianScherer, JiZhang, WeidongWang  
> **journal**:   
> **conference**: 2021 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)  
> **link**: [zhuDSVP2021](zotero://select/library/items/F64YLF4C)  

### Global Planner: Global Graph
1. The graph is incrementally built based on the viewpoints in branches with positive gain.
2. Ensure the graph is not too dense while providing short paths between vertices.

## Autonomous Robotic Exploration by Incremental Road Map Construction

> **author**: ChaoqunWang, WenzhengChi, YuxiangSun, Max Q.-H.Meng  
> **journal**: IEEE Transactions on Automation Science and Engineering  
> **conference**:   
> **link**: [wangAutonomous2019](zotero://select/library/items/3P58NINL)  

### Road Map for global path
- **Sample in the laser range finder scope**; sampling distribution depends on the laser ray: sample more points on the longer ray;
- The road map is built on the sample points and the frontier cluster;
- The edge is connected without collision-checking; the collision-checking is performed during path finding;
- The generated sampling points will only be connected to only one node on the graph;
## Efficient Autonomous Exploration With Incrementally Built Topological Map in 3-D Environments

> **author**: ChaoqunWang, HanMa, WeinanChen, LiLiu, Max Q.-H.Meng  
> **journal**: IEEE Transactions on Instrumentation and Measurement  
> **conference**:   
> **link**: [wangEfficient2020](zotero://select/library/items/MGBFHNIA)  
### Road map
- Sample points **within the sensor scope**;
- The new point is only connected to the nearest node, similar to RRT;
- Detect frontiers using the road map nodes;

