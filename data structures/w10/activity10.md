# Activity 10: Graphs
## 1. Create a theoretical graph using a pen and paper OR electronically. (2 points)

Video Explanation: https://youtu.be/BJ6OM87klb8

<img width="2044" height="1237" alt="Untitled" src="https://github.com/user-attachments/assets/f658e4a5-79ab-472f-9666-fe1ac0ad123f" />

## 2. Implement the graph created in step 1 and apply breadth and depth-first search algorithms using C++. (6 points)

Let's try to search from Mark to Kevin. 
```c++
#include <iostream>
using namespace std;

//we have 6 people in the graph.
const int N = 6;

// array stores the names for printing.
string names[N] = {"Mark","Sharon","Omar","Leo","Priya","Kevin"};

//graph matrix stores what is connected to what. 
// graph[a][b] will be 1 if there is a connection (edge) between a and b and 0 if there is no connection.
// initialize to 0 because no edges exist yet.
int graph[N][N] = {0};

// this function creates a directionless edge between two nodes
// Because the graph doesn't have direction, we must set BOTH graph[u][v] and graph[v][u] to 1
// this marks that the two nodes are connected in BOTH directions
void addEdge(int u, int v) {
    graph[u][v] = 1; // mark connection from u to v
    graph[v][u] = 1; // mark connection from v to u
}

//this function prints the path from start to target
//uses  parent[] array, which tells  how we reached each node
// parent[x] stores which node we came from to reach x.
void printPath(int parent[], int start, int target) {
    // If target has no parent and is not the start, then there is no path.
    if (parent[target] == -1 && start != target) {
        cout << "No path\n";
        return;
    }

    // store the path backwards (from target back to start) 
    int path[N];
    int len = 0;
    int cur = target;
   // follow the parent chain until we hit -1, which means we got back to the start 
    while (cur != -1) {
        path[len++] = cur;
        cur = parent[cur];
    }

    // now print the path forward by reversing it 
    for (int i = len - 1; i >= 0; --i) {
        cout << names[path[i]];
        if (i > 0) cout << " -> ";
    }

    cout << '\n';
}

// this is the breadth first search 
//uses a simple array as a queue.
// explores the graph level by level, starting from the start node 
void BFS(int start, int target) {
    // this keeps track of which nodes we have already discovered.
    bool discovered[N] = {false};

    // stores how we reached each node.
    int parent[N];
    for (int i = 0; i < N; ++i) parent[i] = -1;

    // This array acts as a queue.
    int queue[N];
    int front = 0; // index of next item to remove
    int back = 0;  // index of next empty spot to insert into

    // Put the starting node into the queue.
    queue[back++] = start;
    discovered[start] = true; // mark that we discovered the start node

    cout << "BFS order: ";

    // keep looping until the queue becomes empty.
    while (front < back) {
        // take the next node out of the queue.
        int cur = queue[front++];

        // print the current node we are visiting.
        cout << names[cur] << " ";

        // check every possible neighbor of this node.
        // we do this by scanning the adjacency matrix row for 'cur'.
        for (int adj = 0; adj < N; ++adj) {
            // If graph[cur][adj] == 1, then cur is connected to adj.
            // If discovered[adj] is false, then we have not visited adj yet.
            if (graph[cur][adj] == 1 && !discovered[adj]) {
                discovered[adj] = true; // mark adj as discovered
                parent[adj] = cur;      // record how we reached adj
                queue[back++] = adj;    // put adj into the queue
            }
        }
    }

    cout << '\n';

    // now show the actual path BFS found.
    cout << "BFS path from " << names[start] << " to " << names[target] << ": ";
    if (discovered[target]) printPath(parent, start, target);
    else cout << "No path\n";
}

// depth first search
// uses a simple array as a stack like breadth
//it explores as deep as it can go before backtracking 
void DFS(int start, int target) {
    // keep track of visited nodes.
    bool visited[N] = {false};

    // parent array same as BFS.
    int parent[N];
    for (int i = 0; i < N; ++i) parent[i] = -1;

    // simple array stack.
    int stack[N];
    int top = 0; // top of the stack

    // push the starting node on the stack.
    stack[top++] = start;

    cout << "DFS order: ";

    // process the stack until it is empty.
    while (top > 0) {
        // pop the top node from the stack.
        int cur = stack[--top];

        // only process it if we have not visited it yet.
        if (!visited[cur]) {
            visited[cur] = true; // mark visited
            cout << names[cur] << " "; // print visit

            // we now push all neighbors of cur onto the stack.
            //  scan the adjacency matrix row for 'cur'.
            // go backwards so lower numbered nodes get processed first.
            for (int adj = N - 1; adj >= 0; --adj) {
                // If edge and adj is not visited, we want to keep going, add it to stack 
                if (graph[cur][adj] == 1 && !visited[adj]) {
                    // asign a parent only the first time we encounter adj, don't want to backtrack 
                    if (parent[adj] == -1 && adj != start) parent[adj] = cur;
                    stack[top++] = adj; // push onto the stack
                }
            }
        }
    }

    cout << '\n';

    // now show the path found by DFS.
    cout << "DFS path from " << names[start] << " to " << names[target] << ": ";
    if (visited[target]) printPath(parent, start, target);
    else cout << "No path\n";
}

int main() {
    //make  all edges exactly as shown in the picture.
    //each addEdge call connects two nodes in both directions.
    addEdge(0,1); // Mark connected to Sharon
    addEdge(1,2); // Sharon connected to Omar
    addEdge(1,4); // Sharon connected to Priya
    addEdge(2,3); // Omar connected to Leo. Make Leo a 3 so that depth is inclined to traverse it so we can see difference between it and breadth 
    addEdge(4,5); // Priya connected to Kevin

    int start = 0;   // want to see how to get from Mark
    int target = 5;  // to Kevin 

    cout << "Starting from " << names[start] << '\n' << '\n';

    BFS(start, target);
    cout << '\n';
    DFS(start, target);

    return 0;
}

```
## 3. Compare both search algorithms in the context of Big O notations. (2 points)

They are both O(n). Breadth-First Search:
BFS starts at a node and first visits all the nodes that are directly connected to it, then visits all the nodes that are two steps away, then three steps away, and so on. It’s like exploring the graph in “waves,” going level by level. Because it looks at every node and every edge once, its time complexity is O(V + E), and it needs extra space to keep track of which nodes are waiting to be visited, which is O(V).

Depth-First Search:
DFS starts at a node and goes as far as it can down one path before backtracking and trying another path. It’s like following a tunnel to the end, then going back and taking a different tunnel. Just like BFS, it visits each node and edge only once, so the time complexity is also O(V + E). It uses a stack (or recursion) to remember where it is, which means the space it needs is also O(V).

Consider eating this candy.

<img width="800" height="738" alt="image" src="https://github.com/user-attachments/assets/6d7abc1f-1552-430a-888d-7d6c1d983f0a" />

You could consider eating the candy from the highest to lowest, working your way down the layers (breadth), or you could simply eat all the candies of a certain flavor, then return to the top and do it again (similar to depth). 

