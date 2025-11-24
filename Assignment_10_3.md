## Problem Statement
Write a Program to implement Prim's algorithm to find minimum spanning tree of a user defined graph. Use Adjacency List to represent a graph.

##  Code
```cpp
#include <iostream>
#include <vector>
#include <climits>
#include <cstring>
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
        
        Edge_TVV edge1_TVV;
        edge1_TVV.dest_TVV = dest;
        edge1_TVV.weight_TVV = weight;
        adjList_TVV[src].push_back(edge1_TVV);
        
        Edge_TVV edge2_TVV;
        edge2_TVV.dest_TVV = src;
        edge2_TVV.weight_TVV = weight;
        adjList_TVV[dest].push_back(edge2_TVV);
        
        cout << "Edge added between " << vertexLabels_TVV[src] << " and " << vertexLabels_TVV[dest] 
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

    int minKey_TVV(int key[], bool visited[]) {
        int min_TVV = INF_TVV;
        int minIndex_TVV = -1;
        
        for (int v = 0; v < numVertices_TVV; v++) {
            if (!visited[v] && key[v] < min_TVV) {
                min_TVV = key[v];
                minIndex_TVV = v;
            }
        }
        
        return minIndex_TVV;
    }

    void printMST_TVV(int parent[], int key[]) {
        cout << "\n=== Minimum Spanning Tree Edges ===" << endl;
        cout << "Edge\t\tWeight" << endl;
        cout << "----------------------" << endl;
        
        int totalWeight_TVV = 0;
        for (int i = 1; i < numVertices_TVV; i++) {
            cout << vertexLabels_TVV[parent[i]] << " - " << vertexLabels_TVV[i] 
                 << "\t\t" << key[i] << endl;
            totalWeight_TVV += key[i];
        }
        
        cout << "----------------------" << endl;
        cout << "Total Weight: " << totalWeight_TVV << endl;
        cout << "==================================" << endl;
    }

    void primMST_TVV() {
        if (numVertices_TVV == 0) {
            cout << "Graph is empty!" << endl;
            return;
        }
        
        int parent_TVV[MAX_VERTICES_TVV];
        int key_TVV[MAX_VERTICES_TVV];
        bool visited_TVV[MAX_VERTICES_TVV];
        
        for (int i = 0; i < numVertices_TVV; i++) {
            key_TVV[i] = INF_TVV;
            visited_TVV[i] = false;
        }
        
        key_TVV[0] = 0;
        parent_TVV[0] = -1;
        
        cout << "\n=== Prim's Algorithm Execution ===" << endl;
        cout << "Starting from vertex " << vertexLabels_TVV[0] << endl;
        
        for (int count = 0; count < numVertices_TVV - 1; count++) {
            int u = minKey_TVV(key_TVV, visited_TVV);
            
            if (u == -1) break;
            
            visited_TVV[u] = true;
            
            for (int v = 0; v < adjList_TVV[u].size(); v++) {
                int dest = adjList_TVV[u][v].dest_TVV;
                int weight = adjList_TVV[u][v].weight_TVV;
                
                if (!visited_TVV[dest] && weight < key_TVV[dest]) {
                    parent_TVV[dest] = u;
                    key_TVV[dest] = weight;
                }
            }
        }
        
        printMST_TVV(parent_TVV, key_TVV);
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
        cout << "Number of edges: " << (edgeCount/2) << endl;
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
    int choice, vertices, src, dest, weight;

    cout << "=== Prim's Algorithm for Minimum Spanning Tree ===" << endl;
    cout << "Using Adjacency List Representation" << endl;

    while (true) {
        cout << "\n1. Create Graph\n2. Input Graph\n3. Add Edge\n4. Display Adjacency List\n5. Find Minimum Spanning Tree (Prim)\n6. Display Graph Info\n7. Exit\n";
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
                graph_TVV.primMST_TVV();
                break;

            case 6:
                graph_TVV.displayGraphInfo_TVV();
                break;

            case 7:
                cout << "Thank you for using Prim's Algorithm Implementation!" << endl;
                return 0;

            default:
                cout << "Invalid choice!" << endl;
        }
    }

    return 0;
}


##  Output

=== Prim's Algorithm for Minimum Spanning Tree ===
Using Adjacency List Representation

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Prim)
6. Display Graph Info
7. Exit
Enter your choice: 1
Enter number of vertices: 6
Graph created with 6 vertices (labeled A, B, C, ...)

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Prim)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 0
Enter destination vertex: 1
Enter weight: 4
Edge added between A and B with weight 4

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Prim)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 0
Enter destination vertex: 2
Enter weight: 2
Edge added between A and C with weight 2

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Prim)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 1
Enter destination vertex: 2
Enter weight: 1
Edge added between B and C with weight 1

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Prim)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 1
Enter destination vertex: 3
Enter weight: 5
Edge added between B and D with weight 5

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Prim)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 2
Enter destination vertex: 3
Enter weight: 8
Edge added between C and D with weight 8

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Prim)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 2
Enter destination vertex: 4
Enter weight: 10
Edge added between C and E with weight 10

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Prim)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 3
Enter destination vertex: 4
Enter weight: 2
Edge added between D and E with weight 2

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Prim)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 3
Enter destination vertex: 5
Enter weight: 6
Edge added between D and F with weight 6

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Prim)
6. Display Graph Info
7. Exit
Enter your choice: 3
Enter source vertex: 4
Enter destination vertex: 5
Enter weight: 3
Edge added between E and F with weight 3

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Prim)
6. Display Graph Info
7. Exit
Enter your choice: 4

=== Adjacency List Representation ===
A: (B, 4) (C, 2) 
B: (A, 4) (C, 1) (D, 5) 
C: (A, 2) (B, 1) (D, 8) (E, 10) 
D: (B, 5) (C, 8) (E, 2) (F, 6) 
E: (C, 10) (D, 2) (F, 3) 
F: (D, 6) (E, 3) 
===================================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Prim)
6. Display Graph Info
7. Exit
Enter your choice: 5

=== Prim's Algorithm Execution ===
Starting from vertex A

=== Minimum Spanning Tree Edges ===
Edge		Weight
----------------------
A - C		2
B - C		1
B - D		5
D - E		2
E - F		3
----------------------
Total Weight: 13
==================================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Prim)
6. Display Graph Info
7. Exit
Enter your choice: 6

=== Graph Information ===
Number of vertices: 6
Vertices: A B C D E F 
Number of edges: 9
========================

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Prim)
6. Display Graph Info
7. Exit
Enter your choice: 2
Enter number of vertices: 5
Enter number of edges: 7
Enter edges (source destination weight):
Edge 1: 0 1 10
Edge added between A and B with weight 10
Edge 2: 0 2 6
Edge added between A and C with weight 6
Edge 3: 0 3 5
Edge added between A and D with weight 5
Edge 4: 1 3 15
Edge added between B and D with weight 15
Edge 5: 2 3 4
Edge added between C and D with weight 4
Edge 6: 2 4 2
Edge added between C and E with weight 2
Edge 7: 3 4 8
Edge added between D and E with weight 8

1. Create Graph
2. Input Graph
3. Add Edge
4. Display Adjacency List
5. Find Minimum Spanning Tree (Prim)
6. Display Graph Info
7. Exit
Enter your choice: 5

=== Prim's Algorithm Execution ===
Starting from vertex A

=== Minimum Spanning Tree Edges ===
Edge		Weight
----------------------
A - D		5
C - D		4
C - E		2
A - B		10
----------------------
Total Weight: 21
