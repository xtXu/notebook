# Exploration Method using Graph
#UAV #exploration #planning #paper #graph
## Graph-based Path Planning for Autonomous Robotic Exploration in Subterranean Environments

> **author**: TungDang, FrankMascarich, ShehryarKhattak, ChristosPapachristos, KostasAlexis  
> **journal**:   
> **conference**: 2019 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)  
> **link**: [dangGraphbased2019](zotero://select/library/items/SVLFRL4Z)  


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
1. Add the best path by introducing new vertices and edges along such paths., and add extra edges from the new vertex to the neighbors.
2. Add additional paths from the list of shortest paths of the local graph which contain vertices with high volumetric gain. To reduce the number of paths, cluster the high-gain paths by DTW similarity metric and only the longest paths from each cluster is added.
3. The leaf vertices from the added paths are marked as "frontiers". The volumetric gain of each frontier is re-evaluated periodically.


