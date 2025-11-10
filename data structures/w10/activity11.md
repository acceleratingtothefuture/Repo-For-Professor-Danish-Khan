# Activity 11: Graphs

## Explain with the help of an example "Why Dijkstra's algorithm fails on negative weights".
<img width="1807" height="993" alt="Untitled" src="https://github.com/user-attachments/assets/4551ab9f-9a81-464d-a9b1-d3ea9357653b" />

Suppose you want a shortest path from the start to the end. Dijkstra’s algorithm does the following:

1. Set every node’s distance to infinity except the start, which gets 0.

2. From the nodes you can already reach with a finite distance but haven’t expanded yet, pick the one with the smallest distance. Look at all of its outgoing edges and update neighbors if you find a cheaper route.

3. Keep doing that until you reach the end. The first time you reach the end, the distance you get is the shortest possible.

Now look at the graph in the image. The start is 0. A, D, and E get distances 2, 5, and 8. A is the smallest, so expand A. A reaches D with cost 2, so start to A to D is 4, which beats 5, so update D to 4. A also reaches B with cost 1, so start to A to B is 3, which beats infinity, so B becomes 3. A is done.

Unexpanded nodes with known distances are now B (3), D (4), and E (8). B is smallest, so expand B. B reaches the end with cost 1, so start to A to B to end is 4. Dijkstra stops. It reports 4 as the shortest path.

But notice that the algorithm never touches start to E to C to end. That route is 8 + 10 + (−100) which gives −82. That’s way cheaper than 4. Dijkstra never discovers it because the algorithm always expands the node with the smallest known distance so far. It grows outward along the cheapest frontier and assumes later edges only add more cost. That assumption only holds when all edges are non-negative. With a large negative edge, a path that looks expensive at first can collapse to a much smaller value later.

The algorithm is greedy. It keeps pushing outward from the cheapest partial path and won’t explore heavier paths that could pay off with a big negative edge later. This is why Dijkstra breaks on graphs with negative weights.


<img width="259" height="194" alt="image" src="https://github.com/user-attachments/assets/16b80593-b60f-4d84-b519-cda3a7038316" />

The algorithim is "greedy." It isn't looking for diamonds (a negative number that would cancel out the distance), it just wants to find the quickest way out. If it checks a path only to find there are other paths that are still quicker than it, it will abandon that path to check the possible cheaper paths. It always wants the cheapest one until it bumps into the end. This doesn't work if there's a big reward (a negative number) that makes the longer path actually worth it. It jumps to whoever has been acting the most effecient so far. 

Dijkstra's algorithim is greedy in the sense that it moves out from a target and prioritizes looking for the shortest path built so far to test further paths from. 
