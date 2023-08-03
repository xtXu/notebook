# Sample-based Path Finding
## Notion of Completeness Planning
+ Complete: always answers a path planning query correctly in bounded time
+ Probabilistic Complete: if solution exists, planner will eventually find it, using random sampling
+ Resolution Complete: same as "Probabilistic" but based on a deterministic sampling (e.g. sampling on a fixed grid)

## Probabilistic Road Map
PRM:
+ A graph structure
+ Two phases planning:
	+ Learning 
	+ Query
+ 