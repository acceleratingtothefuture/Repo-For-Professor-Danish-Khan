# Activity 11: Graphs

## Explain with the help of an example "Why Dijkstra's algorithm fails on negative weights".
<img width="1807" height="993" alt="Untitled" src="https://github.com/user-attachments/assets/4551ab9f-9a81-464d-a9b1-d3ea9357653b" />

Suppose we want to get from the start to the end. Dijkstra's algorithim basically says 


"1. initialize everything as infinite distance, the start node is zero distance since you don't need to travel to it.

2. Of the nodes you haven't checked all the paths of, select the one with the smallest distance from the start, and check all of the new paths you can get from it. if the new paths are smaller distances from the exisitng paths, update the path to the newly found smaller path.
  
4. Repeat step 2 until you land at the end. The first time you land at the end, you've found the shortest path. It's the path from the node you got to the end from, and the path deduced to get to that node from the start."

Let's look at the above image. We'll initalize the start to 0 since we don't have to move and everything else will be initialized to infinity. 


The algorithim works by prioritizing whatever the shortest path out from the start is. It always prioritizes checking from the shortest known path to A so far that hasn't been further explored, until it lands at the end. This assumes that each unseen path will only get "longer" (i.e have positive distance needed to travel it) since it is just an extra positive number on top of a path from the start we already discovered. So we are "greedy" and assume it will be worse. But with a negative number, the total distance actually contracts. You could add 5 to your distance for 99 nodes, but if the 99th node is has a -500, that 100 node path is actually a negative distance if you go all the way to the end. 

<img width="259" height="194" alt="image" src="https://github.com/user-attachments/assets/16b80593-b60f-4d84-b519-cda3a7038316" />

The algorithim is "greedy." It doesn't want any rewards along the way. It just wants to get to the end as fast as possible. If it checks a path only to find there are other paths that are still cheaper than it, it will abandon that path to check the possible cheaper paths. It always wants the cheapest one until it bumps into the. This doesn't work if there's a big reward (a negative number) that makes the whole journey worth it. 


Dijkstra's algorithim is greedy in the sense that it moves out from a target and prioritizes looking for the shortest path built so far to test further paths from. 
