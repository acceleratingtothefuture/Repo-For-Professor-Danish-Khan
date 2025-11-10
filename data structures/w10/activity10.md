# Activity 10: Graphs
## 1. Create a theoretical graph using a pen and paper OR electronically. (2 points)

<img width="2044" height="1237" alt="Untitled" src="https://github.com/user-attachments/assets/f658e4a5-79ab-472f-9666-fe1ac0ad123f" />

## 2. Implement the graph created in step 1 and apply breadth and depth-first search algorithms using C++. (6 points)

```c++
#include <iostream>
using namespace std;

const int N = 6; // number of nodes
string names[N] = {"Mark","Sharon","Omar","Leo","Priya","Kevin"};

// adjacency matrix
int graph[N][N] = {0};

// add edge (undirected)
void addEdge(int u, int v) {
    graph[u][v] = 1;
    graph[v][u] = 1;
}

// BFS iterative
void BFS(int start) {
    bool discovered[N] = {false};
    int queue[N], front = 0, back = 0;

    queue[back++] = start;
    discovered[start] = true;

    cout << "BFS order: ";
    while(front < back) {
        int current = queue[front++];
        cout << names[current] << " ";
        for(int adj = 0; adj < N; adj++) {
            if(graph[current][adj] == 1 && !discovered[adj]) {
                queue[back++] = adj;
                discovered[adj] = true;
            }
        }
    }
    cout << endl;
}

// DFS iterative
void DFS(int start) {
    bool visited[N] = {false};
    int stack[N], top = 0;

    stack[top++] = start;

    cout << "DFS order: ";
    while(top > 0) {
        int current = stack[--top]; // pop
        if(!visited[current]) {
            cout << names[current] << " ";
            visited[current] = true;
            // push neighbors (reverse order optional)
            for(int adj = N-1; adj >= 0; adj--) {
                if(graph[current][adj] == 1 && !visited[adj]) {
                    stack[top++] = adj;
                }
            }
        }
    }
    cout << endl;
}

int main() {
    // Add edges
    addEdge(0,1); // Mark - Sharon
    addEdge(1,2); // Sharon - Omar
    addEdge(1,4); // Sharon - Priya
    addEdge(2,3); // Omar - Leo
    addEdge(4,5); // Priya - Kevin

    cout << "Starting from Mark:\n";
    BFS(0);
    DFS(0);

    return 0;
}


```
## 3. Compare both search algorithms in the context of Big O notations. (2 points)

They are both O(n). BFS (Breadth-First Search):
BFS starts at a node and first visits all the nodes that are directly connected to it, then visits all the nodes that are two steps away, then three steps away, and so on. It’s like exploring the graph in “waves,” going level by level. Because it looks at every node and every edge once, its time complexity is O(V + E), and it needs extra space to keep track of which nodes are waiting to be visited, which is O(V).

DFS (Depth-First Search):
DFS starts at a node and goes as far as it can down one path before backtracking and trying another path. It’s like following a tunnel to the end, then going back and taking a different tunnel. Just like BFS, it visits each node and edge only once, so the time complexity is also O(V + E). It uses a stack (or recursion) to remember where it is, which means the space it needs is also O(V).
