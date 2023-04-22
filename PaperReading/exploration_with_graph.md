# Exploration Method using Graph
#UAV #exploration #planning #paper #graph
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

**path planning**:
1. Find shortest paths from the root to all other vertices using Dijkstra.
2. Compute the ExplorationGain considering the gain, the distance and the direction.
3. Select the path maximizing the ExplorationGain.

### Global Planner - Sparse Graph from Local Graph
**graph building:**
1. Add the best path by introducing new vertices and edges along such paths., and add extra edges from the new vertex to the neighbors.
2. Add additional paths from the list of shortest paths of the local graph which contain vertices with high volumetric gain. To reduce the number of paths, cluster the high-gain paths by DTW similarity metric and only the longest paths from each cluster is added.
3. The leaf vertices from the added paths are marked as "frontiers". The volumetric gain of each frontier is re-evaluated periodically.

**path planning**:
1. Two times Dijkstra to (a) find the paths from current position to all frontiers, (b) find the paths from frontiers to the home.
2. Evaluate GlobalExplorationGain for each frontier considering the volumetric gain, the distance from current to frontier, and the time current->frontier->home.

### Path Refinement


