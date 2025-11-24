
## Problem Statement
Write a Program to implement Dijkstra's algorithm to find shortest distance between two nodes of a user defined graph. Use Adjacency List to represent a graph.

## Code
```cpp
#include <iostream>
#include <vector>
#include <climits>
#include <cstring>
#include <queue>
using namespace std;

const int MAX_VERTICES_TVV = 20;
const int INF_TVV = INT_MAX;

struct Edge_TVV {
    int dest_TVV;
    int weight_TVV;
};

class Graph_TVV {
private:
    vector<Edge_TVV> adjList_TVV[MAX_VERTICES_TVV];
    int numVertices_TVV;
    char vertexLabels_TVV[MAX_VERTICES_TVV];

public:
    Graph_TVV() {
        numVertices_TVV = 0;
        for (int i = 0; i < MAX_VERTICES_TVV; i++) {
            vertexLabels_TVV[i] = 'A' + i;
        }
    }

    void createGraph_TVV(int vertices) {
        if (vertices > MAX_VERTICES_TVV) {
            cout << "Maximum vertices limit exceeded! Max allowed: " << MAX_VERTICES_TVV << endl;
            return;
        }
        
        numVertices_TVV = vertices;
        
        for (int i = 0; i < numVertices_TVV; i++) {
            adjList_TVV[i].clear();
        }
        
        cout << "Graph created with " << vertices << " vertices (labeled A, B, C, ...)" << endl;
    }

    void addEdge_TVV(int src, int dest, int weight) {
        if (src >= numVertices_TVV || dest >= numVertices_TVV || src < 0 || dest < 0) {
            cout << "Invalid vertex! Vertices must be between 0 and " << (numVertices_TVV - 1) << endl;
            return;
        }
        
        Edge_TVV edge_TVV;
        edge_TVV.dest_TVV = dest;
        edge_TVV.weight_TVV = weight;
        adjList_TVV[src].push_back(edge_TVV);
        
        cout << "Directed edge added from " << vertexLabels_TVV[src] << " to " << vertexLabels_TVV[dest] 
             << " with weight " << weight << endl;
    }

    void displayAdjList_TVV() {
        if (numVertices_TVV == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        cout << "\n=== Adjacency List Representation ===" << endl;
        for (int i = 0; i < numVertices_TVV; i++) {
            cout << vertexLabels_TVV[i] << ": ";
            for (int j = 0; j < adjList_TVV[i].size(); j++) {
                cout << "(" << vertexLabels_TVV[adjList_TVV[i][j].dest_TVV] << ", " 
                     << adjList_TVV[i][j].weight_TVV << ") ";
            }
            cout << endl;
        }
        cout << "===================================" << endl;
    }

    void printPath_TVV(int parent[], int dest) {
        if (parent[dest] == -1) {
            cout << vertexLabels_TVV[dest];
            return;
        }
        
        printPath_TVV(parent, parent[dest]);
        cout << " -> " << vertexLabels_TVV[dest];
    }

    void dijkstra_TVV(int src) {
        if (numVertices_TVV == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        if (src >= numVertices_TVV || src < 0) {
            cout << "Invalid source vertex!" << endl;
            return;
        }
        
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq_TVV;
        vector<int> dist_TVV(numVertices_TVV, INF_TVV);
        vector<int> parent_TVV(numVertices_TVV, -1);
        
        pq_TVV.push(make_pair(0, src));
        dist_TVV[src] = 0;
        
        cout << "\n=== Dijkstra's Algorithm Execution ===" << endl;
        cout << "Finding shortest paths from vertex " << vertexLabels_TVV[src] << endl;
        
        while (!pq_TVV.empty()) {
            int u = pq_TVV.top().second;
            pq_TVV.pop();
            
            for (int i = 0; i < adjList_TVV[u].size(); i++) {
                int v = adjList_TVV[u][i].dest_TVV;
                int weight = adjList_TVV[u][i].weight_TVV;
                
                if (dist_TVV[u] + weight < dist_TVV[v]) {
                    dist_TVV[v] = dist_TVV[u] + weight;
                    parent_TVV[v] = u;
                    pq_TVV.push(make_pair(dist_TVV[v], v));
                }
            }
        }
        
        printSolution_TVV(dist_TVV, parent_TVV, src);
    }

    void printSolution_TVV(vector<int>& dist_TVV, vector<int>& parent_TVV, int src) {
        cout << "\n=== Shortest Paths from Vertex " << vertexLabels_TVV[src] << " ===" << endl;
        cout << "Destination\tDistance\tPath" << endl;
        cout << "------------------------------------------------" << endl;
        
        for (int i = 0; i < numVertices_TVV; i++) {
            if (i != src) {
                cout << vertexLabels_TVV[src] << " -> " << vertexLabels_TVV[i] << "\t\t";
                if (dist_TVV[i] == INF_TVV) {
                    cout << "INF\t\t";
                    cout << "No path exists" << endl;
                } else {
                    cout << dist_TVV[i] << "\t\t";
                    printPath_TVV(parent_TVV.data(), i);
                    cout << endl;
                }
            }
        }
        cout << "===============================================" << endl;
    }

    void findShortestPath_TVV(int src, int dest) {
        if (numVertices_TVV == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        if (src >= numVertices_TVV || dest >= numVertices_TVV || src < 0 || dest < 0) {
            cout << "Invalid vertices!" << endl;
            return;
        }
        
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq_TVV;
        vector<int> dist_TVV(numVertices_TVV, INF_TVV);
        vector<int> parent_TVV(numVertices_TVV, -1);
        
        pq_TVV.push(make_pair(0, src));
        dist_TVV[src] = 0;
        
        while (!pq_TVV.empty()) {
            int u = pq_TVV.top().second;
            pq_TVV.pop();
            
            for (int i = 0; i < adjList_TVV[u].size(); i++) {
                int v = adjList_TVV[u][i].dest_TVV;
                int weight = adjList_TVV[u][i].weight_TVV;
                
                if (dist_TVV[u] + weight < dist_TVV[v]) {
                    dist_TVV[v] = dist_TVV[u] + weight;
                    parent_TVV[v] = u;
                    pq_TVV.push(make_pair(dist_TVV[v], v));
                }
            }
        }
        
        cout << "\n=== Shortest Path from " << vertexLabels_TVV[src] << " to " << vertexLabels_TVV[dest] << " ===" << endl;
        if (dist_TVV[dest] == INF_TVV) {
            cout << "No path exists from " << vertexLabels_TVV[src] << " to " << vertexLabels_TVV[dest] << endl;
        } else {
            cout << "Shortest Distance: " << dist_TVV[dest] << endl;
            cout << "Path: ";
            printPath_TVV(parent_TVV.data(), dest);
            cout << endl;
        }
        cout << "========================================================" << endl;
    }

    void displayGraphInfo_TVV() {
        cout << "\n=== Graph Information ===" << endl;
        cout << "Number of vertices: " << numVertices_TVV << endl;
        cout << "Vertices: ";
        for (int i = 0; i < numVertices_TVV; i++) {
            cout << vertexLabels_TVV[i] << " ";
        }
        cout << endl;
        
        int edgeCount = 0;
        for (int i = 0; i < numVertices_TVV; i++) {
            edgeCount += adjList_TVV[i].size();
        }
        cout << "Number of edges: " << edgeCount << endl;
        cout << "========================" << endl;
    }

    void inputGraph_TVV() {
        int vertices, edges, src, dest, weight;
        
        cout << "Enter number of vertices: ";
        cin >> vertices;
        createGraph_TVV(vertices);
        
        cout << "Enter number of edges: ";
        cin >> edges;
        
        cout << "Enter edges (source destination weight):" << endl;
        for (int i = 0; i < edges; i++) {
            cout << "Edge " << (i+1) << ": ";
            cin >> src >> dest >> weight;
            addEdge_TVV(src, dest, weight);
        }
    }
};

int main() {
    Graph_TVV graph_TVV;
    int choice, vertices, src, dest, weight, source;

    cout << "=== Dijkstra's Algorithm for Shortest Path ===" << endl;
    cout << "Using Adjacency List Representation" << endl;

    while (true) {
        cout << "\n1. Create Graph\n2. Input Graph\n3. Add Edge\n4. Display Adjacency List\n5. Find Shortest Paths from Source\n6. Find Shortest Path between Two Nodes\n7. Display Graph Info\n8. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter number of vertices: ";
                cin >> vertices;
                graph_TVV.createGraph_TVV(vertices);
                break;

            case 2:
                graph_TVV.inputGraph_TVV();
                break;

            case 3:
                cout << "Enter source vertex: ";
                cin >> src;
                cout << "Enter destination vertex: ";
                cin >> dest;
                cout << "Enter weight: ";
                cin >> weight;
                graph_TVV.addEdge_TVV(src, dest, weight);
                break;

            case 4:
                graph_TVV.displayAdjList_TVV();
                break;

            case 5:
                cout << "Enter source vertex: ";
                cin >> source;
                graph_TVV.dijkstra_TVV(source);
                break;

            case 6:
                cout << "Enter source vertex: ";
                cin >> src;
                cout << "Enter destination vertex: ";
                cin >> dest;
                graph_TVV.findShortestPath_TVV(src, dest);
                break;

            case 7:
                graph_TVV.displayGraphInfo_TVV();
                break;

            case 8:
                cout << "Thank you for using Dijkstra's Algorithm Implementation!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


## Output

=== Dijkstra's Algorithm for Shortest Path ===
Using Adjacency List Representation

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 1
Enter number of vertices: 6
Graph created with 6 vertices (labeled A, B, C, ...)

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 0
Enter destination vertex: 1
Enter weight: 4
Directed edge added from A to B with weight 4

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 0
Enter destination vertex: 2
Enter weight: 2
Directed edge added from A to C with weight 2

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 1
Enter destination vertex: 2
Enter weight: 1
Directed edge added from B to C with weight 1

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 1
Enter destination vertex: 3
Enter weight: 5
Directed edge added from B to D with weight 5

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 2
Enter destination vertex: 3
Enter weight: 8
Directed edge added from C to D with weight 8

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 2
Enter destination vertex: 4
Enter weight: 10
Directed edge added from C to E with weight 10

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 3
Enter destination vertex: 4
Enter weight: 2
Directed edge added from D to E with weight 2

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 3
Enter destination vertex: 5
Enter weight: 6
Directed edge added from D to F with weight 6

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 3
Enter source vertex: 4
Enter destination vertex: 5
Enter weight: 3
Directed edge added from E to F with weight 3

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 4

=== Adjacency List Representation ===
A: (B, 4) (C, 2) 
B: (C, 1) (D, 5) 
C: (D, 8) (E, 10) 
D: (E, 2) (F, 6) 
E: (F, 3) 
F: 
===================================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 5
Enter source vertex: 0

=== Dijkstra's Algorithm Execution ===
Finding shortest paths from vertex A

=== Shortest Paths from Vertex A ===
Destination	Distance	Path
------------------------------------------------
A -> B		4		A -> B
A -> C		2		A -> C
A -> D		9		A -> B -> D
A -> E		11		A -> B -> D -> E
A -> F		14		A -> B -> D -> E -> F
===============================================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 6
Enter source vertex: 0
Enter destination vertex: 5

=== Shortest Path from A to F ===
Shortest Distance: 14
Path: A -> B -> D -> E -> F
========================================================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 7

=== Graph Information ===
Number of vertices: 6
Vertices: A B C D E F 
Number of edges: 9
========================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 2
Enter number of vertices: 5
Enter number of edges: 8
Enter edges (source destination weight):
Edge 1: 0 1 10
Edge added from A to B with weight 10
Edge 2: 0 2 3
Edge added from A to C with weight 3
Edge 3: 1 2 1
Edge added from B to C with weight 1
Edge 4: 1 3 2
Edge added from B to D with weight 2
Edge 5: 2 1 4
Edge added from C to B with weight 4
Edge 6: 2 3 8
Edge added from C to D with weight 8
Edge 7: 2 4 2
Edge added from C to E with weight 2
Edge 8: 3 4 7
Edge added from D to E with weight 7

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 5
Enter source vertex: 0

=== Dijkstra's Algorithm Execution ===
Finding shortest paths from vertex A

=== Shortest Paths from Vertex A ===
Destination	Distance	Path
------------------------------------------------
A -> B		7		A -> C -> B
A -> C		3		A -> C
A -> D		9		A -> C -> B -> D
A -> E		5		A -> C -> E
===============================================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Shortest Paths from Source
6. Find Shortest Path between Two Nodes
7. Display Graph Info
8. Exit
Enter your choice: 6
Enter source vertex: 0
Enter destination vertex: 3

=== Shortest Path from A to D ===
Shortest Distance: 9
Path: A -> C -> B -> D
========================================================
