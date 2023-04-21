# Exploration Method using Graph
#UAV #exploration #planning #paper
## Graph‐based subterranean exploration path planning using aerial and legged robots

> **author**: TungDang, MarcoTranzatto, ShehryarKhattak, FrankMascarich, KostasAlexis, MarcoHutter  
> **journal**: Journal of Field Robotics  
> **link**: [dangGraph2020]([zotero://select/library/items/D3K32WEX])  

### Local Planner - Rapidly Exploring Random Graph
**graph building:**
1. Sample a collision-free configuration $\xi_{\text{{rand}}}$.
2. Find closest vertex $\xi_{\text{nearest}}$ in the graph.
3. Check the straight connection between $\xi_{\text{rand}}$ and $\xi_{\text{nearest}}$, add the sample and path to the graph.
4. Connect denser collision-free edges from the new vertex to the neighbor vertices within a radius.
